# Day 36: Nginx Container Deployment

## 📌 Task Details
The Nautilus DevOps team is conducting application deployment tests on selected application servers.  
They require an **nginx container deployment** on **Application Server 2**.  

**Your task:**  
- On Application Server 2, create a container named **nginx_2**.  
- Use the **nginx image with the alpine tag** (`nginx:alpine`).  
- Ensure the container is in a **running state**.  

---

## 🛠️ Things to Know (Beginner-Friendly)

### 🔹 What is Docker?
- Docker allows you to **package applications into containers**.  
- A container is like a **mini-computer box** that has everything your app needs.  

### 🔹 What is an Image?
- An **image** is a **blueprint** for creating containers.  
- Example: `nginx:alpine`  
  - `nginx` → the application (a web server).  
  - `alpine` → a lightweight Linux distribution to keep the container small.  

### 🔹 What is a Container?
- A **container** is a **running instance of an image**.  
- Example: If you run the `nginx:alpine` image → you get a running nginx web server inside a container.  

### 🔹 Docker Commands You’ll Use
- `docker pull <image>` → Download an image.  
- `docker run <options> <image>` → Create & start a container.  
- `docker ps` → Show running containers.  
- `docker ps -a` → Show all containers (running + stopped).  
- `docker start <container>` → Start a stopped container.  
- `docker stop <container>` → Stop a running container.  

---

## 🚀 Steps to Complete the Task

### Step 1: Log into Application Server 2
```bash
ssh user@stapp02
```

### Step 2: Pull the nginx image (alpine version)
```bash
docker pull nginx:alpine
```
👉 Ensures we have the required image locally.

### Step 3: Run the Container
```bash
docker run -d --name nginx_2 nginx:alpine
```
**Explanation:**
- `docker run` → Create & start a container.  
- `-d` → Run in detached mode (background).  
- `--name nginx_2` → Assign the name `nginx_2`.  
- `nginx:alpine` → The image to use.  

### Step 4: Verify the Container is Running
```bash
docker ps
```
👉 Check if the container is listed with status `Up`.  

---
<img width="1600" height="793" alt="image" src="https://github.com/user-attachments/assets/61a51683-6bc9-48f7-b755-73c7d0b0395c" />

---


## ✅ Summary of Commands Used
1. `docker pull nginx:alpine` → Download the nginx image with alpine tag.  
2. `docker run -d --name nginx_2 nginx:alpine` → Create and start the container.  
3. `docker ps` → Verify the container is running.

---
<img width="1600" height="757" alt="image" src="https://github.com/user-attachments/assets/b2f9e209-99dd-4119-9180-c3914f7d66d4" />

---

