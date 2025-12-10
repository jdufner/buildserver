# Buildserver

## Introduction

This buildserver is a compilation of tools to build, test and deliver my side projects.
It runs completely locally in some docker containers using Windows-Subsystem for Linux (WSL2).
It started as buildserver to run Jenkins and other buildtools like Sonarqube, Artifactory etc.
But the scope changed, now it's more a kind of runtime environment to run PostgreSQL, Minio, ELK-Stack etc.

## Scope

This buildserver provides following tools:

* [Elastic](https://www.elastic.co/elasticsearch), [Logstash](https://www.elastic.co/logstash), [Kibana](https://www.elastic.co/kibana) (commonly called ELK stack) to collect, search, analyze, and visualize (log) data.
* [PostgreSQL](https://www.postgresql.org/) and [pgAdmin](https://www.pgadmin.org/) as a relational database management system (RDBMS) and a user interface to administrate the database.
* [Selenium](https://www.selenium.dev/) with [Chrome](https://www.selenium.dev/documentation/webdriver/browsers/chrome/) to run UI tests and to do some web scraping.
* [MinIO](https://www.min.io/) to provide an S3-compatible storage.
* [Jenkins](https://www.jenkins.io/) to run the build pipeline.
* [Watchtower](https://watchtower.nickfedor.com/) to automatically update docker images.

Other tools like SonarQube etc will be added shortly.

## Usage

### Start

To start the all docker containers at once, use following command:

`docker-compose -f docker-compose.yml up` or simply `docker-compose up`

The Docker daemon runs on Linux with root privileges, managed via the 'docker' group.
To run Docker Compose without sudo, you must add your current user to the 'docker' group.

`sudo usermod -aG docker <username>`

This creates a directory called 'docker-data'.
All required directories will be created beneath 'docker-data'.

### Stop

`docker-compose -f docker-compose.yml down` or simply `docker-compose down`

## UI

### Kibana

#### Preconfigured URL

    http://localhost:9220/app/home#/

#### View logging information

To view the logging information Kibana must be configured

1. Go to _Stack Management_ -> _Data Views_
2. Create a new data view with the pattern `docker-logs-*`
3. Go to _Discover_ to view the logs.

### Postgres

#### Mandatory configuration changes

By default, Postgres blocks username / password login from non-localhost.
This can be fixed in `pg_hba.conf`.
Add a line:

    host all all all trust

Probably, there is a line

    host all all all scram-sha-256

That line must be commented or removed.
Refer to [Postgres Client Authentication](https://www.postgresql.org/docs/current/auth-pg-hba-conf.html).

This can be automated by a short script.

    sed -i 's/^\(host\s\+all\s\+all\s\+all\s\+\)scram-sha-256/\1trust/' ./docker-data/postgresql/18/docker/pg_hba.conf

To make it effective either restart Postgres or reload the configuration (`systemctl reload postgresql` or `pg_ctl reload`).

### pgAdmin 4

    http://localhost:9081/browser/

#### MinIO Console

    http://localhost:9090/browser

### Jenkins

    http://localhost:9080/

#### Sonarqube

    http://localhost:9000/

## Implementation

### Logging

This setup sends the logging information to Elastic.

1. The containers write logging information a plain text to `stdout` (console).
2. The Docker daemon (the process `dockerd` on the host) scrapes the output.
3. Logstash provides a Graylog Extended Log Format (GELF) endpoint to receive the logging information.
4. The Docker daemon send the data the the GELF address.

The GELF address is accessible from the host.
So all applications on the host could use it.

To test the logging you could send any message to the GELF address.
Example:

    echo '{"short_message":"Hello World!", "host":"my-pc"}' | nc -u -w 1 127.0.0.1 12201

Open [Kibana](http://localhost:9220/app/home#/) and find the "Hello World!" message.

## References

### [Jenkins](https://www.jenkins.io/)

* [Jenkins Docker image](https://hub.docker.com/r/jenkins/jenkins)
* [jenkins/jenkins:lts](https://hub.docker.com/layers/jenkins/jenkins/lts/images/sha256-62e89f94ba6848ddf3aa95711174e06a40868208c0a08145cf6fa25a1009c7a5)

### [Selenium](https://www.selenium.dev/)

* [Selenium Docker image](https://hub.docker.com/r/selenium/standalone-chrome)
* [selenium/hub:4.27](https://hub.docker.com/layers/selenium/hub/4.27/images/sha256-a363a4221fd90cd6c6c5129494631ee7e9fe665ee06bfb39c0b7081ed62d7d21)
* [selenium/node-chrome:4.27](https://hub.docker.com/layers/selenium/node-chrome/4.27/images/sha256-af9c594fdfcdecc6a1772d9a63593db9ba9fd294b17d3b3d73606bb741f28a6e)

### [PostgreSQL](https://www.postgresql.org/)

* [Postgres Docker image](https://hub.docker.com/_/postgres)
* [postgres:18](https://hub.docker.com/layers/library/postgres/18.0/images/sha256-6b8a8a9f35a6b4ac51e67ca9e73b16fb578155572055dcca32d75d1ed49045fa)

### [pgAdmin](https://www.pgadmin.org/)

* [pgAdmin docker image](https://hub.docker.com/r/dpage/pgadmin4)
* [dpage/pgadmin4](https://hub.docker.com/layers/dpage/pgadmin4/9/images/sha256-361e14c4d1cc38a1c3a02f762b407b33a92bff3a0d387ed0d1d8333e0bfd0d69)

### [MinIO](https://www.min.io/)

* [MinIO docker image](https://hub.docker.com/r/minio/minio)
* [minio/minio](https://hub.docker.com/layers/minio/minio/latest/images/sha256-3f97c5651cb6662b880c787a232b6b34fec8d8922e08d6617b25d241a21164bb)

### [Sonarqube](https://www.sonarsource.com/products/sonarqube/)

* [Sonarqube docker image](https://hub.docker.com/_/sonarqube)
* [sonarqube:lts](https://hub.docker.com/layers/library/sonarqube/lts/images/sha256-efb34aa6f1e106e70ff5e9fb42fa766a2a36087bd7f90b765922939b56a34637)

#### Host requirements

File system

* /opt/sonarqube/data, UID=1000
* /opt/sonarqube/extensions, UID=1000
* /opt/sonarqube/logs, UID=1000

Database connection

* JDBC_URL
* JDBC_USERNAME
* JDBC_PASSWORD

### [Watchtower](https://watchtower.nickfedor.com/v1.12.3/)

* [Nick Fedor's Wattower Docker image](https://hub.docker.com/r/nickfedor/watchtower)
* [nickfedor/watchtower:latest](https://hub.docker.com/layers/nickfedor/watchtower/latest/images/sha256-6cbf78b4f76e70e41c1fcc2feb1ecee84316f5c1a71a9dfa39f9969376c246f8)
