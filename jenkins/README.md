Jenkins-Image
=============

Description
-----------

The original Jenkins Docker image with LTS from https://hub.docker.com/r/jenkins/jenkins/ . 
Its Dockerfile resists at https://github.com/jenkinsci/docker .
The Docker image contains nothing else than JDK 8 and Jenkins.
Therefore I add additional programs. In detail they are:

* Maven-3.5.4 https://maven.apache.org/
* JDK-11.0.0 https://jdk.java.net/11/
* NodeJS-8 https://nodejs.org/en/
* Angular CLI https://cli.angular.io/
* Graphviz-2.38.0 https://www.graphviz.org/


Build Docker image
------------------

`docker build -t jdufner/jenkins:v3 .`


Clean Docker images
-------------------

Remove Docker stopped containers:

`docker ps -a | grep Exit | cut -d ' ' -f 1 | xargs sudo docker rm` 

Remove Docker images without tag:

`docker images -a | grep "^<none>" | awk "{print \$3}" | xargs sudo docker rmi -f`
