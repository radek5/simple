Simple DevOps Project

1. 	Launch an EC2 instance for Docker host

2.  Install Git
    yum install -y git
    
3.	Install docker on EC2 instance and start services
	  yum install -y docker
	  systemctl start docker
	
4.	Create new user for Docker management & add to Docker (default) group
    useradd dockeradmin -p "password"
	  usermod -aG docker dockeradmin
	
5. 	Create Directories
	  mkdir data
	  cd data/
	  mkdir docker
	  cd docker/
	  mkdir simple
	  cd simple/
   
6.	Create Dockerfile - /data/docker/simple
	  vim Dockerfile
	
	  # Pull Image
	  From tomcat:8-jre8

	  # Maintainer
	  MAINTAINER "ralph.adekoya@sofology.co.uk"

	  # Copy war
	  COPY ./webapp.war /usr/local/tomcat/webapps
	
7.	Login to Jenkins
	  Manage Jenkins --> Configure system --> Publish over SSH --> add Docker server and credentials

8. 	Create Jenkins Job

    A. Source Code Management
	     Repository: https://github.com/radek5/simple.git
	     Branches to build: */master
	
    B. Build Root POM: pom.xml
	     Goals and options: Clean install package
	
    C. Send files or execute commands over SSH Name: DOCKER_HOST
	     Source files: webapp/target/*.war
	     Remove prefix: webapp/target/
	     Remote directory: /data/docker
	     Exec command: docker stop simple_demo; docker rm -f simple_demo; docker image rm -f simple_demo; cd /data/docker; docker build -t simple_demo
	
    D. Send files or execute commands over SSJ
       Name: docker_host
	     Exec command: docker run -d --name simple_demo -p 8080:8080 simple_demo 
	
9.	Login to docker host, check images and containers (No images and containers)

10.	Execute Jenkins job

11.	Check images and containers on docker host (This time an image and container is created)

12.	Access web application - docker_host_ip:8080


	
