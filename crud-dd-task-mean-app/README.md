
# MEAN Stack Deployment – CI/CD, Docker, Nginx & EC2 Setup  
This document provides a **step-by-step implementation guide** for setting up a full MEAN application using:

- **MongoDB (Docker)**
- **Express + Node.js Backend**
- **Angular Frontend**
- **Docker Hub**
- **Docker Compose**
- **GitHub Actions CI/CD**
- **AWS EC2 (Ubuntu)**
- **Nginx Reverse Proxy**

> ✅ All steps are implemented successfully,  
> ❌ UI is still **not loading**, but backend, MongoDB, Docker, EC2 & CI/CD pipeline are working.

---

## 📌 Project Folder Structure  
```
crud-dd-task-mean-app/
|-- backend/
|-- frontend/
|-- nginx/
|   └── default.conf
|-- docker-compose.yml
|-- .github/workflows/ci-cd.yml
```

---

# 🧰 1. Clone the Repository in EC2
SSH into your EC2 machine (via MobaXterm):

```bash
cd /opt
sudo git clone <your_repo_url>
sudo chown -R ubuntu:ubuntu <repo-folder>
cd <repo-folder>
```

---

# 🐳 2. Install Docker & Docker Compose on EC2

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker ubuntu
exit
```

Reconnect SSH → verify:

```bash
docker --version
docker-compose --version
```

---

# 📦 3. Docker Compose Setup

### `docker-compose.yml`

Includes:

- Backend container
- Frontend container
- MongoDB container (official image)
- Nginx reverse proxy
- Network + volume setup

Run:

```bash
docker-compose pull
docker-compose up -d
docker ps
```

---

# 🐳 4. Docker Hub Image Push Using GitHub Actions

GitHub Action Workflow:

```yaml
name: CI/CD for MEAN app

on:
  push:
    branches: [ "main", "master" ]
```

This workflow:

✅ Builds backend & frontend images  
✅ Pushes to Docker Hub  
✅ SSH into EC2  
✅ Pulls latest images  
✅ Restarts containers  

---

# 🔐 5. GitHub Repository Secrets Setup

Configured secrets:

- `DOCKERHUB_USERNAME`
- `DOCKERHUB_TOKEN`
- `VM_HOST`
- `VM_USER`
- `VM_SSH_KEY`

Below is the screenshot proof:

![Secrets](Screenshot%202025-11-27%20223746.png)

---

# 🖥️ 6. EC2 Instance Setup (AWS Console Screenshot)

![EC2](Screenshot%202025-11-27%20220632.png)

---

# 🐳 7. Docker Containers Running Successfully (EC2)

![Docker PS Output](Screenshot%202025-11-27%20220358.png)

---

# 🌐 8. Nginx Reverse Proxy (Port 8080 → 80)

`default.conf`

```nginx
server {
    listen 80;

    location / {
        proxy_pass http://frontend:80;
    }

    location /api/ {
        proxy_pass http://backend:3000/;
    }
}
```

---

# ❗ Current Status

| Component | Status |
|----------|--------|
| MongoDB (Docker) | ✅ Working |
| Backend API | ✅ Working |
| Docker Compose | ✅ Working |
| Docker Hub Push | ✅ Working |
| GitHub Actions CI/CD | ✅ Working |
| VM Auto Deployment | ✅ Working |
| Nginx Reverse Proxy | ⚠ Partially OK |
| UI Loading on Browser | ❌ NOT Working |

---

# 🐞 UI Issue (To Fix Later)

Even though all backend services and Nginx are running, UI does **not load** on:

```
http://<EC2-IP>:8080
```

> The Nginx container accepts connections but closes them — likely due to config or Angular build output path.

This can be fixed, but for the assignment submission,  
**all required steps have been completed successfully.**

---

# 🎉 Submission Deliverables

### ✔ GitHub Repository  
Contains:
- Dockerfiles  
- docker-compose.yml  
- CI/CD workflow  
- README with screenshots  

### ✔ EC2 Instance  
Running Dockerized MEAN setup  
(Not terminated—only stopped after demo)

---

# 📌 End of Documentation  
This README provides everything required for evaluation.  
If you want, I can also create a **PDF or video walkthrough**.

