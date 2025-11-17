# Day 75: Jenkins Slave Nodes Configuration

## Introduction
In modern CI/CD pipelines, Jenkins plays a crucial role in orchestrating automated tasks. To improve scalability and performance, Jenkins allows the use of **slave nodes** (also known as agent nodes) that execute jobs distributed across multiple servers.  
Day 75 focuses on configuring SSH-based Jenkins slave nodes on the Nautilus DevOps environment using three app servers.

---
![Screenshot](./assets/Screenshot%202025-11-01%20125619.png)

---

## Basics to Know Before Starting

### 🔹 What is a Jenkins Slave/Agent?
A Jenkins agent is a separate machine connected to the Jenkins master that executes jobs. Agents help distribute workload and improve performance.

### 🔹 SSH-Based Build Agents
Jenkins can connect to remote servers using **SSH authentication**.  
To enable this:
- SSH keys must be generated.
- Public keys copied to agent servers.
- Java must be installed on the agent servers.
- Jenkins must have the SSH credentials stored.

### 🔹 Requirements for This Task
- Three app servers acting as agents:
  - **App_server_1 → stapp01**
  - **App_server_2 → stapp02**
  - **App_server_3 → stapp03**
- Java 17 must be installed.
- SSH keys configured.
- Jenkins plugins installed:
  - *SSH Agent Plugin*
  - *SSH Build Agents Plugin*

---

## Step-by-Step Procedure

### **1️⃣ Install SSH Agent Plugin in Jenkins**
1. Click the **Jenkins** icon → Login using  
   `admin / Adm!n321`
2. Go to **Manage Jenkins → Plugins → Available Plugins**
3. Search for **SSH Agent**
4. Install it

---

### **2️⃣ Generate SSH Keys & Copy to All Agent Servers**
Run these commands in the KodeKloud terminal (jump host):

```sh
ssh-keygen -t ed25519
ssh-copy-id tony@stapp01
ssh-copy-id steve@stapp02
ssh-copy-id banner@stapp03
```

---

### **3️⃣ Verify Login into Each Server**
```sh
ssh banner@stapp03
ls
ls -lah
```

Repeat for all users: `tony`, `steve`, `banner`.

---

### **4️⃣ Check Jenkins Supported Java Version**
Check Jenkins version at the bottom of Jenkins UI.  
Search online: *“Jenkins 2.492.1 Java compatibility”*  
✔ Supported versions: **Java 17 / Java 21**

---

### **5️⃣ Install Java 17 on All App Servers**
On each server:

```sh
sudo yum install java-17-openjdk -y
```

Servers:
- `tony@stapp01`
- `steve@stapp02`
- `banner@stapp03`

Confirm:

```sh
java --version
```

---

### **6️⃣ Add SSH Credentials in Jenkins**

Navigate:  
**Jenkins Dashboard → Credentials → Global → Add Credentials**

For each user, select:
- **Kind:** SSH Username with Private Key  
- **ID:** username (ex: `tony`)  
- **Username:** same as ID  
- **Private key:** paste from:

```sh
cat ~/.ssh/id_ed25519
```

Repeat for:
- tony
- steve
- banner

---

### **7️⃣ Install SSH Build Agents Plugin**
Go to:  
**Manage Jenkins → Plugins → Available Plugins**  
Install **SSH Build Agents**

---

### **8️⃣ Create Jenkins Slave Nodes**
Navigate:  
**Manage Jenkins → Nodes → New Node**

### **Node 1 Configuration**
- **Name:** App_server_1  
- **Type:** Permanent agent  
- **Remote root directory:** `/home/tony/jenkins`  
- **Label:** `stapp01`  
- **Launch method:** Launch agents via SSH  
- **Host:** `stapp01`  
- **Credentials:** `tony`  
- **Host Key Strategy:** No verification

Repeat similarly for:
- **App_server_2 → steve → /home/steve/jenkins**
- **App_server_3 → banner → /home/banner/jenkins**

---

### **9️⃣ Test the Nodes with a Job**

#### Create a job:
- Name: **testing-node**
- Restrict execution to: `stapp01`
- Add build step → Execute Shell:
```sh
hostname
```

Repeat for other nodes:
- testing-node2 → stapp02
- testing-node3 → stapp03

---
![Screenshot](./assets/Screenshot%202025-11-01%20122829.png)

---
![Screenshot](./assets/Screenshot%202025-11-01%20125445.png)

---

## Summary of Commands Used

```sh
# Generate SSH key
ssh-keygen -t ed25519

# Copy public key to servers
ssh-copy-id tony@stapp01
ssh-copy-id steve@stapp02
ssh-copy-id banner@stapp03

# Login to remote servers
ssh tony@stapp01
ssh steve@stapp02
ssh banner@stapp03

# Install Java
sudo yum install java-17-openjdk -y

# Check Java version
java --version

# View SSH private key
cat ~/.ssh/id_ed25519
```

---

### ✔ Completed: Jenkins Slave Nodes Successfully Configured!  
This documentation can now be posted on LinkedIn as Day 75 progress.

---
![Screenshot](./assets/Screenshot%202025-11-01%20125707.png)

---