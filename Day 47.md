# 🚀 Dockerize and Deploy Python App

## 📌 Task
A Python app needed to be Dockerized and deployed on **App Server 2**.  
We were given a `requirements.txt` file under `/python_app/src/`.  

The steps involved:  
- Create a Dockerfile to build the image.  
- Build an image named `nautilus/python-app`.  
- Run a container named `pythonapp_nautilus` mapping container port `5000` to host port `8098`.  
- Test the app using `curl http://localhost:8098/`.  

---

## 🔑 Basics to Know Before Doing the Task

### 1. What is Docker?

- Docker is a tool to **package applications with dependencies** into a single unit (image).  
- That image can be run anywhere as a **container**.  

### 2. Dockerfile

- A **Dockerfile** is a script of instructions used to build an image.  
- Steps usually include:  
  - Picking a base image (e.g., `python:3.9`)  
  - Copying application files  
  - Installing dependencies  
  - Exposing required ports  
  - Defining how the app should run  

### 3. Image vs Container
- **Image**: Blueprint (recipe).  
- **Container**: Running instance of that image (prepared dish).  

### 4. Port Mapping
- Apps inside containers run in isolation.  
- Example: App listens on port **5000 inside the container**, but we want to access it at **8098 on host**.  
- Mapping format: `-p hostPort:containerPort`.  

---

## 🛠️ Steps and Commands

### Step 1: Navigate to the project directory
```bash
cd /python_app
```

---

### Step 2: Create the Dockerfile
Create a file named `Dockerfile` in `/python_app` with the following content:

```dockerfile
# Use Python base image
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Copy requirements.txt
COPY src/requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY src/ .

# Expose port 5000
EXPOSE 5000

# Start the app
CMD ["python", "server.py"]
```

---

### Step 3: Build the Docker image
```bash
docker build -t nautilus/python-app .
```
- `docker build` → Builds image from Dockerfile.  
- `-t nautilus/python-app` → Names the image.  
- `.` → Use Dockerfile in current directory.  

---

### Step 4: Run the container
```bash
docker run -d --name pythonapp_nautilus -p 8098:5000 nautilus/python-app
```
- `-d` → Run in background.  
- `--name` → Container name.  
- `-p 8098:5000` → Map host port `8098` → container port `5000`.  

---

### Step 5: Verify the container
```bash
docker ps
```
Shows running containers and port mappings.  

---

### Step 6: Test the application
```bash
curl http://localhost:8098/
```
If successful, you’ll see the app’s response.  

---
![Screenshot 2025-09-23 101521](assets/Screenshot%202025-09-23%20101521.png)

---

## ⚠️ Error Faced and Solution

While building the image, it was stuck at:

```
[internal] load metadata for docker.io/library/python:3.9
```

**Reason:**  
- The server had slow or restricted network, so pulling `python:3.9` took too long.  

**Fix:**  
- Switched to a lighter base image: `python:3.9-slim`, which is smaller and faster to download.  

---
![Screenshot 2025-09-23 101622](assets/Screenshot%202025-09-23%20101622.png)

---

## ✅ Summary of Commands

```bash 
cd /python_app
vi Dockerfile   # create Dockerfile with given instructions

docker build -t nautilus/python-app .
docker run -d --name pythonapp_nautilus -p 8098:5000 nautilus/python-app
docker ps
curl http://localhost:8098/
```
