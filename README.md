# About this Repo

This is the Git repo of the official Docker image for [SonarQube](https://registry.hub.docker.com/_/sonarqube/). See the Hub page for the full readme on how to use the Docker image and for information regarding contributing and issues.

The full readme is generated over in [docker-library/docs](https://github.com/docker-library/docs), specifically in [docker-library/docs/sonarqube](https://github.com/docker-library/docs/tree/master/sonarqube).

[![Build Status](https://travis-ci.org/SonarSource/docker-sonarqube.svg)](https://travis-ci.org/SonarSource/docker-sonarqube)

### License

Copyright 2015-2019 SonarSource.

Licensed under the [GNU Lesser General Public License, Version 3.0](http://www.gnu.org/licenses/lgpl.txt)

How to use this image
This Docker image contains the Community Edition of SonarQube.

Run SonarQube
The server is started this way:

$ docker run -d --name sonarqube -p 9000:9000 sonarqube
By default you can login as admin with password admin, see authentication documentation.

To analyze a Maven project:

# On Linux:
$ mvn sonar:sonar

# With boot2docker:
$ mvn sonar:sonar -Dsonar.host.url=http://$(boot2docker ip):9000
To analyze other kinds of projects and for more details see Analyzing Source Code documentation.

Advanced configuration
Option 1: Database configuration
By default, the image will use an embedded H2 database that is not suited for production.

The production database is configured with the following SonarQube properties used as environment variables: sonar.jdbc.username, sonar.jdbc.password and sonar.jdbc.url.

$ docker run -d --name sonarqube \
    -p 9000:9000 \
    -e sonar.jdbc.username=sonar \
    -e sonar.jdbc.password=sonar \
    -e sonar.jdbc.url=jdbc:postgresql://localhost/sonar \
    sonarqube
Use of the environment variables SONARQUBE_JDBC_USERNAME, SONARQUBE_JDBC_PASSWORD and SONARQUBE_JDBC_URL is deprecated, and will stop working in future releases.

More recipes can be found here.

Option 2: Use parameters via Docker environment variables
You can pass sonar. configuration properties as Docker environment variables, as demonstrated in the example above for database configuration.

Option 3: Use bind-mounted persistent volumes
The images contain the SonarQube installation at /opt/sonarqube. You can use bind-mounted persistent volumes to override selected files or directories, for example:

sonarqube_conf:/opt/sonarqube/conf: configuration files, such as sonar.properties
sonarqube_data:/opt/sonarqube/data: data files, such as the embedded H2 database and Elasticsearch indexes
sonarqube_logs:/opt/sonarqube/logs
sonarqube_extensions:/opt/sonarqube/extensions: plugins, such as language analyzers
You could also use bind-mounted configurations specified on the command line, for example:

$ docker run -d --name sonarqube \
    -p 9000:9000 \
    -v /path/to/conf:/opt/sonarqube/conf \
    -v /path/to/data:/opt/sonarqube/data \
    -v /path/to/logs:/opt/sonarqube/logs \
    -v /path/to/extensions:/opt/sonarqube/extensions \
    sonarqube
Option 4: Customized image
In some environments, it may make more sense to prepare a custom image containing your configuration. A Dockerfile to achieve this may be as simple as:

FROM sonarqube:7.4-community
COPY sonar.properties /opt/sonarqube/conf/
You could then build and try the image with something like:

$ docker build --tag=sonarqube-custom .
$ docker run -ti sonarqube-custom
Administration
The administration guide can be found here.

License
View license information for the software contained in this image.

As with all Docker images, these likely also contain other software which may be under other licenses (such as Bash, etc from the base distribution, along with any direct or indirect dependencies of the primary software being contained).

Some additional license information which was able to be auto-detected might be found in the repo-info repository's sonarqube/ directory.

As for any pre-built image usage, it is the image user's responsibility to ensure that any use of this image complies with any relevant licenses for all software contained within.
