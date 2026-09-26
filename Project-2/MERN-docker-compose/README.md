# 🚀 MERN Stack Application with Docker Compose

A complete **MERN Stack (MongoDB, Express.js, React.js, Node.js)** application containerized using **Docker** and orchestrated with **Docker Compose**.

This project demonstrates how to take a multi-container MERN application and run the entire application stack using a single `docker-compose.yaml` file.

---

## 📌 Project Overview

The MERN stack consists of four major technologies:

* **MongoDB** → Database
* **Express.js** → Backend web framework
* **React.js** → Frontend UI
* **Node.js** → Backend JavaScript runtime

In this project, the application is divided into multiple Docker containers:

```text
                    ┌──────────────────────┐
                    │      User / Browser  │
                    └──────────┬───────────┘
                               │
                               │ HTTP :5173
                               ▼
                    ┌──────────────────────┐
                    │   React Frontend     │
                    │      Container       │
                    │      Port 5173        │
                    └──────────┬───────────┘
                               │
                               │ API Requests
                               ▼
                    ┌──────────────────────┐
                    │ Express + Node.js    │
                    │ Backend Container    │
                    │      Port 5050       │
                    └──────────┬───────────┘
                               │
                               │ MongoDB Connection
                               ▼
                    ┌──────────────────────┐
                    │      MongoDB         │
                    │      Container       │
                    │      Port 27017      │
                    └──────────────────────┘
```

Docker Compose creates and manages all these services together.

---

# 📂 Repository Structure

This project is located inside the main `DOCKER_PROJECTS` repository:

```text
DOCKER_PROJECTS/
│
├── Project-1/
│
└── Project-2/
    │
    └── MERN-docker-compose/
        │
        ├── docker-compose.yaml
        ├── README.md
        ├── .gitignore
        │
        └── mern/
            │
            ├── frontend/
            │   ├── Dockerfile
            │   ├── package.json
            │   ├── package-lock.json
            │   ├── src/
            │   └── ...
            │
            └── backend/
                ├── Dockerfile
                ├── package.json
                ├── package-lock.json
                └── ...
```

---

# 🏗️ Architecture

The application consists of three Docker services:

```text
┌─────────────────────────────────────────────┐
│              Docker Compose                 │
│                                             │
│  ┌───────────────┐                          │
│  │   Frontend    │                          │
│  │   React/Vite  │                          │
│  │   Port 5173   │                          │
│  └───────┬───────┘                          │
│          │                                  │
│          ▼                                  │
│  ┌───────────────┐                          │
│  │    Backend    │                          │
│  │ Node + Express│                          │
│  │   Port 5050   │                          │
│  └───────┬───────┘                          │
│          │                                  │
│          ▼                                  │
│  ┌───────────────┐                          │
│  │    MongoDB    │                          │
│  │   Port 27017  │                          │
│  │               │                          │
│  │  mongo-data   │                          │
│  └───────────────┘                          │
│                                             │
└─────────────────────────────────────────────┘
```

---

# 🐳 Docker Services

## 1. Frontend

The frontend is built using **React.js** and runs inside its own Docker container.

**Port:**

```text
5173
```

Access it from your browser:

```text
http://localhost:5173
```

The frontend Dockerfile uses Node.js:

```dockerfile
FROM node:18.9.1

WORKDIR /app

COPY package.json .

RUN npm install --fetch-timeout=600000 --fetch-retries=5

EXPOSE 5173

COPY . .

CMD ["npm", "run", "dev"]
```

### Dockerfile Explanation

### `FROM`

```dockerfile
FROM node:18.9.1
```

Uses Node.js version `18.9.1` as the base image.

---

### `WORKDIR`

```dockerfile
WORKDIR /app
```

Sets `/app` as the working directory inside the container.

---

### `COPY`

```dockerfile
COPY package.json .
```

Copies the application's `package.json` into the container.

---

### `RUN npm install`

```dockerfile
RUN npm install --fetch-timeout=600000 --fetch-retries=5
```

Installs the frontend dependencies.

The additional options increase the npm timeout and retry count, which helps prevent dependency installation failures caused by network timeouts.

---

### `EXPOSE`

```dockerfile
EXPOSE 5173
```

Documents that the application listens on port `5173`.

---

### `COPY . .`

