# Jenkins-Image

## Description

The original Jenkins Docker image with LTS from [https://hub.docker.com/r/jenkins/jenkins/](https://hub.docker.com/r/jenkins/jenkins/).
Its Dockerfile resides at [https://github.com/jenkinsci/docker](https://github.com/jenkinsci/docker).

This image added following tools to Jenkins:

* [Graphviz-2.38.0](https://www.graphviz.org/) `graphviz` version? path?
* [Maven-3.5.4](https://archive.apache.org/dist/maven/maven-3/3.5.4/binaries/) `/opt/maven-3.5.4/bin/mvn`
* [Maven-3.6.3](https://archive.apache.org/dist/maven/maven-3/3.6.3/binaries/) `/opt/maven-3.6.3/bin/mvn`
* [Maven-3.8.8](https://archive.apache.org/dist/maven/maven-3/3.8.8/binaries/) `/opt/maven-3.8.8/bin/mvn`
* [Maven-3.9.9](https://archive.apache.org/dist/maven/maven-3/3.9.9/binaries/) `/opt/maven-3.9.9/bin/mvn`
* OpenJDK-8 [jdk8u422-b05](https://github.com/adoptium/temurin8-binaries/releases) `/opt/jdk-8/bin/java`
* OpenJDK-11 [jdk11.0.24+8](https://github.com/adoptium/temurin11-binaries/releases/) `/opt/jdk-11/bin/java`
* OpenJDK-17 [jdk17.0.9+9.1](https://github.com/adoptium/temurin17-binaries/releases/) `/opt/jdk-17/bin/java`
* OpenJDK-21 [jdk21.0.4+7](https://github.com/adoptium/temurin21-binaries/releases/) `/opt/jdk-21/bin/java`

## Build Docker image

`docker build -t jdufner/jenkins:latest .`

## Clean Docker images

Remove Docker stopped containers:

`docker ps -a | grep Exit | cut -d ' ' -f 1 | xargs sudo docker rm`

Remove Docker images without tag:

`docker images -a | grep "^<none>" | awk "{print \$3}" | xargs sudo docker rmi -f`
