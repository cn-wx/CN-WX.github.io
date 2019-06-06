---
title: Docker learning notes(2)--Install Docker
date: 2018-06-27 22:44:07
tags: [ubuntu,docker]
---
Before Installing docker, we have to install and configure necessary environments.

### Configure Java environment
+ Jdk Download link：
`http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html`
<!-- more -->
+ Install Jdk
```bash
sudo mkdir /opt/java
sudo mv jdk-8u162-linux-x64.tar.gz /opt/java
cd /opt/java/
sudo tar -zxvf *
sudo rm *.tar.gz
```
+ Configure environment variables: `sudo vim ~/.bashrc`
+ Add JAVA_HOME, PATH and CLASSPATH for Jdk：
```bash
export JAVA_HOME=/opt/java/jdk1.8.0_162
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
```
+ Remenber to source the configuration file!
`sudo source ~/.bashrc`
### Configure Maven Environment
+ Maven Download link: `https://maven.apache.org/download.cgi`
+ Install Maven
```bash
sudo mkdir /opt/maven
sudo mv apache-maven3.5.2.tar.gz /opt/maven
cd /opt/maven
sudo tar -zxvf *
sudo rm *.tar.gz
```
+ Configure environment variables: `sudo vim etc/profile`
+ Add M2_HOME, PATH and CLASSPATH for Maven：
```bash
export M2_HOME=/opt/maven/apache-maven-3.5.2
export PATH=$PATH:$M2_HOME/bin
export CLASSPATH=$CLASSPATH:$M2_HOME/lib
```
+ Remenber to source the configuration file: `sudo source etc/profile`

+ Check version for Java and Maven：
```bash
java- version
mvn -v
```
### Install and configure Docker
+ Install docker with apt
```bash
sudo apt-get update
sudo apt-get install docker.io
```
+ In China, the speed of downloading docker image from official library is slow,so I configured a daocloud accelerator for docker:
```bash
sudo curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://ff626ecd.m.daocloud.io
sudo systemctl restart docker.service
```
+ Check docker version: `docker --version`