```dockerfile
COPY . .
```

Copies the remaining application source code into the container.

---

### `CMD`

```dockerfile
CMD ["npm", "run", "dev"]
```

Starts the React/Vite development server.

---

# 2. Backend

The backend uses:

* Node.js
* Express.js

The backend runs inside its own Docker container.

**Port:**

```text
5050
```

Backend:

```text
http://localhost:5050
```

The backend communicates with MongoDB through the Docker Compose network.

---

# 3. MongoDB

MongoDB provides the database for the application.

**Port:**

```text
27017
```

MongoDB runs inside its own container.

The database data is stored using a Docker named volume:

```yaml
volumes:
  - mongo-data:/data/db
```

This allows MongoDB data to persist even if the MongoDB container is recreated.

---

# 🔗 Docker Compose

The entire application is managed using:

```text
docker-compose.yaml
```

Instead of manually creating three separate containers, Docker Compose allows us to start the complete application using:

```bash
docker compose up -d
```

---

# 📄 Docker Compose Configuration

The Compose architecture contains:

```text
frontend
backend
mongodb
```

along with:

```text
mern_network
mongo-data
```

Conceptually:

```text
services:
  frontend
  backend
  mongodb

networks:
  mern_network

volumes:
  mongo-data
```

---

# 🌐 Docker Network

Docker Compose automatically creates a network for the services.

For example:

```text
mern-docker-compose_mern_network
```

Containers connected to this network can communicate with each other using **service names**.

For example, the backend should connect to MongoDB using:

```text
mongodb
```

rather than:

```text
localhost
```

This is an important Docker networking concept.

### Why not `localhost`?

Inside the backend container:

```text
localhost
```

means:

```text
the backend container itself
```

It does **not** mean the MongoDB container.

Therefore:

```text
mongodb:27017
```

is used for communication between the backend and MongoDB containers.

---

# 💾 MongoDB Persistent Storage

The Compose configuration uses a named volume:

```text
mongo-data
```

MongoDB stores its database files in:

```text
/data/db
```

Therefore:

```text
mongo-data:/data/db
```

means:

```text
Docker volume       Container directory
     │                       │
     ▼                       ▼
mongo-data  ──────────>  /data/db
```

The volume allows data to survive container recreation.

---

# 🛠️ Prerequisites

Before running this project, install:

### Docker Desktop

Verify Docker:

```bash
docker --version
```

Example:

```text
Docker version 29.x.x
```

Verify Docker Compose:

```bash
docker compose version
```

Example:

```text
Docker Compose version v2.x.x
```

---

# 📥 Clone the Repository

Clone the main repository:

```bash
git clone https://github.com/Bikkythecoder/DOCKER_PROJECTS.git
```

Move into the repository:

```bash
cd DOCKER_PROJECTS
```

Navigate to Project-2:

```bash
cd Project-2/MERN-docker-compose
```

Verify the files:

```bash
ls
```

Expected:

```text
docker-compose.yaml
README.md
mern
```

---

# 🔨 Build Docker Images

Before starting the application, build the images:

```bash
docker compose build
```

Docker Compose will build the required images for the frontend and backend.

---

# ▶️ Start the Application

Start all services in detached mode:

```bash
docker compose up -d
```

The `-d` option means:

```text
Detached mode
```

Docker runs the containers in the background.

---

# 🔍 Check Running Containers

Run:

```bash
docker compose ps
```

You should see services similar to:

```text
frontend
backend
mongodb
```

You can also check all Docker containers:

```bash
docker ps
```

Expected architecture:

```text
Frontend    → 5173
Backend     → 5050
MongoDB     → 27017
```

---

# 🌍 Access the Application

Once the containers are running:

### Frontend

Open:

```text
http://localhost:5173
```

### Backend

Open:

```text
http://localhost:5050
```

### MongoDB

MongoDB is available on:

```text
localhost:27017
```

when accessed from the host machine.

---

# 📜 View Logs

View logs for all services:

```bash
docker compose logs
```

Follow logs continuously:

```bash
docker compose logs -f
```

View only frontend logs:

```bash
docker compose logs frontend
```

View backend logs:

```bash
docker compose logs backend
```

View MongoDB logs:

```bash
docker compose logs mongodb
```

---

# 🔎 Check Individual Containers

List running containers:

