Jenkins-Image
=============

Description
-----------

The original Jenkins Docker image with LTS from https://hub.docker.com/r/jenkins/jenkins/ . 
Its Dockerfile resists at https://github.com/jenkinsci/docker .
The Docker image contains nothing else than JDK 8 and Jenkins.
Therefore I add additional programs. In detail they are:

* Graphviz-2.38.0 https://www.graphviz.org/
    `graphviz` path?
* Maven-3.5.4 https://maven.apache.org/
    `/opt/maven-3.5.4/bin/mvn`
* Maven-3.6.0 https://maven.apache.org/
    `/opt/maven-3.6.0/bin/mvn`
* JDK-11.0.1 https://jdk.java.net/11/
    `/opt/jdk-11/bin/java`
* NodeJS-8 https://nodejs.org/en/
    `node` path?
    `npm` path?
* Angular CLI https://cli.angular.io/
    `/usr/bin/ng`

Other Executables
-----------------
* JDK-8
    `/usr/lib/jvm/java-8-openjdk-amd64/jre/java`

Build Docker image
------------------

`docker build -t jdufner/jenkins:latest .`


Clean Docker images
-------------------

Remove Docker stopped containers:

`docker ps -a | grep Exit | cut -d ' ' -f 1 | xargs sudo docker rm` 

Remove Docker images without tag:

`docker images -a | grep "^<none>" | awk "{print \$3}" | xargs sudo docker rmi -f`
