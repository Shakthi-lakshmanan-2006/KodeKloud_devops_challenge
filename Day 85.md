# Day 85: Create Files on App Servers using Ansible

## Introduction

In this task, the Nautilus DevOps team is testing various **Ansible file modules** across multiple application servers in the Stratos DC.  
The goal is to practice **remote file creation**, **permission configuration**, and **user ownership management** using an automated Ansible playbook.

The problem requires creating a blank file on all app servers with specific permissions and correct user/group ownership.  
This is a common real‑world scenario—for example, preparing shared storage files or configuration placeholders before deploying applications.

---

## Task Explanation (Simplified)

The question says:

1. Create an **inventory file** on the jump host and include all app servers.
2. Create an **Ansible playbook** that:
   - Creates a blank file `/tmp/nfsshare.txt` on all app servers.
   - Sets its permissions to **0744**.
   - Ensures the correct user owns the file:
     - `tony` on server 1  
     - `steve` on server 2  
     - `banner` on server 3
3. Validation will run your playbook using:
   ```
   ansible-playbook -i inventory playbook.yml
   ```
   So your playbook should work without extra arguments.

---

## What You Should Know Before Doing This Task

Before attempting this task, you must understand:

### ✔ Inventory File  
Defines hosts and variables like username, password, and IP.

### ✔ Ansible Playbook Basics  
A YAML file containing tasks executed on remote servers.

### ✔ `file` Module  
Used for:
- Creating files
- Managing permissions
- Managing ownership

### ✔ User-Specific Variables  
Using `ansible_user` allows each host to automatically assign the correct owner and group.

---
![Screenshot](./assets/Screenshot%202025-12-05%20193748.png)

---

## Steps I Followed to Complete the Task

### **1. Created inventory file**

```
[app_servers]
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_password='Ir0nM@n'
stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_password='Am3ric@'
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_password='BigGr33n'
```

### **2. Verified connectivity**

```
ansible all -i inventory -m ping
```

### **3. Created the playbook**

`playbook.yml`:

```yaml
-
- name: create a blank file
  hosts: app_servers
  become: true
  tasks:
    - name: create a blank file /tmp/nfsshare.txt
      file:
        path: /tmp/nfsshare.txt
        state: touch
        mode: '0744'
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
```

### **4. Executed the playbook**

```
ansible-playbook -i inventory playbook.yml
```

### **5. Verified on a server**

Example check on stapp03:

```
ssh banner@stapp03
ls -lah /tmp/nfsshare.txt
```

Output showed correct owner (`banner`), permissions (`0744`), and blank file created.

---
![Screenshot](./assets/Screenshot%202025-12-05%20193812.png)

---

## Summary of Commands Used

| Purpose | Command |
|--------|---------|
| Create inventory | `vi inventory` |
| Check server connectivity | `ansible all -i inventory -m ping` |
| Open playbook | `vi playbook.yml` |
| Run playbook | `ansible-playbook -i inventory playbook.yml` |
| SSH to verify | `ssh <user>@<host>` |
| List file details | `ls -lah /tmp/nfsshare.txt` |

---

## Final Notes

This task is an excellent example of how **Ansible automates configuration management**.  
It ensures consistency across multiple servers with a single playbook, reducing manual effort and errors.

---