```bash
docker ps
```

List all containers:

```bash
docker ps -a
```

Inspect a container:

```bash
docker inspect <container-name>
```

---

# 🛑 Stop the Application

To stop the application:

```bash
docker compose stop
```

This stops the containers without removing them.

---

# ⬇️ Stop and Remove Containers

To stop and remove the Compose containers:

```bash
docker compose down
```

This removes the containers and network created by Compose.

The named MongoDB volume remains unless explicitly removed.

---

# ⚠️ Remove Containers and Volumes

To remove the containers **and MongoDB data**:

```bash
docker compose down -v
```

⚠️ **Warning:** This removes the named volume containing MongoDB data.

Therefore, database data may be lost.

Use this command carefully.

---

# 🔄 Restart the Application

Restart the services:

```bash
docker compose restart
```

Or recreate everything:

```bash
docker compose down
docker compose up -d
```

---

# 🧹 Rebuild After Code/Dockerfile Changes

If you modify a Dockerfile or dependencies:

```bash
docker compose build
```

Then:

```bash
docker compose up -d
```

Alternatively:

```bash
docker compose up -d --build
```

This builds the images and starts the containers in one command.

---

# 🐛 Troubleshooting

## Problem 1: Port Already in Use

If you receive an error such as:

```text
port is already allocated
```

check which process/container is using the port.

```bash
docker ps
```

You can also check the host port:

```bash
lsof -i :5173
```

For the backend:

```bash
lsof -i :5050
```

For MongoDB:

```bash
lsof -i :27017
```

---

# Problem 2: Frontend `npm install` Timeout

If npm reports something similar to:

```text
ERR_SOCKET_TIMEOUT
```

the Dockerfile uses increased timeout and retry settings:

```dockerfile
RUN npm install --fetch-timeout=600000 --fetch-retries=5
```

Then rebuild:

```bash
docker compose build --no-cache frontend
```

Start the services:

```bash
docker compose up -d
```

---

# Problem 3: Container Is Not Running

Check:

```bash
docker compose ps
```

Then inspect logs:

```bash
docker compose logs <service>
```

For example:

```bash
docker compose logs backend
```

---

# Problem 4: Backend Cannot Connect to MongoDB

Make sure MongoDB is running:

```bash
docker compose ps
```

Check MongoDB logs:

```bash
docker compose logs mongodb
```

Remember that inside Docker Compose, the backend should use the service name:

```text
mongodb
```

and not:

```text
localhost
```

---

# Problem 5: Start Fresh

If you want to completely recreate the application:

```bash
docker compose down -v
docker compose build --no-cache
docker compose up -d
```

⚠️ The `-v` option removes the MongoDB volume and therefore deletes the stored database data.

---

# 🧪 Useful Docker Commands

### Show images

```bash
docker images
```

### Show running containers

```bash
docker ps
```

### Show all containers

```bash
docker ps -a
```

### Show volumes

```bash
docker volume ls
```

### Show networks

```bash
docker network ls
```

### Inspect volume

```bash
docker volume inspect mongo-data
```

### Inspect network

```bash
docker network inspect mern-docker-compose_mern_network
```

---

# 🧰 Useful Docker Compose Commands

| Command                        | Purpose                       |
| ------------------------------ | ----------------------------- |
| `docker compose build`         | Build images                  |
| `docker compose up`            | Start services                |
| `docker compose up -d`         | Start services in background  |
| `docker compose up -d --build` | Build and start               |
| `docker compose ps`            | Show Compose containers       |
| `docker compose logs`          | Show logs                     |
| `docker compose logs -f`       | Follow logs                   |
| `docker compose restart`       | Restart services              |
| `docker compose stop`          | Stop services                 |
| `docker compose down`          | Stop and remove containers    |
| `docker compose down -v`       | Remove containers and volumes |

---

# 🧠 Key Docker Concepts Demonstrated

This project is designed to demonstrate several important Docker concepts.

### 1. Docker Images

The frontend and backend applications are packaged into Docker images.

---

### 2. Docker Containers

Each application component runs inside its own container.

```text
Frontend Container
Backend Container
MongoDB Container
```

---

### 3. Docker Compose

Docker Compose manages multiple containers as one application.

```bash
docker compose up -d
```

---

### 4. Container Networking

