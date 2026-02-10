# Docker & Containerization – Theory


## commands used ( practiced in class )
These screenshots contain practice commands executed during learning Docker and containerization such as:

- Checking Docker version  
- Pulling images  
- Running containers  
- Listing containers and images  
- Building Docker images  
- Saving and loading images  

### Command History Screenshots

![Practice 1](PracticeCommands/history1.png)

![Practice 2](PracticeCommands/history2.png)

![Practice 3](PracticeCommands/history3.png)



# 27th and 28th January
- Started an Ubuntu container using Docker and installed OpenJDK inside the container.

![Java Install](27thJan/Screenshot%20(34).png)

- Created a simple Java program (Hello.java), compiled it using javac, and executed it inside a Docker container.

![Java Run](27thJan/Screenshot%20(35).png)

- Converted a running container into a Docker image, viewed Docker images, ran a container from the created image, and saved/loaded the image as a tar file.

![Docker Image](27thJan/Screenshot%20(36).png)



- created a Dockerfile for a Java application using Ubuntu as base image.
- built a Docker image from the Dockerfile.
- verified successful image creation using docker images.
- understood Docker build steps (FROM, RUN, WORKDIR, COPY, CMD).

![Docker Build Process](28thJan/1.jpeg)

![Image Created](28thJan/2.jpeg)



# 29th January
- practiced Markdown syntax for documentation using online tutorial.
- created headings, subheadings, bold and italic text.
- created ordered and unordered lists.
- inserted links and images using markdown format.
- practiced writing code blocks and inline code.
- learned how to structure README files properly.

Reference:
https://www.markdowntutorial.com/


# 30th january
![Java Run](30thjan/1.jpeg)

![Java Run](30thjan/2.jpeg)

![Java Run](30thjan/3.jpeg)

![Java Run](30thjan/4.png)

![Java Run](30thjan/5.png)

![Java Run](30thjan/6.jpeg)


# 3rd February
## Docker Engine API – Hands-On (WSL)
### Overview
Studied how Docker CLI commands internally communicate with the Docker daemon using the Docker Engine REST API through a Unix socket.

### What I Learned
- Docker CLI works as a client for Docker Engine API  
- Docker API uses REST endpoints  
- Docker socket path: `/var/run/docker.sock`  
- API is versioned (example: v1.44)

### Hands-On Activities

- Verified Docker API socket and connectivity 
- Retrieved Docker version using API
  ![Java Run](3rdfeb/1.png)
  
- Listed running and all containers using API
  ![Java Run](3rdfeb/2.png)
  
- Pulled nginx image using API
  ![Java Run](3rdfeb/4-1.png)
  
  ![Java Run](3rdfeb/4-2.png)
  
- Created container using JSON request and Started, stopped, and restarted container using API
  ![Java Run](3rdfeb/stsrtstop.jpeg)
  
- Inspected container details
  ![Java Run](3rdfeb/inspect.png)
  
- Viewed container logs using API
  ![Java Run](3rdfeb/logs1.png)

  ![Java Run](3rdfeb/logs2.png)
  
- Executed commands inside container using API
  ![Java Run](3rdfeb/exec1.png)

  ![Java Run](3rdfeb/exec2.png)
  
- Removed containers and images using API
- Checked Docker system info and container stats
  ![Java Run](3rdfeb/remove.png)
  
  ![Java Run](3rdfeb/imageinfo.png) 

# 4TH february
## Docker Engine API over TCP (WSL)

### Overview
Enabled Docker daemon to listen on TCP port **2375** and accessed Docker Engine API from browser and terminal.

### What I Did
- Edited Docker daemon configuration to enable TCP host.
- Restarted Docker service and manually started dockerd.
- Verified Docker API is listening on port 2375.
- Opened Docker API in browser.
- Fetched Docker version using HTTP API.

### What I Learned
- Docker daemon can expose REST API over TCP.
- Docker API works without Docker CLI.
- API version and engine details can be fetched using HTTP.

![Java Run](4thfeb/1.png) 

![Java Run](4thfeb/2.png) 

![Java Run](4thfeb/3.png) 

![Java Run](4thfeb/4.png) 

# 5th february
## practice test 

Class Test: Containerize a Python Application (Containerization Focus)
Objective
The goal of this hands-on is to evaluate your understanding of containerization concepts:

Base images
Dependency installation
Image naming and tagging
Container execution and testing

![Java Run](5thfeb/1.png) 

![Java Run](5thfeb/2.png) 

![Java Run](5thfeb/3.png) 

![Java Run](5thfeb/4.png) 

![Java Run](5thfeb/5.png) 

![Java Run](5thfeb/6.png) 


# 6th february
![Java Run](6thfeb/1.png) 

![Java Run](6thfeb/2.png) 

![Java Run](6thfeb/3.png) 

![Java Run](6thfeb/4.png) 

![Java Run](6thfeb/5.png)

## Assignment : using C programming 
![Java Run](6thfeb/11.png) 

![Java Run](6thfeb/12.png) 

![Java Run](6thfeb/13.png) 

![Java Run](6thfeb/14.png)


# 10th february
previously we did using single stage , now using multi stage 
For C file:

![Java Run](10thfeb/1.png)

![Java Run](10thfeb/2.png)

![Java Run](10thfeb/3.png)


for JAVA file:

![Java Run](10thfeb/4.png)

![Java Run](10thfeb/5.png)

![Java Run](10thfeb/6.png)

![Java Run](10thfeb/7.png)

Result: Image size reduced from ~150MB to ~1MB!









