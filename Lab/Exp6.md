# Experiment 6 : Comparison of Docker Run and Docker Compose

## PART A – THEORY

### Docker Run vs Docker Compose – Theory Summary

#### 1. Objective

The objective of this experiment is to understand the relationship between docker run and Docker Compose, and to compare their configuration style, syntax, and practical use cases in containerized environments.

---

#### 2. Background Theory

#### 2.1 Docker Run – Imperative Approach

The docker run command is used to create and start a container directly from an image. It follows an imperative approach, which means the user provides step-by-step instructions every time a container is created.

When using docker run, we manually specify configuration options such as:

- Port mapping (-p)
- Volume mounting (-v)
- Environment variables (-e)
- Network configuration (--network)
- Restart policies (--restart)
- Resource limits (--memory, --cpus)
- Container name (--name)
- Detached mode (-d)

Example:

docker run -d \
  --name my-nginx \
  -p 8080:80 \
  -v ./html:/usr/share/nginx/html \
  -e NGINX_HOST=localhost \
  --restart unless-stopped \
  nginx:alpine

In this approach, every time the container is recreated, the full command must be rewritten. It is suitable for quick testing, single-container applications, and learning purposes, but becomes difficult to manage as application complexity increases.

---

#### 2.2 Docker Compose – Declarative Approach

Docker Compose uses a YAML configuration file (docker-compose.yml) to define services, networks, volumes, and environment variables in a structured and organized format.

It follows a declarative approach, meaning the user defines the desired final state of the application instead of providing step-by-step instructions.

Instead of running multiple docker run commands, a single command is used:

docker compose up -d

Docker Compose reads the configuration file and automatically creates and manages containers based on that definition.

Equivalent Compose configuration:

version: '3.8'

services:
  nginx:
    image: nginx:alpine
    container_name: my-nginx
    ports:
      - "8080:80"
    volumes:
      - ./html:/usr/share/nginx/html
    environment:
      NGINX_HOST: localhost
    restart: unless-stopped

Docker Compose allows configuration to be saved in a file, reused easily, version-controlled, and shared across teams. It is especially useful for multi-container applications.

---

#### 3. Mapping: Docker Run vs Docker Compose

Docker Run Flag → Docker Compose Equivalent

-p 8080:80 → ports:
-v host:container → volumes:
-e KEY=value → environment:
--name → container_name:
--network → networks:
--restart → restart:
--memory → deploy.resources.limits.memory
--cpus → deploy.resources.limits.cpus
-d → docker compose up -d

This mapping shows that Docker Compose provides a structured and cleaner alternative to complex docker run commands.

---

#### 4. Advantages of Docker Compose

Docker Compose provides several advantages:

- Simplifies multi-container application management
- Ensures reproducibility across environments
- Allows configuration to be version-controlled
- Provides unified lifecycle management (up, down, restart)
- Supports service scaling

Example of scaling:

docker compose up --scale web=3

This command runs three instances of the web service without manually executing multiple docker run commands.

---

#### 5. Conclusion

Docker run is best suited for quick testing and single-container setups where manual control is required.

Docker Compose is ideal for structured, scalable, and production-ready applications involving multiple services.

In simple terms:

Docker Run = Imperative, manual, step-by-step approach  
Docker Compose = Declarative, structured, reusable, and scalable approach  

Understanding both approaches helps in selecting the right tool depending on application complexity and deployment requirements.

## Part - B : PRACTICLE:
### Task 1: Single Container Comparison
#### Step 1: Run Nginx Using Docker Run

#### Step 2: Run Same Setup Using Docker Compose

### Task 2: Multi-Container Application
- Objective:
- Deploy WordPress with MySQL using:
   - Docker Run (manual way)
   - Docker Compose (structured way)
