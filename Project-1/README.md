# 🚀 Project 1 — Python Web Application with Docker

## 📌 Overview

**Project 1** demonstrates how to containerize and run a Python-based web application using **Docker**.

The project focuses on fundamental DevOps and containerization concepts, including:

* Python web application structure
* Dockerfile creation
* Docker image building
* Docker container execution
* Python dependency management
* Python virtual environments
* Docker `WORKDIR`, `COPY`, `RUN`, and `SHELL` instructions
* `.gitignore` configuration
* Git and GitHub project organization

---

## 🏗️ Project Structure

```text
Project-1/
│
├── README.md
│
└── Python-Web-App/
    │
    ├── Dockerfile
    ├── requirements.txt
    ├── .gitignore
    │
    └── devops/
        │
        ├── manage.py
        ├── db.sqlite3
        │
        ├── demo/
        │   ├── __init__.py
        │   ├── admin.py
        │   ├── apps.py
        │   ├── models.py
        │   ├── tests.py
        │   ├── urls.py
        │   ├── views.py
        │   ├── migrations/
        │   └── templates/
        │       └── demo_site.html
        │
        └── devops/
            ├── __init__.py
            ├── asgi.py
            ├── settings.py
            ├── urls.py
            └── wsgi.py
```

---

# 🐍 Python Web Application

The application is located inside:

```text
Project-1/Python-Web-App/devops/
```

The project follows a Django-style structure.

### Important files

| File               | Purpose                                         |
| ------------------ | ----------------------------------------------- |
| `manage.py`        | Django project management utility               |
| `settings.py`      | Application configuration                       |
| `urls.py`          | URL routing                                     |
| `views.py`         | Application views                               |
| `models.py`        | Database models                                 |
| `admin.py`         | Django admin configuration                      |
| `requirements.txt` | Python dependencies                             |
| `Dockerfile`       | Instructions for building the Docker image      |
| `.gitignore`       | Prevents unnecessary files from being committed |

---

# 🐳 Docker Configuration

The Dockerfile uses Ubuntu as the base image and installs the required Python packages.

```dockerfile
FROM ubuntu

WORKDIR /app

COPY requirements.txt /app/
COPY devops /app/

RUN apt-get update && apt-get install -y python3 python3-pip python3-venv

SHELL ["/bin/bash", "-c"]

RUN python3 -m venv venv1 && \
    source venv1/bin/activate && \
    pip install -r requirements.txt
```

---

# 🔍 Dockerfile Explanation

### `FROM ubuntu`

Uses Ubuntu as the base image.

```dockerfile
FROM ubuntu
```

---

### `WORKDIR /app`

Sets `/app` as the working directory inside the container.

```dockerfile
WORKDIR /app
```

---

### `COPY`

Copies application files from the local machine into the Docker image.

```dockerfile
COPY requirements.txt /app/
COPY devops /app/
```

---

### Install Python

Installs Python, pip, and the Python virtual-environment package.

```dockerfile
RUN apt-get update && apt-get install -y python3 python3-pip python3-venv
```

---

### Change Shell

Uses Bash for subsequent `RUN` instructions.

```dockerfile
SHELL ["/bin/bash", "-c"]
```

---

### Create Virtual Environment

Creates a Python virtual environment and installs the dependencies.

```dockerfile
RUN python3 -m venv venv1 && \
    source venv1/bin/activate && \
    pip install -r requirements.txt
```

---

# 📦 Requirements

Python dependencies are maintained in:

```text
requirements.txt
```

Keeping dependencies in a requirements file makes it easier to reproduce the application environment.

Dependencies can be installed with:

```bash
pip install -r requirements.txt
```

---

# 🐳 Build the Docker Image

Navigate to the Python web application directory:

```bash
cd Project-1/Python-Web-App
```

Build the Docker image:

```bash
docker build -t python-web-app .
```

Check the image:

```bash
docker images
```

---

# ▶️ Run the Docker Container

Run the container:

```bash
docker run -d --name python-web-app python-web-app
```

Check running containers:

```bash
docker ps
```

View container logs:

```bash
docker logs python-web-app
```

---

# 🛠️ Useful Docker Commands

### List images

```bash
docker images
```

### List running containers

```bash
docker ps
```

### List all containers

```bash
docker ps -a
```

### Stop the container

```bash
docker stop python-web-app
```

### Start the container

```bash
docker start python-web-app
```

### Remove the container

```bash
docker rm python-web-app
```

### Remove the image

```bash
docker rmi python-web-app
```

---

# 🧹 Git Configuration

The project contains a `.gitignore` file to prevent unnecessary files from being committed.

Examples include:

```gitignore
__pycache__/
*.py[cod]
*.sqlite3
venv/
venv1/
.venv/
env/
.env
.DS_Store
```

This helps keep the Git repository clean by excluding generated files, local environments, databases, and operating-system-specific files.

---

# 🎯 Learning Objectives

Through this project, the following concepts are practiced:

### Python

* Python application structure
* Virtual environments
* Dependency management
* Django project organization

### Docker

* Dockerfile
* Docker images
* Docker containers
* Base images
* Working directories
* Copying application files
* Installing packages inside containers
* Shell configuration
* Container lifecycle commands

### Git

* Repository organization
* `.gitignore`
* Staging changes
* Commits
* Branches
* Pushing code to GitHub

---

# 🚀 Future Improvements

The project can be extended with additional DevOps practices:

* [ ] Create a production-ready Dockerfile
* [ ] Add Docker Compose
* [ ] Expose the Django application through a Docker port
* [ ] Add environment variables
* [ ] Add PostgreSQL
* [ ] Add Nginx
* [ ] Add Docker image optimization
* [ ] Add CI/CD using GitHub Actions
* [ ] Push Docker images to Docker Hub
* [ ] Deploy the application to AWS
* [ ] Deploy the application to Kubernetes
* [ ] Add monitoring and logging

---

# 📚 Project Goal

The main goal of **Project 1** is to understand the complete basic workflow:

```text
Python Application
       ↓
requirements.txt
       ↓
Dockerfile
       ↓
Docker Build
       ↓
Docker Image
       ↓
Docker Container
       ↓
Running Web Application
```

This project serves as a foundation for progressing toward more advanced **Docker, CI/CD, AWS, Kubernetes, and DevOps** projects.

---

## 👨‍💻 Project

**Project:** Project 1 — Python Web Application
**Technology:** Python / Django
**Containerization:** Docker
**Version Control:** Git / GitHub

---

⭐ **Learning by building, breaking, fixing, and automating.**

