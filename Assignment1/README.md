# Project Assignment 1  
## Containerized Web Application with PostgreSQL using Docker Compose and Macvlan/Ipvlan

## Objective

Design, containerize, and deploy a web application using:

- **PostgreSQL** (mandatory database)
- Backend API using either **Node.js + Express** OR **FastAPI**
- **Docker multi-stage builds**
- **Separate Dockerfiles** (Backend + Database)
- **Docker Compose** for orchestration
- **Macvlan or Ipvlan networking** (mandatory)

The system must demonstrate:

- Production-ready image building
- Container networking concepts
- Persistent storage
- Service orchestration

---

## Project Deliverables

### 1. GitHub Repository

The repository must contain the complete project implementation.

### 2. Separate Dockerfiles

- `backend/Dockerfile`
- `database/Dockerfile`

### 3. Docker Compose Configuration

- `docker-compose.yml`

### 4. Network Creation Command

Command used to create the **Macvlan or Ipvlan network**.

---

## Screenshot Proofs

The following screenshots must be included:

- `docker network inspect`
- Container IP verification
- Volume persistence test

---

## Short Report (3–5 pages)

The report must include:

- Build optimization explanation
- Network design diagram
- Image size comparison
- **Macvlan vs Ipvlan comparison**
