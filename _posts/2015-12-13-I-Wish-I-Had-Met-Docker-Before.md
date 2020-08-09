---
layout: post
title: I Wish I Had Met Docker Before
date: '2015-12-13T04:41:00.001-08:00'
author: Ivan
tags:
- Backend
modified_time: '2015-12-18T17:25:14.921-08:00'
blogger_id: tag:blogger.com,1999:blog-415098333644749081.post-2499395070133616141
blogger_orig_url: http://invictuscode.blogspot.com/2015/12/hello.html
---

I started to dig into Docker few days ago, and it totally blew my mind. It make building system environments and deploying server a lot more easier than not using it. Since I worked on backend server development few years ago, I spent most of my time on building server environment and deploying server. Also, configuring server sometimes could be extremely painful because there are different kinds of OS you have to deal with. For example, if it’s on Windows, you have to use configuration for Windows. If it’s on Linux, you have to use configuration for Linux. Event you finished writing those scripts, configuration files or property files to make the process, you still have to switch and adjust them if needed. Docker can make those things easier. For now, I only use Docker to build images and run them on container, but I believe I’m gonna use it for more advanced uses.  
Before you started to dive into wonderful Docker world, you can check on offical docs to understand what exactly Docker is. It’s easier for you to use it, and it’s very detailed.  
[https://docs.docker.com/mac/step\_two/](https://docs.docker.com/mac/step_two/)  

* * *

Install Docker
--------------

### Ubuntu

    sudo apt-key adv --keyserver hkp://pgp.mit.edu:80 --    recv-keys 58118E89F3A912897C070ADBF76221572C52609Dsudo add-apt-repository "deb https://apt.dockerproject.org/repo ubuntu-$(lsb_release -s -c) main"sudo apt-get updatesudo apt-get install docker-engine

### Mac Or Windows

Download it from the website and install it.  
[https://www.docker.com/docker-toolbox](https://www.docker.com/docker-toolbox)  

Test Docker Installation
------------------------

### Check installed version

    docker -v

### Run Sample

    docker run Hello-world

“docker run” runs a image on docker  

Some constantly used commands
-----------------------------

### See Docker process

    docker ps -a

### Check Docker images

    docker images

### Remove Docker images

    docker rmi 2e8

2e8 is image id in docker process. Also, you can use image name + tag.  

### Stop Docker process

    docker stop 2e8

### Stop and remove all containers and images

if you’re tired of stoping and removing all containers and images one by one, here’s the command to stop and remove all of them.  

    docker stop $(docker ps  -a -q)docker rm $(docker ps -a -q)

### Dockerfile

Create a dockerfile to build your own image  
tomcat7:jdk7 is image name + tag.  

    docker build -t tomcat7:jdk7 .

### Dockerfile Example

This example shows you how to write a dockefile to make a tomcat image  

    FROM sshd:ubuntuMAINTAINER Ivan.W from -----#ENV DEBIAN_FRONTEND noninteractiveRUN echo "Asia/Taipei" >/etc/timezone && \dpkg-reconfigure -f noninteractive tzdataRUN apt-get install -yq --no-install-recommends wget pwgen ca-certificates && \    apt-get clean && \    rm -rf /var/lib/apt/lists/*ENV CATALINA_HOME /tomcatENV JAVA_HOME /jdkENV JRE_HOME /jdkADD apache-tomcat-7.0.47 /tomcatADD jdk /jdkADD run.sh /run.shRUN chmod +x /*.shRUN chmod +x /tomcat/bin/*.shVOLUME ["/opt/webapps", "apache-tomcat-7.0.47/webapps"]EXPOSE 8080CMD ["/run.sh"]

### Enable docker-compose command

    sudo curl -L https://github.com/docker/compose/releases/download/1.4.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-composesudo chmod +x /usr/local/bin/docker-compose

### What is docker-compose

“docker-compose” allows you to run different images on different containers all at once, what’s more, you can define environment variables, links, volumes, and many other things inside a yml file, and run docker-compose with this yml file.  

### Docker Compose File Example

compose.yml  

    tomcat:  container_name: tomcat  image: tomcat7:jdk7  links:    - db  volumes:    - /opt/webapps:/tomcat/webapps  ports:    - "8080:8080"    - "9002:22"db:  container_name: db  image: mysql:5.6  volumes:    - /opt/mysqldb:/var/lib/mysql  ports:    - "9006:3006"    - "9003:22"

“image” assign the base image that you want to put in container and run it with docker-compose.  
“volumes” allows you to link your machines’ directories to the directories of your containers. The front directory params is your local directory path, and the back directory params is the directory path inside your container.  
“ports” can link your local ports to the ports in containers.  
“links” is a way to link containers and let containers connect with each other.  

### Run Docker-Compose

    docker-compose -f compose.yml up -d

\-f : use yml file to run docker-compose  
\-d : run in background  
Using docker-compose, you can directly run many containers all at once without typing complex command of “run”, and the advantage is that yml file is easier to read when you want to maintain your containers.  

### Stop Docker-Compose

    docker-compose -f compose.yml stop