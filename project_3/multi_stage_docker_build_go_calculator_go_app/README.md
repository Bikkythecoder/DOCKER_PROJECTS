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
│          In
```
