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
                    │      Port 51
```
