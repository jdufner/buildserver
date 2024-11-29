# Buildserver

## Introduction

This buildserver is a compilation of tools to build, test and deliver my side projects.
It runs completely locally in some docker containers using Windows-Subsystem for Linux (WSL2).

## Scope

This buildserver provides following tools:

* Jenkins to run the pipeline.
* Selenium to run UI tests and to do some web scraping.

Other tools like SonarQube etc will be added shortly.

### Jenkins

Used docker image:

* [jenkins/jenkins:lts](https://hub.docker.com/layers/jenkins/jenkins/lts/images/sha256-62e89f94ba6848ddf3aa95711174e06a40868208c0a08145cf6fa25a1009c7a5)

### Selenium

Used docker images:

* [selenium/hub:4.27](https://hub.docker.com/layers/selenium/hub/4.27/images/sha256-a363a4221fd90cd6c6c5129494631ee7e9fe665ee06bfb39c0b7081ed62d7d21)
* [selenium/node-chrome:4.27](https://hub.docker.com/layers/selenium/node-chrome/4.27/images/sha256-af9c594fdfcdecc6a1772d9a63593db9ba9fd294b17d3b3d73606bb741f28a6e)

### Watchtower

Used docker image:

* [containrrr/watchtower](https://hub.docker.com/layers/containrrr/watchtower/latest/images/sha256-f9086bfda061100361fc2bacf069585678d760d705cf390918ccdbda8a00980b)

## Usage

### Start all

`docker-compose -f docker-compose.yml up`

### Stop all

`docker-compose -f docker-compose.yml down`

## Jenkins

### Start Jenkins

`docker-compose -f docker-compose-jenkins.yml up`

### Stop Jenkins

`docker-compose -f docker-compose-jenkins.yml down`