Services communicate using Docker's internal network.

Example:

```text
backend → mongodb:27017
```

---

### 5. Port Mapping

Host ports are mapped to container ports.

```text
Host                 Container

localhost:5173  →   frontend:5173
localhost:5050  →   backend:5050
localhost:27017 →   mongodb:27017
```

---

### 6. Docker Volumes

MongoDB uses persistent storage:

```text
mongo-data → /data/db
```

---

### 7. Dockerfiles

Both frontend and backend applications have their own Dockerfiles.

This demonstrates how individual application components can be containerized independently.

---

# 🔐 Environment Variables

For production deployments, sensitive configuration should not be hard-coded inside Dockerfiles or source code.

Typical environment variables may include:

```text
MONGO_URI
PORT
NODE_ENV
```

For example:

```text
MONGO_URI=mongodb://mongodb:27017/mydatabase
```

Docker Compose can provide environment variables to containers.

---

# 🚀 Production Improvements

This project is primarily intended for learning Docker and Docker Compose.

For production, the following improvements could be added:

* Multi-stage Docker builds
* Smaller production images
* Nginx reverse proxy
* HTTPS/TLS
* Docker secrets
* Health checks
* Production MongoDB configuration
* Environment-specific Compose files
* CI/CD pipeline
* Container image scanning
* Kubernetes deployment
* AWS deployment
* Monitoring and logging
* Resource limits
* Non-root containers

---

# ☁️ Possible DevOps Evolution

This project can be extended into a complete DevOps pipeline:

```text
Developer
    │
    ▼
GitHub
    │
    ▼
CI/CD Pipeline
    │
    ├── Build
    ├── Test
    ├── Docker Build
    ├── Docker Image Scan
    └── Push Image
            │
            ▼
       Docker Hub
            │
            ▼
       Kubernetes
            │
            ▼
          AWS
```

Possible technologies:

```text
GitHub
Docker
Docker Compose
Docker Hub
Jenkins
GitHub Actions
Kubernetes
Helm
AWS
```

---

# 📚 Learning Objectives

After completing this project, you should understand:

* What Docker is
* What Docker images are
* What Docker containers are
* How to write a Dockerfile
* How to containerize a Node.js application
* How to containerize a React application
* How MongoDB runs in a container
* How Docker Compose works
* How multiple containers communicate
* Docker Compose networking
* Docker port mapping
* Docker volumes
* Persistent database storage
* Container logs
* Container lifecycle management
* Image rebuilding
* Basic Docker troubleshooting

---

# 📝 Project Commands — Quick Reference

From:

```bash
cd Project-2/MERN-docker-compose
```

### Build

```bash
docker compose build
```

### Start

```bash
docker compose up -d
```

### Check

```bash
docker compose ps
```

### Logs

```bash
docker compose logs -f
```

### Stop

```bash
docker compose stop
```

### Remove containers

```bash
docker compose down
```

### Remove containers + volumes

```bash
docker compose down -v
```

### Rebuild and start

```bash
docker compose up -d --build
```

---

# 🎯 Project Goal

The primary goal of this project is to understand how a traditional MERN application can be transformed into a **containerized multi-service application**.

Instead of installing Node.js, MongoDB, and all dependencies directly on the host machine, Docker provides isolated environments for each component.

```text
Traditional Setup

Host Machine
├── Node.js
├── npm
├── MongoDB
└── Application


Docker Setup

Docker
├── Frontend Container
├── Backend Container
└── MongoDB Container
```

Docker Compose then provides a simple way to manage the complete application.

---

# 👨‍💻 Author

**Bikky Roy**

DevOps / Cloud / Docker Learning Projects

GitHub:

`https://github.com/Bikkythecoder`

---

# ⭐ Acknowledgement

This project is part of a hands-on Docker and DevOps learning journey focused on:

```text
Docker
Docker Compose
MERN
Linux
Git & GitHub
CI/CD
Kubernetes
AWS
```

---

## ⭐ If You Found This Project Useful

Feel free to explore the other projects in the repository and use this project as a foundation for learning containerization and DevOps.
                    └──────────┬───────────┘
                               │
                               │ HTTP :5173
                               ▼
                    ┌──────────────────────┐
                    │   React Frontend     │
                    │      Container       │
                    │      Port 51
```
