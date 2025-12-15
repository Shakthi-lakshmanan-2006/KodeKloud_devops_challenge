
# Day 89 – Install and Configure vsftpd using Ansible

## 🚀 KodeKloud 100 Days DevOps Challenge – Day 89

### 📌 Introduction
On **Day 89** of my **100 Days KodeKloud DevOps Challenge**, I worked on an Ansible automation task where the goal was to manage **package installation and service configuration** across multiple servers.

This task focused on installing and managing the **vsftpd (FTP server)** on Nautilus application servers in **Stratos DC**, using **Ansible playbooks only**.

---

## 🧠 Problem Explained in Simple Words
The developers needed an FTP service to be available on all application servers.  
Instead of manually logging into each server, the DevOps team decided to automate the task using Ansible.

The requirements were simple:

- Install **vsftpd** on all app servers
- Start the **vsftpd service**
- Enable the service so it starts automatically after reboot
- Use the existing inventory file
- Ensure the playbook runs with:
  ```bash
  ansible-playbook -i inventory playbook.yml
  ```

In short:  
👉 *Install vsftpd → start it → enable it → automate everything with Ansible*

---

## 📚 Things to Know Before Doing This Task
Before working on this task, it helps to understand:

- What **Ansible** is and how it works
- Purpose of an **inventory file**
- Difference between:
  - `hosts: all`
  - `hosts: app_servers`
- Basic Ansible modules:
  - `yum` (package installation)
  - `service` (service management)
- Why `become: yes` is required
- How Ansible ensures **idempotency**

---
![Screenshot](./assets/Screenshot%202025-12-14%20204251.png)

---

## 🛠️ What This Task Helps You Learn
This task reinforces:

- Managing services across multiple servers
- Writing clean and minimal Ansible playbooks
- Automating repetitive system administration work
- Understanding how validation systems execute playbooks

---

## ✅ Steps I Followed to Complete the Task

### 1️⃣ Verified Inventory File
The inventory file was already available at:
```
/home/thor/ansible/inventory
```
It contained the `app_servers` group.

---

### 2️⃣ Created the Playbook
I created the playbook at:
```
/home/thor/ansible/playbook.yml
```

---

### 3️⃣ Wrote the Playbook Logic
The playbook performs the following actions:

- Installs `vsftpd` package
- Starts the `vsftpd` service
- Enables the service at boot
- Uses privilege escalation (`become: yes`)

---

### 4️⃣ Executed the Playbook
Command used for execution:
```bash
ansible-playbook -i inventory playbook.yml
```

The playbook executed successfully on all application servers without any errors.

---

## 📄 Playbook Used

```yaml
---
- name: Install and configure vsftpd on app servers
  hosts: app_servers
  become: yes

  tasks:
    - name: Install vsftpd package
      yum:
        name: vsftpd
        state: present

    - name: Start and enable vsftpd service
      service:
        name: vsftpd
        state: started
        enabled: yes
```

---
![Screenshot](./assets/Screenshot%202025-12-14%20204746.png)

---
![Screenshot](./assets/Screenshot%202025-12-14%20205202.png)

---

## 🧾 Summary of Commands and Modules Used

| Command / Module | Purpose |
|----------------|--------|
| `ansible-playbook` | Runs the Ansible playbook |
| `yum` | Installs vsftpd |
| `service` | Starts and enables vsftpd |
| `become: yes` | Executes tasks with sudo privileges |
| Inventory file | Defines target app servers |

---

## 🎯 Final Thoughts
Day 89 was another practical step in understanding **real-world Ansible automation**.  
Tasks like this show how DevOps tools help scale operations efficiently while avoiding manual errors.

Onward to **Day 90** 🚀
