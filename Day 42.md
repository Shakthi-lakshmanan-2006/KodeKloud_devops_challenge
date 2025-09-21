# 🚀 Day 42 – Docker Network Setup Challenge

## 📌 Task Description
The Nautilus DevOps team needs to set up several Docker environments for different applications.  
As part of this, I was given a ticket to **create a Docker network** with the following requirements:

1. Create a docker network named **`blog`** on App Server 2 in Stratos DC.  
2. Configure it to use the **bridge driver**.  
3. Assign it a **subnet** of `172.168.0.0/24` and an **ip-range** of `172.168.0.0/24`.

The goal is to prepare a network that containers can later use to communicate with each other.

---

## 🐳 Basics to Know Before Doing the Task

### 1. What is a Docker Network?
- A **Docker network** lets containers talk to each other.
- Each container connected to a network gets an **IP address**.
- Networks make communication easier and safer compared to using the host network.

---

### 2. Types of Docker Network Drivers
- **bridge** → Default driver, used for connecting containers on the same host.  
- **host** → Removes network isolation; container shares the host’s network.  
- **overlay** → Used in Docker Swarm for multi-host networking.  

👉 In this task, we use **bridge**.

---

### 3. Subnet and IP Range
- **Subnet**: A block of IP addresses (e.g., `172.168.0.0/24`) from which containers can get their IPs.  
- **/24** means 256 IPs (from `.0` to `.255`).  
- **IP range**: A smaller range within the subnet from which Docker assigns IPs.  

👉 In this case, both subnet and ip-range are the same (`172.168.0.0/24`).

---

## 🛠️ Steps to Complete the Task

### Step A – Create the Network with Bridge Driver
```bash
docker network create   --driver bridge   --subnet 172.168.0.0/24   --ip-range 172.168.0.0/24   blog
```

#### Explanation:
- `docker network create` → Command to create a network.  
- `--driver bridge` → Ensures we’re using the **bridge driver**.  
- `--subnet 172.168.0.0/24` → Defines the IP block.  
- `--ip-range 172.168.0.0/24` → Restricts container IPs to this range.  
- `blog` → The name of the network.

---

### Step B – Verify Network Creation
```bash
docker network ls
```
This lists all networks. You should see `blog` in the list.

```bash
docker network inspect blog
```
This shows details about the `blog` network including **Driver**, **Subnet**, and **IPRange**.

---
![Screenshot 2025-09-19 214344](assets/Screenshot%202025-09-19%20214344.png)

---

## 🐞 Error Faced During the Task

When I first tried creating the network:
```bash
docker network create --driver bridge blog
```

I got the error:
```
Error response from daemon: network with name blog already exists
```

### 🔍 Why?
A network named `blog` was already present.

### ✅ Fix:
1. Check existing networks:
   ```bash
   docker network ls
   ```
2. If the existing `blog` network does not have the right configuration → remove it:
   ```bash
   docker network rm blog
   ```
3. Recreate it with the correct settings (with subnet + ip-range):
   ```bash
   docker network create      --driver bridge      --subnet 172.168.0.0/24      --ip-range 172.168.0.0/24      blog
   ```

---
![Screenshot 2025-09-19 214423](assets/Screenshot%202025-09-19%20214423.png)

---

## 📝 Final Summary
- Learned about **Docker networks**, **bridge driver**, **subnets**, and **ip-ranges**.  
- Created a network named **blog** using the bridge driver with the required subnet and ip-range.  
- Verified it using `docker network inspect blog`.  
- Faced an error (`network with name blog already exists`) → solved it by removing the old network and recreating it with the right configuration.  

👉 Task is complete after confirming that the `blog` network shows the correct details in the JSON output.

---
