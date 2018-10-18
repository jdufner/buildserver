FROM jenkins/jenkins:lts
#
# use user if we want to install new software
USER root
#
#RUN apk add --update bash && rm -rf /var/cache/apk/*
#RUN apk add --nocache curl tar bash
#
# Add Maven to /opt/maven-$MAVEN_VERSION
ARG MAVEN_VERSION=3.5.4
RUN mkdir -p /opt/maven-$MAVEN_VERSION && \
curl -fsSL http://apache.osuosl.org/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar -xzC /opt/maven-$MAVEN_VERSION --strip-components=1 && \
ln -s /opt/maven-$MAVEN_VERSION/bin/mvn /usr/bin/mvn
ENV MAVEN_HOME /usr/share/maven-$MAVEN_VERSION
#
# Add OpenJDK-8
# TODO
#
# Add OpenJDK-11
RUN mkdir /opt/jdk-11 && \
curl -fsSL https://download.java.net/java/ga/jdk11/openjdk-11_linux-x64_bin.tar.gz | tar -xzC /opt/jdk-11 --strip-components=1
#
# back to user jenkins
USER jenkins
