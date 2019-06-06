---
title: Docker learning notes(3)--Use Jenkins for CI/CD
date: 2018-06-28 14:25:58
tags: [docker,jenkins]
---
### Why choose Jenkins?
Jenkins is an extensible continuous integration engine, mainly used in build / test software projects continuously and automatically and monitoring some of the tasks that are executed on time. So it can ensure developers and related personnel save time and effort to improve development efficiency.
<!-- more -->

### Install Jenkins
+ Download and install Jenkins
```bash
wget -q -O - https://pkg.jenkins.io/debian/jenkins-ci.org.key | sudo apt-key add -  
sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update  
sudo apt-get install jenkins
```
+ Visit [localhost:8080](http://localhost:8080) for Jenkins home page
Initial password for admin: <code> vim /var/lib/jenkins/secrets/initialAdminPassword</code>
