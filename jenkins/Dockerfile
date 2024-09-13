FROM jenkins/jenkins:lts-jdk17
#
# use user if we want to install new software
USER root
#
# Language / Locale
#RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
#    locale-gen
#ENV LANG en_US.UTF-8
#ENV LANGUAGE en_US:en
#ENV LC_ALL en_US.UTF-8
#
# Add Graphviz
RUN apt-get -y update
RUN apt-get install -y graphviz
RUN dot -version
#
# Add Maven to /opt/maven-$MAVEN_VERSION
ARG MAVEN_VERSION=3.5.4
RUN mkdir -p /opt/maven-$MAVEN_VERSION && \
curl -fsSL https://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar -xzC /opt/maven-$MAVEN_VERSION --strip-components=1
#
# Add Maven to /opt/maven-$MAVEN_VERSION
ARG MAVEN_VERSION=3.6.3
RUN mkdir -p /opt/maven-$MAVEN_VERSION && \
curl -fsSL https://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar -xzC /opt/maven-$MAVEN_VERSION --strip-components=1
#
# Add Maven to /opt/maven-$MAVEN_VERSION
ARG MAVEN_VERSION=3.8.8
RUN mkdir -p /opt/maven-$MAVEN_VERSION && \
curl -fsSL https://archive.apache.org/dist/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar -xzC /opt/maven-$MAVEN_VERSION --strip-components=1
#
# Add Maven to /opt/maven-$MAVEN_VERSION
ARG MAVEN_VERSION=3.9.9
RUN mkdir -p /opt/maven-$MAVEN_VERSION && \
curl -fsSL http://apache.osuosl.org/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar -xzC /opt/maven-$MAVEN_VERSION --strip-components=1 && \
ln -s /opt/maven-$MAVEN_VERSION/bin/mvn /usr/bin/mvn
RUN mvn -version
ENV MAVEN_HOME /usr/share/maven-$MAVEN_VERSION
#
#
# Add OpenJDK-8
RUN mkdir /opt/jdk-8 && \
curl -fsSL https://github.com/adoptium/temurin8-binaries/releases/download/jdk8u422-b05/OpenJDK8U-jdk_x64_linux_hotspot_8u422b05.tar.gz | tar -xzC /opt/jdk-8 --strip-components=1
RUN /opt/jdk-8/bin/java -version
#
# Add OpenJDK-11
RUN mkdir /opt/jdk-11 && \
curl -fsSL https://github.com/adoptium/temurin11-binaries/releases/download/jdk-11.0.24%2B8/OpenJDK11U-jdk_x64_linux_hotspot_11.0.24_8.tar.gz | tar -xzC /opt/jdk-11 --strip-components=1
RUN /opt/jdk-11/bin/java -version
#
# Add OpenJDK-17
RUN mkdir /opt/jdk-17 && \
curl -fsSL https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.12%2B7/OpenJDK17U-jdk_x64_linux_hotspot_17.0.12_7.tar.gz | tar -xzC /opt/jdk-17 --strip-components=1
RUN /opt/jdk-17/bin/java -version
#
# Add OpenJDK-21
RUN mkdir /opt/jdk-21 && \
curl -fsSL https://github.com/adoptium/temurin21-binaries/releases/download/jdk-21.0.4%2B7/OpenJDK21U-jdk_x64_linux_hotspot_21.0.4_7.tar.gz | tar -xzC /opt/jdk-21 --strip-components=1
RUN /opt/jdk-21/bin/java -version
#
#
# back to user jenkins
USER jenkins
#
# Jenkins Settings
# HTML Publisher plugin should display CSS in correct way
ENV JAVA_OPTS "-Dhudson.model.DirectoryBrowserSupport.CSP="
