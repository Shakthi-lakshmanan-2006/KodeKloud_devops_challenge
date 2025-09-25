
# 🚀 Day 46 - KodeKloud 100 Days Challenge

## 📌 Task Overview

Today’s task was about setting up a **containerized stack** using **Docker Compose**.  
We had to deploy two services (Web + DB) on **App Server 2** in Stratos Datacenter.

---

## 🔹 Basics I Learned Before Doing the Task

### 1. What is Docker Compose?

- A tool to **define and run multi-container applications**.  
- Instead of running multiple `docker run` commands, we write a single **YAML file** (`docker-compose.yml`) and run:

  ```bash
  docker-compose up -d
  ```

### 2. Services in Compose

- Each service = one container.  
- Services include: `image`, `ports`, `volumes`, `environment`, `container_name`.

### 3. Port Mapping

- Exposes container port to the host system.  
- Example: `8088:80` means → host port **8088** maps to container’s **80**.

### 4. Volume Mapping

- Ensures data persistence even if containers are removed.  
- Example: `/var/www/html:/var/www/html` (host ↔ container).

### 5. Environment Variables

- Used to configure DB credentials.  
- Example:

  ```yaml
  environment:
    MYSQL_DATABASE: database_apache
    MYSQL_USER: appuser
    MYSQL_PASSWORD: ComplexP@ss123
  ```

---

## 🔹 Steps to Complete the Task

### 1. SSH into App Server 2

```bash
ssh <username>@<app-server-2-ip>
```

### 2. Create the Docker Compose File

```bash
sudo mkdir -p /opt/sysops
sudo vi /opt/sysops/docker-compose.yml
```

### 3. Add the Compose Definition

```yaml
version: '3.8'

services:
  web:
    container_name: php_apache
    image: php:apache
    ports:
      - "8088:80"
    volumes:
      - /var/www/html:/var/www/html

  db:
    container_name: mysql_apache
    image: mariadb:latest
    ports:
      - "3306:3306"
    volumes:
      - /var/lib/mysql:/var/lib/mysql
    environment:
      MYSQL_DATABASE: database_apache
      MYSQL_USER: appuser
      MYSQL_PASSWORD: ComplexP@ss123
      MYSQL_ROOT_PASSWORD: RootP@ss123
```

### 4. Start the Containers

```bash
cd /opt/sysops
sudo docker-compose up -d
```

### 5. Verify Containers

```bash
sudo docker ps
```

### 6. Test Web Service

```bash
curl http://localhost:8088/
```

---
![Screenshot 2025-09-23 090051](assets/Screenshot%202025-09-23%20090051.png)

---
![Screenshot 2025-09-23 090110](assets/Screenshot%202025-09-23%20090110.png)

---

## 🔹 Error Faced

When I tried to check containers with:

```bash
docker ps
```

I got the error:
permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock

🔑 **Fix:** Run with `sudo`:

```bash
sudo docker ps
```

---
![Screenshot 2025-09-23 090216](assets/Screenshot%202025-09-23%20090216.png)

---

## 🔹 Summary of Commands

```bash
# Connect to server
ssh <user>@<server-ip>

# Create directory & file
sudo mkdir -p /opt/sysops
sudo vi /opt/sysops/docker-compose.yml

# Start containers
cd /opt/sysops
sudo docker-compose up -d

# Verify running containers
sudo docker ps

# Test web service
curl http://localhost:8088/
```
