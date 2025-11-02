# Buildserver

## Introduction

This buildserver is a compilation of tools to build, test and deliver my side projects.
It runs completely locally in some docker containers using Windows-Subsystem for Linux (WSL2).

## Scope

This buildserver provides following tools:

* Jenkins to run the pipeline.
* Selenium to run UI tests and to do some web scraping.

Other tools like SonarQube etc will be added shortly.

### [Jenkins docker image]()

* [jenkins/jenkins:lts](https://hub.docker.com/layers/jenkins/jenkins/lts/images/sha256-62e89f94ba6848ddf3aa95711174e06a40868208c0a08145cf6fa25a1009c7a5)

### [Selenium docker images]()

* [selenium/hub:4.27](https://hub.docker.com/layers/selenium/hub/4.27/images/sha256-a363a4221fd90cd6c6c5129494631ee7e9fe665ee06bfb39c0b7081ed62d7d21)
* [selenium/node-chrome:4.27](https://hub.docker.com/layers/selenium/node-chrome/4.27/images/sha256-af9c594fdfcdecc6a1772d9a63593db9ba9fd294b17d3b3d73606bb741f28a6e)

### [Postgres docker images](https://hub.docker.com/_/postgres)

* [postgres:16](https://hub.docker.com/layers/library/postgres/16/images/sha256-5193bc608de2df39d5f960895987b8896bb92efbcaf6b2761eaf7e047cc8cdf9)

#### Upgrade Postgres 16 to Postgres 19

    docker run --rm \
	--mount 'type=bind,src=/home/jdufner/postgres/data,dst=/var/lib/postgresql/16/data' \
	--mount 'type=bind,src=/home/jdufner/postgres18/docker,dst=/var/lib/postgresql/18/docker' \
	--env 'PGDATAOLD=/var/lib/postgresql/16/data' \
	--env 'PGDATANEW=/var/lib/postgresql/18/docker' \
	tianon/postgres-upgrade:16-to-18

### [PG Admin docker image](https://hub.docker.com/r/dpage/pgadmin4)

* [dpage/pgadmin4](https://hub.docker.com/layers/dpage/pgadmin4/9/images/sha256-361e14c4d1cc38a1c3a02f762b407b33a92bff3a0d387ed0d1d8333e0bfd0d69)

### [MinIO docker image](https://hub.docker.com/r/minio/minio)

* [minio/minio](https://hub.docker.com/layers/minio/minio/latest/images/sha256-3f97c5651cb6662b880c787a232b6b34fec8d8922e08d6617b25d241a21164bb)

### [Sonarqube docker image](https://hub.docker.com/_/sonarqube)

[sonarqube:lts](https://hub.docker.com/layers/library/sonarqube/lts/images/sha256-efb34aa6f1e106e70ff5e9fb42fa766a2a36087bd7f90b765922939b56a34637)

Host requirements

File system

* /opt/sonarqube/data, UID=1000
* /opt/sonarqube/extensions, UID=1000
* /opt/sonarqube/logs, UID=1000

Database connection

* JDBC_URL
* JDBC_USERNAME
* JDBC_PASSWORD

### [Watchtower docker image]()

* [containrrr/watchtower](https://hub.docker.com/layers/containrrr/watchtower/latest/images/sha256-f9086bfda061100361fc2bacf069585678d760d705cf390918ccdbda8a00980b)

## Usage

### Start all

`docker-compose -f docker-compose.yml up`

### Stop all

`docker-compose -f docker-compose.yml down`

## UI

### Jenkins

    http://localhost:9080/

#### Sonarqube

    http://localhost:9000/

### pgAdmin 4

    http://localhost:9081/browser/

#### MinIO Console

    http://localhost:9090/browser
