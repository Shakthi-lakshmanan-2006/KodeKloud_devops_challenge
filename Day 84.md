# Day 84 – Copy Data to App Servers Using Ansible

## 📘 Introduction

In today’s challenge, the Nautilus DevOps team needs our help to automate a file distribution task. The requirement is simple:  
We must **copy a file from the jump host** to **every application server** inside the Stratos Datacenter using **Ansible**.

This is a very common real-world DevOps scenario — pushing updates, configuration files, or application content to multiple nodes with a single automated playbook.

---

## 📝 Simplified Explanation of the Task

The problem states that:

- We are on the **jump host**.
- A file exists at **/usr/src/itadmin/index.html**.
- We must copy this file to **every application server**.
- The file must be placed in the **/opt/itadmin/** directory on all servers.
- To do this, we must:
  1. Create an **inventory file** listing all app servers.
  2. Create an **Ansible playbook** that performs the copy.
  3. The validator will run:
     ```
     ansible-playbook -i inventory playbook.yml
     ```
     So the playbook must work **without extra arguments**.

---

## 📚 Things You Should Know Before Doing This Task

### ✔ Inventory Files  
- Used by Ansible to know which servers to connect to.  
- Can contain hostnames, IPs, ssh users, and passwords.

### ✔ Ansible Copy Module  
- Responsible for copying files from the **control/jump host** to target nodes.

### ✔ YAML Playbook Structure  
A valid playbook contains:
- Play name
- Hosts
- Tasks
- Modules (like copy)

### ✔ Remote Access  
Correct SSH credentials in the inventory are mandatory.

---

## 🛠 Steps I Followed to Complete the Task

### 1️⃣ Create the inventory file

```bash
touch /home/thor/ansible/inventory
cd ansible
vi inventory

```

Inventory Content :

```yaml
[app_servers]
stapp01 ansible_host=172.16.238.10 ansible_user=tony ansible_password='Ir0nM@n'
stapp02 ansible_host=172.16.238.11 ansible_user=steve ansible_password='Am3ric@'
stapp03 ansible_host=172.16.238.12 ansible_user=banner ansible_password='BigGr33n'
```

After fixing a missing quote error, verify hosts:

```bash
ansible all -i inventory -m ping
```

### 2️⃣ Create the playbook

```bash
vi playbook.yml
```

Playbook content:

```yaml
---
- name: Copy Data
  hosts: all
  become: true
  tasks:
    - name: Copy Data to app server
      copy:
        src: /usr/src/itadmin/index.html
        dest: /opt/itadmin/
        remote_src: false
```

### 3️⃣ Run the playbook

```bash
ansible-playbook -i inventory playbook.yml
```

---

## 📌 Summary of Commands Used

| Purpose | Command |
|---------|---------|
| Create inventory | `touch /home/thor/ansible/inventory` |
| Navigate | `cd ansible` |
| Edit inventory | `vi inventory` |
| Test hosts | `ansible all -i inventory -m ping` |
| Create playbook | `vi playbook.yml` |
| Run playbook | `ansible-playbook -i inventory playbook.yml` |

---

## 🎉 Final Notes

This task reinforces the power of Ansible:  
With just **one command**, we can configure **multiple servers** consistently and reliably.
