FROM jenkins/jenkins:lts
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
# Add Maven to /opt/maven-$MAVEN_VERSION
ARG MAVEN_VERSION=3.5.4
RUN mkdir -p /opt/maven-$MAVEN_VERSION && \
curl -fsSL http://apache.osuosl.org/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz | tar -xzC /opt/maven-$MAVEN_VERSION --strip-components=1 && \
ln -s /opt/maven-$MAVEN_VERSION/bin/mvn /usr/bin/mvn
RUN mvn -version
ENV MAVEN_HOME /usr/share/maven-$MAVEN_VERSION
#
# OpenJDK-8 is already installed
# /usr/lib/jvm/java-8-openjdk-amd64/jre/java -version
#
# Add OpenJDK-11
RUN mkdir /opt/jdk-11 && \
curl -fsSL https://download.java.net/java/ga/jdk11/openjdk-11_linux-x64_bin.tar.gz | tar -xzC /opt/jdk-11 --strip-components=1
RUN /opt/jdk-11/bin/java -version
#
# Add Node
RUN apt-get -y update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends apt-utils
RUN apt-get install -y curl
RUN curl --silent --location https://deb.nodesource.com/setup_8.x | bash -
RUN apt-get install -y nodejs
RUN apt-get install -y build-essential
RUN node -v
RUN npm -v
#
# Add Angular CLI
RUN npm install -g @angular/cli
RUN ng --version
#
# Add Graphviz
RUN apt-get -y update
RUN apt-get install -y graphviz
RUN dot -version
#
# back to user jenkins
USER jenkins
#
# Jenkins Settings
# HTML Publisher plugin should display CSS in correct way
ENV JAVA_OPTS "-Dhudson.model.DirectoryBrowserSupport.CSP="
