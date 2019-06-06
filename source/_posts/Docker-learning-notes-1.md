---
title: Docker learning notes(1)--Docker Intoduction
date: 2018-06-26 21:44:07
tags: [docker]
---
## Introduction：
+ Series of **Docker learning notes** are based on:  
OS：Ubuntu 16.04 LTS  
Java version：Jdk 1.8。  
<!-- more -->
### What is Docker?
![docker](../../../../images/docker/docker.jpg)
+ Docker's idea comes from cargo containers. What problem can cargo containers solve? For example, goods can be put in order on a large ship and all kinds of goods are standardized by containers, containers and containers do not interact with each other so that we don't need a special fruit ship or a chemical carrier. As long as these goods are properly packaged in containers, we can transport them with one large ship. Docker is a similar idea like this. Cloud computing is now very popular, and cloud computing is like a big cargo ship while docker is a container.

1>&nbsp;Different applications may have different application environments, for example, dependencies for websites developed by .Net and by PHP are not the same. If install their dependencies on one server, debugging will take a long time and dependencies might be conflict to each other. For example, IIS and Apache access port conflicts. At this point, you have to isolate the website developed by .Net and the website developed by PHP. Traditionally, we can create different virtual machines on the server and put different applications on different virtual machines, but the cost of virtual machines is relatively high. Docker can realize the function of virtual machine isolation application environment, and it costs less than virtual machine.

2>&nbsp;When you develop the software on Ubuntu, but the operation and maintenance management is on CentOS, the operation and maintenance of your software from the development environment to the production environment will encounter some Ubuntu to CentOS problems, such as: a special version of the database only supported on Ubuntu but not on CentOS, so that you have to solve such problems during the process of transfer between operations. But if you use docker, you can directly transfer the development environment to operation and deploy by docker directly and faster than regular deployment.

3>&nbsp;On the server load, if you use virtual machines instead of docker, free memory will not be used.

### Life cycle of docker:（image, container, registry）

#### 1.docker image
+ Docker image is actually made up of a file system, which is called UnionFS. At the bottom of the Docker image is bootfs. This layer is the same as our typical Linux/Unix system, including boot loader and kernel. When the boot is loaded, the kernel is all in memory. At this point, the right to use memory is transferred from bootfs to the kernel, and the system will also unload bootfs. The first layer of Docker on bootfs is rootfs (root file system). Rootfs is the distribution of different operating systems, such as Ubuntu, Centos and so on.
![docker-image](../../../../images/docker/docker-image.png)
#### 2.docker container
+ <code>docker run</code>&nbsp;Instantiate an image to a container, similar to "new" in java.<br>
+ Some parameters for <code>docker run</code>：<br>
`-d` : Run container in background<br>
`-e` : Set environment variables<br>
`-p` : Host port:Container port <br>
`--name` : Set name for container

#### 3.docker registry
+ A place to store and share docker images.<br>
eg., Docker Hub: &nbsp;[hub.docker.com](https://hub.docker.com)

### Deploying a web application with docker
#### 1.pull source code to host
Subversion: <code>svn checkout svn://svn_address [host_volume]</code><br>
Git:<code>git clone [.git file]</code>

#### 2.Packaging .jar/.war file through Maven/ANT/Other 
`mvn package` / `ant makewar` / etc.

#### 3.Write a Dockerfile
```bash
FROM [imageName：tag]
MAINTAINER [author] [contact_e-mail]
ADD [.jar file] [volume_in_docker_container]
RUN bash -c 'touch /opt/xxx.jar'
#Change timezone
RUN echo "Asia/Shanghai" > /etc/timezone && dpkg-reconfigure -f noninteractive tzdata
#expose default port [8080_for_tomcat]
EXPOSE 8080
#execute .war/.jar file
CMD ["catalina.sh","run"]       --for xxx.war
CMD ["java","-jar","/opt/xxx.jar"]   --for xxx.jar
```

#### 4.Docker build
+ `docker build -t [imageName] .`  
_DO NOT FORGET_ the `.` at the back.

#### 5.Docker run
+ `docker run -d -p [port]:[port in container]`

### Other commands for docker
`sudo docker ps -a` : List all containers  
`sudo docker images` : List all images
`sudo docker images|grep test|awk '{print $3 }'|xargs sudo docker rmi` : remove all images that contain 'test'
