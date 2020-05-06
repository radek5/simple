README.md

Simple DevOps Project

1. 	Launch an EC2 instance for Docker host

2.	Install docker on EC2 instance and start services
	yum install -y docker
	systemctl start docker
	
3.	Create new user for Docker management & add to Docker (default) group
        useradd dockeradmin -p "password"
	usermod -aG docker dockeradmin
	
4. 	Create Directories
	mkdir data
	cd data/
	mkdir docker
	cd docker/
	mkdir simple
	cd simple/
   
5.	Create Dockerfile - /data/docker/simple
	vim Dockerfile
	
	# Pull Image
	From tomcat:8-jre8

	# Maintainer
	MAINTAINER "ralph.adekoya@sofology.co.uk"

	# Copy war
	COPY ./webapp.war /usr/local/tomcat/webapps
	
6.	Login to Jenkins
	Manage Jenkins --> Configure system --> Publish over SSH --> add Docker server and credentials

7. 	Create Jenkins Job

A.	Source Code Management
	Repository: https://github.com/radek5/simple.git
	Branches to build: */master
	
B.	Build Root POM: pom.xml
	Goals and options: Clean install package
	
C.	send files or execute commands over SSH Name: DOCKER_HOST
	Source files: webapp/target/*.war
	Remove prefix: webapp/target/
	Remote directory: /data/docker
	Exec command: docker stop simple_demo; docker rm -f simple_demo; docker image rm -f simple_demo; cd /data/docker; 
	docker build -t simple_demo
	
D.	Send files or execute commands over SSJ
        Name; docker_host
	Exec command: docker run -d --name simple_demo -p 8080:8080 simple_demo 
	
8.	Login to docker host, check images and containers (No images and containers)

9.	Execute Jenkins job

10.	Check images and containers on docker host (This time an image and container is created)

11.	Access web application - docker_host_ip:8080


	
