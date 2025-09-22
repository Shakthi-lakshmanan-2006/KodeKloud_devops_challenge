# Day 43 Challenge – Hosting an Application on an nginx-based Container

## 📌 Introduction
In this challenge, the Nautilus DevOps team has been assigned the task of hosting an application using an **nginx-based container**. The goal is to pull the `nginx:alpine` Docker image, create a container named `demo`, and then map a host port to the container port so that the application can be accessed externally.

This task helps us understand how to work with **Docker images**, **containers**, and **port mapping**.

---

## 🧑‍💻 Basics You Should Know Before This Challenge

### 1. What is Docker?
- Docker is a tool that allows applications to run inside **containers**.
- A **container** is like a lightweight virtual machine, but much faster and portable.

### 2. What is an Image vs a Container?
- **Image**: A blueprint/template (e.g., `nginx:alpine`).
- **Container**: A running instance created from an image.

### 3. What is nginx?
- **nginx** is a web server used to serve web pages.  
- By default, it listens on **port 80** inside the container.

### 4. What is Alpine?
- `nginx:alpine` is a lightweight version of the nginx image built on Alpine Linux.  
- It is smaller and faster to pull and run.

### 5. Why Port Mapping?
- The container runs nginx on port `80` internally.  
- We map a **host port** (8088) → **container port** (80) so that we can access it via the server’s IP and port.

---

## 🚀 Steps to Perform the Task

### Step a: Pull the nginx:alpine Image
```bash
docker pull nginx:alpine
```

### Step b: Create a Container Named `demo`
```bash
docker create --name demo nginx:alpine
```

Check if the container was created successfully:
```bash
docker ps -a
```

### Step c: Map Host Port 8088 to Container Port 80 and Run the Container
```bash
docker rm demo   # Remove the old container (since we need port mapping)
docker run -d --name demo -p 8088:80 nginx:alpine
```

Check if the container is running:
```bash
docker ps
```

Verify nginx is working:
```bash
curl http://localhost:8088
```

You should see the nginx welcome page.

---
![Screenshot 2025-09-21 184745](assets/Screenshot%202025-09-21%20184745.png)

---


## ⚠️ Challenges Faced
I faced **no challenges** while performing this task.

---

## ✅ Final Summary
- Pulled the `nginx:alpine` image successfully.  
- Created a container named `demo`.  
- Mapped **host port 8088 → container port 80**.  
- Verified that nginx was running and serving content.  

This task helped me understand Docker basics, container creation, and port mapping in a very practical way.

---
![Screenshot 2025-09-21 185019](assets/Screenshot%202025-09-21%20185019.png)

---