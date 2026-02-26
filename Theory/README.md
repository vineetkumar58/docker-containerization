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
### For C file:

![Java Run](10thfeb/1.png)

![Java Run](10thfeb/2.png)

![Java Run](10thfeb/3.png)



### for JAVA file(without multistage) :

![Java Run](10thfeb/4.png)

![Java Run](10thfeb/5.png)

![Java Run](10thfeb/6.png)

![Java Run](10thfeb/7.png)

### for JAVA file(with multistage) :

![Java Run](10thfeb/8.png)

![Java Run](10thfeb/9.png)

![Java Run](10thfeb/10.png)

![Java Run](10thfeb/11.png)

#### comparing image size:

![Java Run](10thfeb/12.png)

### Result: Image size reduced  to ~1MB!


# 11th february

![Java Run](11thfeb/persistantvolume.png)

![Java Run](11thfeb/1.png)


# 12th february
![Java Run](12thfeb/1.jpeg)

# 18th february

## Docker Networking
![Java Run](18thfeb/1.jpeg)

![Java Run](18thfeb/2.jpeg)

![Java Run](18thfeb/3.jpeg)

![Java Run](18thfeb/4.jpeg)

![Java Run](18thfeb/5.jpeg)

![Java Run](18thfeb/6.jpeg)


# 20th febraury
# Docker Networking – Overlay Network (Swarm)

## What is Overlay Network
Overlay network allows containers running on different Docker hosts to communicate as if they are on the same local network.
Works using Docker Swarm.

## Prerequisites
- Docker installed
- Swarm mode enabled
- (Real multi-host) Open ports:
  - 2377/tcp → Swarm management
  - 7946/tcp/udp → Node communication
  - 4789/udp → Overlay traffic

## Single Machine Lab (Learning Purpose)

### Step 1 — Initialize Swarm
### Step 2 — Create Overlay Network
### Step 3 — Run Containers
```bash
docker swarm init --advertise-addr 127.0.0.1
docker network create -d overlay --attachable my_overlay

docker run -d --network my_overlay --name app1 alpine sleep 3600
docker run -d --network my_overlay --name app2 alpine sleep 3600

docker exec app1 ping app2
```
![Java Run](20thfeb/1.jpeg)

### Result: Containers communicate using container name (built-in DNS works)


## Real Multi-Host Setup
### MACVLAN LAB
```bash
# Shows join command for workers
docker swarm join-token worker
# Step 1: Find your network details
# On Linux:
ip route show default
# Shows: default via 192.168.1.1 dev eth0
# Interface: eth0, Gateway: 192.168.1.1

# Step 2: Create MACVLAN network
# -o parent=eth0 : Use your actual network interface
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  my_macvlan

# Step 3: Run container with specific IP
docker run -d \
  --name web-macvlan \
  --network my_macvlan \
  --ip 192.168.1.100 \
  nginx

# Step 4: Test from ANOTHER computer on same network
# Open browser to http://192.168.1.100
# Should see nginx!

# Step 5: Important - Host cannot reach container!
# This won't work (expected behavior)
curl 192.168.1.100  # From host, may fail
```
![Java Run](20thfeb/2.jpeg)


### IPVLAN LAB
![Java Run](20thfeb/3.jpeg)

## IPVLAN vs MACVLAN

| Feature | MACVLAN | IPVLAN |
|--------|-------|------|
| MAC addresses | One per container | One shared for all |
| Network switch load | Higher (learns many MACs) | Lower (one MAC) |
| Scalability | Limited by switch | Much higher |
| Best for | Small deployments | Large-scale |

### Key Points:
- Overlay = multi-host container communication
- Uses Docker Swarm clustering
- Provides automatic DNS name resolution
- Can also be tested on single machine

# 25th february
![Java Run](25thfeb/1.jpeg)
![Java Run](25thfeb/2.jpeg)
![Java Run](25thfeb/3.jpeg)
![Java Run](25thfeb/4.jpeg)
![Java Run](25thfeb/5.jpeg)
![Java Run](25thfeb/6.jpeg)
![Java Run](25thfeb/7.jpeg)
![Java Run](25thfeb/8.jpeg)
![Java Run](25thfeb/9.jpeg)
![Java Run](25thfeb/10.jpeg)
![Java Run](25thfeb/11.jpeg)
![Java Run](25thfeb/12.png)

# 26th february
![Java Run](26thfeb/1.jpeg)
![Java Run](26thfeb/2.jpeg)
