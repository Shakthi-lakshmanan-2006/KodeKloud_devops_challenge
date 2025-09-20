
# Day 41 Challenge — Create Custom Docker Image with Apache on Port 8085

## 📝 Task Description
As per recent requirements shared by the Nautilus application development team, we need to create a custom Docker image. The Dockerfile should be created at `/opt/docker/Dockerfile` (note: **D must be capital**) with the following requirements:

1. Use `ubuntu:24.04` as the base image.  
2. Install **Apache2** web server.  
3. Configure Apache to listen on port **8085** instead of the default port **80**.  
   (Do not update any other Apache settings like DocumentRoot etc.)  

---

## 🔑 Basics to Know Before Doing This Task

### 1. Dockerfile
- A **Dockerfile** is like a recipe that tells Docker how to build an image.  
- Each line represents a step (like `FROM`, `RUN`, `EXPOSE`, `CMD`).

### 2. Base Image
- A **base image** provides the starting point (like Ubuntu).  
- We’re using `ubuntu:24.04` here.

### 3. Installing Packages in Ubuntu
- Use `apt-get update` (refreshes package list).  
- Use `apt-get install -y <package>` (installs software).  
- `-y` auto-confirms installation.

### 4. Apache Basics
- Apache runs as a web server, default port = **80**.  
- We change its config to listen on **8085** instead.

### 5. Dockerfile Instructions Used
- `FROM` → which base image to use.  
- `RUN` → commands to run inside the image while building.  
- `EXPOSE` → documents the container’s port.  
- `CMD` → defines the command to run when the container starts.

---

## ⚡ Step-by-Step Solution with Commands

### Step A: Create the Dockerfile
```bash
sudo mkdir -p /opt/docker
sudo tee /opt/docker/Dockerfile > /dev/null <<'EOF'
# Use Ubuntu 24.04 base image
FROM ubuntu:24.04

# Avoid interactive prompts during install
ENV DEBIAN_FRONTEND=noninteractive

# Install apache2 and clean apt cache
RUN apt-get update  && apt-get install -y --no-install-recommends apache2  && rm -rf /var/lib/apt/lists/*

# Update Apache config to listen on port 8085
RUN sed -i 's/^Listen 80/Listen 8085/' /etc/apache2/ports.conf  && sed -i 's/<VirtualHost \*:80>/<VirtualHost *:8085>/' /etc/apache2/sites-available/000-default.conf

# Expose the port
EXPOSE 8085

# Run Apache in the foreground
CMD ["apachectl", "-D", "FOREGROUND"]
EOF
```

**Explanation:**  
- `FROM ubuntu:24.04` → starts from Ubuntu 24.04.  
- `RUN apt-get update && apt-get install -y apache2` → installs Apache.  
- `sed -i` → updates config files to change port from 80 → 8085.  
- `EXPOSE 8085` → documents port.  
- `CMD` → runs Apache in foreground.

---

### Step B: Build the Image
```bash
sudo docker build -t custom-apache:day41 -f /opt/docker/Dockerfile /opt/docker
```
- `-t custom-apache:day41` → tags image name as `custom-apache` with tag `day41`.  
- `-f` → specifies Dockerfile path.  
- `/opt/docker` → build context.

---

### Step C: Run the Container
```bash
sudo docker run -d --name apache-day41 -p 8085:8085 custom-apache:day41
```
- `-d` → detached mode.  
- `--name apache-day41` → container name.  
- `-p 8085:8085` → maps host port 8085 to container port 8085.

Check if Apache works:  
```bash
curl -I http://localhost:8085
```

---
![Screenshot 2025-09-19 204036](assets/Screenshot%202025-09-19%20204036.png)


---

## ⚠️ Error I Faced
While doing **Step C.1 (installing Apache inside image)**, I **forgot to use `sudo`** before `apt-get install docker.io` on the server. Because of that, I got a **permission denied error**.  

✅ Solution: Always use `sudo` when installing software on the server. Inside Dockerfile, `sudo` is not required because Docker already runs commands as root.

---

![Screenshot 2025-09-19 204213](assets/Screenshot%202025-09-19%20204213.png)
---

## ✅ Final Summary
- Created Dockerfile at `/opt/docker/Dockerfile`.  
- Used `ubuntu:24.04` as base image.  
- Installed `apache2`.  
- Changed Apache listening port to `8085` (without changing DocumentRoot).  
- Exposed port `8085`.  
- Used `CMD ["apachectl", "-D", "FOREGROUND"]` to keep Apache running.  
- Built and ran container successfully.  

Apache is now running inside a Docker container on port **8085** 🚀.
