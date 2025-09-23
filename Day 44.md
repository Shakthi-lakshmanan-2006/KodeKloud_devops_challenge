# Day 44 - KodeKloud 100 Days Challenge

## 📝 Task Overview
You need to host a static website using the **Apache HTTPD web server** inside a Docker container on **App Server 3**.  
The setup should be done using a `docker-compose.yml` file.  

### Requirements
1. Use `httpd` container (preferably `latest` tag).  
2. Name the container **httpd**.  
3. Map container’s **port 80** → host’s **port 5002**.  
4. Mount host directory `/opt/sysops` → container directory `/usr/local/apache2/htdocs`.  
5. The `docker-compose.yml` file must be placed in `/opt/docker/`.

---

## 🔑 Basics to Understand

### Docker Compose
- Lets you define containers in YAML.
- Start containers:  
  ```bash
  docker-compose up -d
  ```
- Stop containers:  
  ```bash
  docker-compose down
  ```

### httpd (Apache HTTP Server)
- Official Docker image for Apache web server.
- By default, serves files from `/usr/local/apache2/htdocs`.

### Volumes
- Used to share files between host and container.  
- Example:  
  ```yaml
  volumes:
    - /host/path:/container/path
  ```

### Port Mapping
- Format: `host_port:container_port`.  
- Example:  
  ```yaml
  ports:
    - "5002:80"
  ```

---

## ⚙️ Steps to Complete the Task

### 1. SSH into App Server 3
```bash
ssh banner@stapp03
```

### 2. Create/Edit Docker Compose File
```bash
sudo vi /opt/docker/docker-compose.yml
```

Paste the following content:
```yaml
version: '3.8'
services:
  httpd:
    image: httpd:latest
    container_name: httpd
    ports:
      - "5002:80"
    volumes:
      - /opt/sysops:/usr/local/apache2/htdocs
```

### 3. Start the Container
```bash
cd /opt/docker
sudo docker-compose up -d
```

### 4. Verify Running Container
```bash
sudo docker ps
```

### 5. Test the Website
```bash
curl http://localhost:5002
```

---
![Screenshot 2025-09-21 194851](assets/Screenshot%202025-09-21%20194851.png)

---

## Challenges I Faced in this Task:
We are using a newer version of docker compose , so the command
```bash
sudo docker-compose up -d
```
Generated me an error

---

## 📌 Summary of Commands

```bash
# Login to App Server 3
ssh banner@stapp03

# Create/Edit Docker Compose file
sudo vi /opt/docker/docker-compose.yml

# Start container using Docker Compose
cd /opt/docker
sudo docker-compose up -d

# Check running containers
sudo docker ps

# Test website
curl http://localhost:5002
```
