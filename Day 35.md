# Day 35 – Docker CE & Docker Compose Installation

## 📌 Task Details

The Nautilus DevOps team wants to containerize applications.  
For this task on **App Server 2**, you are asked to:

1. Install **Docker CE** (Community Edition).  
2. Install **Docker Compose** plugin (used for multi-container applications).  
3. Start and enable the Docker service.  
4. Verify that Docker and Docker Compose are installed correctly.

---

## 🧠 Things to Know (Beginner Friendly)

Before performing this task, here are a few key concepts:

- **Docker CE**: Software that allows you to run applications in isolated containers. Think of containers as lightweight virtual machines.  
- **Docker Compose plugin**: Helps you define and run multi-container Docker applications using a YAML file.  
- **Service management with `systemctl`**: Commands like `start`, `enable`, and `status` are used to control services on Linux.  
- **Lab limitations**: Some Docker commands (like `docker run hello-world`) may fail due to environment restrictions, but the task is considered complete as long as Docker and Docker Compose are installed and the service is running.

---

## 🧑‍💻 Step-by-Step Instructions

### **Step 1: Connect to App Server 2**
```bash
ssh steve@stapp02
sudo -i
```

---

### **Step 2: Update System Packages**
```bash
yum update -y
```
> Keeps your system up-to-date and ensures package dependencies are satisfied.

---

### **Step 3: Add Docker CE Repository**
```bash
yum install -y yum-utils
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
> Docker packages are not included in the default OS repository, so we add the official Docker repo.

---

### **Step 4: Install Docker CE**
```bash
yum install -y docker-ce docker-ce-cli containerd.io
```
> Installs Docker engine, command-line tools, and the container runtime.

---

### **Step 5: Install Docker Compose Plugin**
```bash
yum install -y docker-compose-plugin
```
> Modern Docker integrates Compose as a plugin. Use `docker compose` (with space) instead of the old `docker-compose` command.

---

### **Step 6: Start and Enable Docker Service**
```bash
systemctl start docker
systemctl enable docker
```
> Starts the Docker service immediately and ensures it starts automatically on boot.

---

### **Step 7: Verify Installation**
```bash
docker --version
docker compose version
docker ps
```
- `docker --version` → Shows Docker version  
- `docker compose version` → Shows Docker Compose plugin version  
- `docker ps` → Lists running containers (should be empty if none are running)  

> Ignore any cgroup or permission warnings in logs — they are normal in lab environments.

---
<img width="1600" height="800" alt="image" src="https://github.com/user-attachments/assets/53d49478-fdc9-48cb-a2c8-50fc44289757" />

---

## 📝 Summary of Commands Used
```bash
ssh steve@stapp02
sudo -i
yum update -y
yum install -y yum-utils
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum install -y docker-ce docker-ce-cli containerd.io
yum install -y docker-compose-plugin
systemctl start docker
systemctl enable docker
docker --version
docker compose version
docker ps
```

> ✅ Once these commands are executed successfully, the Day 35 task is complete.

---

<img width="1600" height="857" alt="image" src="https://github.com/user-attachments/assets/ea39e6e4-faaa-4cce-b62e-85e0fa4b9c8b" /> 

---


This guide is **lab-tested** and will work in **KodeKloud’s App Server 2 environment** without using GitHub downloads or pip, avoiding all network-related issues.

