# 📘 Day 45 - Fixing Dockerfile for Apache SSL Setup

## 🧠 Basics to Know Before Starting

Before jumping into the task, let's quickly understand some basic concepts:

- **Dockerfile:**  
  A text file containing a set of instructions used to build a Docker image. It helps automate the setup of your environment.

- **Apache HTTP Server:**  
  A popular open-source web server used to serve web pages over **HTTP** and **HTTPS** protocols.

- **SSL Configuration:**  
  SSL (Secure Sockets Layer) enables secure communication over **HTTPS** using certificates and private keys.

---

## 🎯 Task Objective

Fix the **broken Dockerfile** located at `/opt/docker` on **App Server 3** so that it builds a **valid Docker image** for an Apache server **with SSL support**.  
👉 **Note:** The base image and content must remain unchanged.

---

## 🛠️ Step-by-Step Guide

### 🔹 Step 1: Navigate to the Dockerfile Directory

```bash
cd /opt/docker
```
This command moves you into the directory where the Dockerfile is located.

---

### 🔹 Step 2: Open the Dockerfile

```bash
sudo vi Dockerfile
```
This opens the Dockerfile using the `vi` editor with superuser permissions.

- Press `i` → Enter insert mode.  
- Make the necessary edits.  
- Press `Esc` → Type `:wq` → Save and exit.

---

### 🔹 Step 3: Fix the Dockerfile

Replace incorrect commands and make sure paths are correct.  
Here’s an example of a fixed Dockerfile:

```dockerfile
FROM httpd:2.4.43

# Change default port from 80 to 8080
RUN sed -i 's/Listen 80/Listen 8080/g' /usr/local/apache2/conf/httpd.conf

# Enable SSL modules
RUN sed -i '/LoadModule ssl_module modules\/mod_ssl.so/s/^#//' /usr/local/apache2/conf/httpd.conf
RUN sed -i '/LoadModule socache_shmcb_module modules\/mod_socache_shmcb.so/s/^#//' /usr/local/apache2/conf/httpd.conf

# Include SSL configuration
RUN sed -i '/Include conf\/extra\/httpd-ssl.conf/s/^#//' /usr/local/apache2/conf/httpd.conf

# Copy SSL certificates and website files
COPY certs/server.crt /usr/local/apache2/conf/server.crt
COPY certs/server.key /usr/local/apache2/conf/server.key
COPY html/index.html /usr/local/apache2/htdocs/
```

---

### 🔹 Step 4: Build the Docker Image

```bash
docker build -t nautilus-app-image .
```
- `-t nautilus-app-image` → Names the image `nautilus-app-image`.
- `.` → Builds from the current directory.

---

### 🔹 Step 5: Verify the Image

```bash
docker images
```
This lists all available Docker images.  
✅ You should see `nautilus-app-image` in the list.

---

### 🔹 Step 6: Run the Docker Container

```bash
docker run -d -p 8080:8080 -p 443:443 --name nautilus-app nautilus-app-image
```
- `-d` → Runs the container in detached (background) mode.  
- `-p 8080:8080` → Maps port **8080** on the host to **8080** in the container.  
- `-p 443:443` → Maps port **443** (HTTPS) on the host.  
- `--name nautilus-app` → Names the container `nautilus-app`.

---

### 🔹 Step 7: Test the HTTPS Endpoint

```bash
curl -k https://localhost
```
- `-k` → Ignores SSL warnings (useful when using self-signed certificates).

If the configuration is correct, you'll see the HTML response from your Apache server.

---

![Screenshot 2025-09-21 204453](assets/Screenshot%202025-09-21%20204453.png)

---

## ✅ Final Summary of Commands

Here’s a quick summary of all the commands you used:

```bash
cd /opt/docker
sudo vi Dockerfile
docker build -t nautilus-app-image .
docker images
docker run -d -p 8080:8080 -p 443:443 --name nautilus-app nautilus-app-image
curl -k https://localhost
docker pull httpd:2.4.43
```

---
![Screenshot 2025-09-21 204203](assets/Screenshot%202025-09-21%20204203.png)

---

## 📦 Final Notes

- Always ensure your **SSL certificates** are placed correctly before building the image.  
- If you modify the Dockerfile, you must **rebuild the image** to apply the changes.  
- Using `curl -k` is only for testing; in production, always use valid certificates.

---

