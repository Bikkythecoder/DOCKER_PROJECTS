# 🚀 Project 3 — Go Multi-Stage Docker Build

## 📌 Overview

This project demonstrates how to **containerize a Go calculator application using a Docker Multi-Stage Build**.

The application is written in Go and accepts basic arithmetic operations such as:

* Addition `+`
* Subtraction `-`
* Multiplication `*`
* Division `/`

The main objective of this project is to understand how Docker multi-stage builds can separate the **application build environment** from the **final runtime environment**.

---

## 📂 Project Structure

```text
project_3/
│
├── README.md
│
└── multi_stage_docker_build_go_calculator_go_app/
    │
    ├── calculator.go
    ├── Dockerfile
    └── README.md
```

---

# 🧮 Go Calculator Application

The application is a simple command-line calculator.

Example:

```text
10 + 20
```

Output:

```text
Result: 30
```

Other examples:

```text
10 - 5
```

```text
Result: 5
```

```text
10 * 5
```

```text
Result: 50
```

```text
20 / 4
```

```text
Result: 5
```

To exit the application:

```text
exit
```

> **Note:** The current application expects spaces between the numbers and operator.

Correct:

```text
10 + 20
```

Not:

```text
10+20
```

---

# 🐳 Docker Multi-Stage Build

The project uses a Dockerfile containing **two stages**.

```text
┌───────────────────────────────────────┐
│             BUILD STAGE               │
│                                       │
│              Ubuntu                   │
│                 ↓                     │
│          Install Go compiler          │
│                 ↓                     │
│          Copy Go source code          │
│                 ↓                     │
│            Build binary               │
│                 ↓                     │
│               /app                    │
└───────────────────┬───────────────────┘
                    │
                    │ COPY --from=build
                    ↓
┌───────────────────────────────────────┐
│            RUNTIME STAGE              │
│                                       │
│               scratch                 │
│                 ↓                     │
│          Copy compiled binary         │
│                 ↓                     │
│               /app                    │
│                 ↓                     │
│          Run Go calculator             │
└───────────────────────────────────────┘
```

---

# 🏗️ Stage 1 — Build

The first stage uses Ubuntu:

```dockerfile
FROM ubuntu AS build
```

The stage is named `build`.

Go is then installed:

```dockerfile
RUN apt-get update && apt-get install -y golang-go
```

The source code is copied:

```dockerfile
COPY . .
```

The application is compiled:

```dockerfile
RUN CGO_ENABLED=0 go build -o /app .
```

This produces the executable:

```text
/app
```

---

# 📦 Stage 2 — Runtime

The second stage starts from:

```dockerfile
FROM scratch
```

`scratch` is an empty Docker base image.

It doesn't contain:

* Ubuntu
* Go
* Go compiler
* Shell
* Package manager
* Unnecessary system utilities

Only the compiled application is copied from the build stage:

```dockerfile
COPY --from=build /app /app
```

The application is then started using:

```dockerfile
ENTRYPOINT ["/app"]
```

---

# 🔥 Why Multi-Stage Builds?

Without multi-stage builds, the final Docker image can contain build tools that are unnecessary when the application is running.

For example:

```text
Go source code
       +
Go compiler
       +
Build dependencies
       +
Operating system packages
       +
Application
```

With a multi-stage build:

```text
Build Environment
       ↓
Compile Application
       ↓
Extract Binary
       ↓
Minimal Runtime Image
       ↓
Run Application
```

This provides a cleaner separation between **build-time dependencies** and **runtime requirements**.

---

# ⚙️ Build the Docker Image

Navigate to the Go application directory:

```bash
cd project_3/multi_stage_docker_build_go_calculator_go_app
```

Build the image:

```bash
docker build -t go-calculator .
```

Check the image:

```bash
docker images go-calculator
```

---

# ▶️ Run the Container

Because the calculator requires interactive terminal input, use:

```bash
docker run -it --rm go-calculator
```

### Meaning of the options

| Option | Purpose                                            |
| ------ | -------------------------------------------------- |
| `-i`   | Keeps STDIN open                                   |
| `-t`   | Allocates a terminal                               |
| `--rm` | Automatically removes the container after it exits |

---

# 🧪 Test the Application

Start the container:

```bash
docker run -it --rm go-calculator
```

Then enter:

```text
10 + 20
```

Expected:

```text
Result: 30
```

Try:

```text
100 - 25
```

Expected:

```text
Result: 75
```

Try:

```text
8 * 5
```

Expected:

```text
Result: 40
```

Try:

```text
100 / 10
```

Expected:

```text
Result: 10
```

Exit:

```text
exit
```

---

# 🔍 Useful Docker Commands

### List Docker images

```bash
docker images
```

### Inspect the image

```bash
docker inspect go-calculator
```

### View image history

```bash
docker history go-calculator
```

### List running containers

```bash
docker ps
```

### List all containers

```bash
docker ps -a
```

### Remove the image

```bash
docker rmi go-calculator
```

---

# 🧠 Key Docker Concepts

This project demonstrates the following Docker concepts:

* Dockerfile
* `FROM`
* Multi-stage builds
* Named build stages
* `RUN`
* `ENV`
* `COPY`
* `COPY --from`
* `ENTRYPOINT`
* `scratch` images
* Docker image creation
* Docker containers
* Interactive containers
* Build-time vs runtime dependencies
* Minimal container images

---

# 🔄 Complete Workflow

```text
Go Source Code
     │
     ▼
calculator.go
     │
     ▼
Docker Build
     │
     ▼
Ubuntu Builder
     │
     ├── Install Go
     │
     ├── Copy source
     │
     └── Compile application
     │
     ▼
Go Binary
     │
     ▼
scratch Runtime Image
     │
     ▼
COPY --from=build
     │
     ▼
/app
     │
     ▼
Docker Container
     │
     ▼
Go Calculator
```

---

# 🎯 Learning Outcome

After completing this project, you should understand the fundamental concept of Docker multi-stage builds:

> **Build your application in one stage and copy only the required application artifact into a separate runtime stage.**

This technique is particularly useful for compiled applications such as:

* Go
* Java
* Rust
* C
* C++

---

# 👨‍💻 Author

**Bikky Roy**

Docker Learning Journey — Project 3

---

## ⭐ Project Focus

**Go Application + Docker Multi-Stage Build + Minimal Runtime Image**
