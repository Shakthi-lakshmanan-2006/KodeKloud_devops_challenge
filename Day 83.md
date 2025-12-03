# Day 83: Troubleshoot and Create Ansible Playbook

## 📝 Introduction  
In this task, we work on the *jump host* to complete an Ansible playbook that a previous team member left unfinished. The objective is simple: fix the inventory file correctly so that the playbook runs on **App Server 1**, and then create a playbook to generate an empty file on the target server.

The original question may look large, but in simpler words:

👉 **You must update the inventory so that Ansible knows which server to connect to,  
and then write a playbook that creates the file `/tmp/file.txt` on `stapp01`.**

---
![Screenshot](./assets/Screenshot%202025-12-03%20171410.png)

---

## 📚 Things You Must Know Before Doing This Task

Before starting, it is important to understand:

### **1. What is an Inventory File?**  
It tells Ansible *which servers to manage*.  
You must provide:  
- The hostname (ex: `stapp01`)  
- The correct SSH user (ex: `tony`, `steve`, etc.)  
- The server's IP address  
- The SSH private key or password method used

### **2. What is a Playbook?**  
A playbook is a YAML file where you define tasks for Ansible to execute on remote servers.

### **3. The File Module**  
We use Ansible’s `file` module with:  
```
state: touch
```
This simply creates an empty file.

---

## 🛠️ Steps Performed to Complete the Task

Below are the exact steps you executed on the terminal to complete the task. These include preparing the inventory, testing the connection, writing the playbook, and finally executing it.

```
thor@jumphost ~$ cd ansible/
thor@jumphost ~/ansible$ ls
inventory

# Editing the inventory file
thor@jumphost ~/ansible$ vi inventory

# Test Ansible connectivity
thor@jumphost ~/ansible$ ansible all -i inventory -m ping
stapp01 | UNREACHABLE

# Fix the inventory file again
thor@jumphost ~/ansible$ vi inventory

# Test again
thor@jumphost ~/ansible$ ansible all -i inventory -m ping
stapp01 | SUCCESS

# Create playbook file
thor@jumphost ~/ansible$ touch playbook.yml
thor@jumphost ~/ansible$ vi playbook.yml

# Run the playbook
thor@jumphost ~/ansible$ ansible-playbook -i inventory playbook.yml

PLAY [Create Empty File]
TASK [Gathering Facts] – ok
TASK [Creating File /tmp/file.txt using file module] – changed
PLAY RECAP – success
```

---

## 📜 Playbook Used

```yaml
- name: Create Empty File
  hosts: stapp01
  tasks:
    - name: Creating File /tmp/file.txt using file module
      file:
        path: /tmp/file.txt
        state: touch
```

---
![Screenshot](./assets/Screenshot%202025-12-03%20171422.png)

---

## 📌 Summary of Commands Used

| Purpose | Command |
|--------|---------|
| Navigate to directory | `cd ansible/` |
| View files | `ls` |
| Edit inventory | `vi inventory` |
| Test connection | `ansible all -i inventory -m ping` |
| Create playbook file | `touch playbook.yml` |
| Edit playbook | `vi playbook.yml` |
| Run playbook | `ansible-playbook -i inventory playbook.yml` |

---

## ✅ Final Output  
A fully working inventory + a playbook that successfully creates `/tmp/file.txt` on **App Server 1**.

---
![Screenshot](./assets/Screenshot%202025-12-03%20171507.png)

---