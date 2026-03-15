# 🚀 Task Manager: Production-Ready Docker Stack

This project is a **full-stack Task Management application** built to demonstrate **advanced Docker orchestration, security, and high-availability patterns**.  
It features a **multi-service architecture** managed entirely via **Docker Compose**.

---

# 🏗️ Architecture

The system consists of **5 interconnected services**:

- **Nginx**
  - Acts as a **Load Balancer** (`least_conn`)
  - Works as a **Reverse Proxy**
  - Handles **SSL/TLS termination**

- **Flask App (x2)**
  - Two redundant application workers:
    - `flask1`
    - `flask2`
  - Ensures **high availability**

- **PostgreSQL**
  - Primary relational database
  - Handles **task persistence**

- **Redis**
  - In-memory data store
  - Used for **caching and performance**

---

# ✨ Key Technical Features

## 🐳 Multi-stage Builds
Optimized Dockerfiles reduced the image size from **~1GB** to approximately **120MB**.

## 🔒 Security
- Applications run as a **Non-root user** to minimize the attack surface.
- **Automated SSL certificate generation** via script.
- Sensitive data managed via **`.env` files** (excluded from version control).

## ❤️ Health Checks & Reliability
- Implemented custom **HEALTHCHECK** for all services.
- Used `depends_on` with **`service_healthy` conditions** to ensure proper startup order.

## ⚖️ Load Balancing
- **Nginx** distributes traffic between Flask workers.
- Static **CSS files are served directly by Nginx**.

---

# 🚀 How to Run

### 1️⃣ Clone the repository
```bash
git clone <your-repo-url>
cd task-manager-docker
```

### 2️⃣ Generate SSL Certificates
```bash
cd ssl
bash generate_ssl.sh
cd ..
```

### 3️⃣ Start the services
```bash
docker compose up --build
```

### 4️⃣ Verify containers
All **5 services** should show:

```
Status: healthy
```

### 5️⃣ Access the application

Open in your browser:

```
https://localhost
```

---

# 🛠️ Tech Stack

| Category | Technology |
|--------|--------|
| Language | Python 3.11 (Alpine) |
| Orchestration | Docker & Docker Compose |
| Web Server | Nginx (Alpine) |
| Database | PostgreSQL 16 |
| Cache | Redis 7 |

---
## Project Architecture

                 ┌─────────────────┐
                 │  User / Browser │
                 └────────┬────────┘
                          │
                          ▼
              ┌─────────────────────────┐
              │          Nginx          │
              │  Reverse Proxy + SSL    │
              │  Load Balancer          │
              └───────┬─────────┬───────┘
                      │         │
                      ▼         ▼
             ┌────────────┐ ┌────────────┐
             │  Flask 1   │ │  Flask 2   │
             │ Gunicorn   │ │ Gunicorn   │
             └─────┬──────┘ └─────┬──────┘
                   │              │
                   └──────┬───────┘
                          │
          ┌───────────────┴────────────────┐
          ▼                                ▼
 ┌───────────────────┐             ┌───────────────────┐
 │    PostgreSQL     │             │       Redis       │
 │ Persistent Storage│             │ Cache / Fast Data │
 └───────────────────┘             └───────────────────┘

# 📌 Project Goal

This project demonstrates **production-ready container architecture**, including:

- Docker multi-stage builds
- Secure container practices
- Reverse proxy + load balancing
- Service health monitoring
- High availability application deployment
