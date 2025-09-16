# Day 37 – Copy Encrypted File to a Docker Container

## 🔹 Task Details
The Nautilus DevOps team possesses confidential data on **App Server 3** in the Stratos Datacenter.  
A container named **`ubuntu_latest`** is already running on the same server.  

Your task is to:  
- Copy the encrypted file `/tmp/nautilus.txt.gpg` from the **docker host** to the `ubuntu_latest` container.  
- Place it inside the container at `/tmp/`.  
- Ensure the file is **not modified** during this operation.

---

## 🔹 What You Should Know (Beginner Friendly)

### 1. Containers and Hosts
- A **host machine** is your server (App Server 3 in this case).  
- A **container** is an isolated environment running inside Docker. It has its own filesystem.  

### 2. File Movement Between Host and Container
- The host and container do **not share files** by default.  
- To move files between them, Docker provides the **`docker cp`** command.  

### 3. `docker cp` Command
The format is:  
```bash
docker cp <source_path> <container_name>:<destination_path>
```

- **From host to container**:
  ```bash
  docker cp /host/path/file.txt my_container:/container/path/
  ```
- **From container to host**:
  ```bash
  docker cp my_container:/container/path/file.txt /host/path/
  ```

👉 In this challenge, we will copy **from host → container**.

---

## 🔹 Step-by-Step Solution

### Step 1: Log in to App Server 3
Use SSH to connect to the correct server (App Server 3):
```bash
ssh tony@stapp03
```
 
---

### Step 2: Verify the Container is Running
List all running containers:
```bash
docker ps
```
✅ Look for the container named **`ubuntu_latest`** in the output.

---

### Step 3: Copy the Encrypted File
Now copy the file from host to container:
```bash
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp/
```
- `/tmp/nautilus.txt.gpg` → Source file on the **host**.  
- `ubuntu_latest:/tmp/` → Destination inside the **container**.

This ensures the file is copied exactly as it is (not modified).

---

### Step 4: Verify Inside the Container
Open a shell inside the container:
```bash
docker exec -it ubuntu_latest bash
```

Check if the file exists:
```bash
ls -l /tmp/
```
You should see:
```
nautilus.txt.gpg
```

Exit the container:
```bash
exit
```

---

<img width="1600" height="798" alt="image" src="https://github.com/user-attachments/assets/41e6af5c-86a4-4df2-b55c-030e1a533a42" />

---

## 🔹 Summary of Commands Used

| Command | Purpose |
|---------|---------|
| `ssh tony@stapp03` | Log in to App Server 3 |
| `docker ps` | Verify if `ubuntu_latest` container is running |
| `docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp/` | Copy file from host to container |
| `docker exec -it ubuntu_latest bash` | Open a shell inside the container |
| `ls -l /tmp/` | Verify the file inside the container |
| `exit` | Leave the container |

---

✅ **Final Solution Command:**
```bash
docker cp /tmp/nautilus.txt.gpg ubuntu_latest:/tmp/
```

<img width="1600" height="790" alt="image" src="https://github.com/user-attachments/assets/50d9331d-ba2e-4325-b5a8-d64efa637cc8" />

---


