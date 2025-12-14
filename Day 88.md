
# Day 88 – Install and Configure httpd Web Server using Ansible

## 🚀 KodeKloud 100 Days DevOps Challenge – Day 88

### 📌 Introduction
On **Day 88** of my **100 Days KodeKloud DevOps Challenge**, I worked on an Ansible-based automation task focused on configuring a web server across multiple servers.  
The goal was to use **Ansible only** to install, configure, and deploy a sample web page on all application servers in **Stratos DC**.

This task helped reinforce real-world concepts like **configuration management, idempotency, inventory usage, and Ansible modules**.

---

## 🧠 Problem Explained in Simple Words
The Nautilus DevOps team wants:

- A **simple Apache (httpd) web server** installed on **all app servers**
- The **httpd service must be running**
- A **sample web page** must be deployed at `/var/www/html/index.html`
- The web page content must be managed **only using Ansible**
- Proper **file ownership and permissions** must be set

In short:  
👉 *Install httpd → start the service → create a web page → set owner & permissions*

---

## 📚 Things to Know Before Doing This Task
Before attempting this task, it’s good to understand:

- Basics of **Ansible**
- What an **inventory file** is and how host groups work
- Common Ansible modules:
  - `yum`
  - `service`
  - `blockinfile`
- Difference between:
  - `hosts: all`
  - `hosts: app_servers`
- File permissions (e.g., `0744`) and ownership in Linux
- Why `become: yes` is required for system-level changes

---

## 🛠️ What This Task Teaches
This task is a great example of **infrastructure automation**:

- Managing multiple servers with a single command
- Ensuring consistency across environments
- Avoiding manual configuration errors
- Writing clean and reusable playbooks

---

## ✅ Steps I Followed to Complete the Task

### 1️⃣ Verified Inventory File
The inventory file was already present at:
```
/home/thor/ansible/inventory
```
It contained the `app_servers` group.

---

### 2️⃣ Created the Playbook
I created `playbook.yml` inside:
```
/home/thor/ansible/
```

### 3️⃣ Playbook Logic
The playbook performs the following:

- Installs `httpd` package
- Starts and enables the `httpd` service
- Creates `/var/www/html/index.html` using `blockinfile`
- Adds the required content
- Sets:
  - Owner: `apache`
  - Group: `apache`
  - Permissions: `0744`

---

### 4️⃣ Ran the Playbook
Command used:
```bash
ansible-playbook -i inventory playbook.yml
```

The output confirmed:
- All tasks executed successfully
- No failures
- httpd installed and running on all app servers

---

## 📄 Playbook Used

```yaml
---
- name: Install and configure httpd web server
  hosts: app_servers
  become: yes

  tasks:
    - name: Install httpd package
      yum:
        name: httpd
        state: present

    - name: Start and enable httpd service
      service:
        name: httpd
        state: started
        enabled: yes

    - name: Create and update index.html with sample content
      blockinfile:
        path: /var/www/html/index.html
        create: yes
        block: |
          Welcome to XfusionCorp!

          This is  Nautilus sample file, created using Ansible!

          Please do not modify this file manually!
        owner: apache
        group: apache
        mode: '0744'
```

---

## 🧾 Summary of Commands Used

| Command | Purpose |
|------|--------|
| `ansible-playbook` | Executes the Ansible playbook |
| `yum` module | Installs httpd |
| `service` module | Starts & enables httpd |
| `blockinfile` module | Manages content inside index.html |
| `become: yes` | Runs tasks with sudo privileges |

---

## 🎯 Final Thoughts
Day 88 was a solid hands-on task that strengthened my confidence with **Ansible playbooks and real server automation**.  
It perfectly demonstrates how DevOps tools simplify repetitive operational work.

On to **Day 89** 🚀

