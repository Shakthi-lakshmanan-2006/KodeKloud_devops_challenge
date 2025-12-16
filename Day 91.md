# Day 91 – Setting Up an HTTPD Web Server Using Ansible (KodeKloud Engineer)

## 📌 Challenge Title
**Install and Configure a Simple Apache (httpd) Web Server on All App Servers Using Ansible**

---

## 🧩 Introduction

On **Day 91** of my **100 Days KodeKloud Engineer Challenge**, the focus was on automating the installation and configuration of a **basic web server** across multiple application servers using **Ansible**.

This task reflects a very common real-world DevOps scenario where teams must ensure that services are consistently installed, configured, and running across all servers — without manual intervention.

---

## 🧠 Problem Explained in Simple Terms

The Nautilus DevOps team wants to:

- Install an **Apache httpd web server** on all app servers
- Ensure the service is **running and enabled**
- Deploy a simple **sample web page**
- Modify the web page content using Ansible
- Set correct **ownership and permissions** on the web file

All of this must be done using a **single Ansible playbook**, executed with the existing inventory file.

---

## 📚 Things to Know Before Doing This Task

Before solving this task, it’s helpful to understand:

- Basics of **Ansible playbooks and inventory**
- What **Apache httpd** is and how it serves web pages
- Important Ansible modules used:
  - `yum` → install packages
  - `service` → manage services
  - `copy` → create files with content
  - `lineinfile` → modify files safely
- File ownership concepts (`user`, `group`)
- Linux file permissions (e.g., `0755`)
- Using `become: yes` for privileged tasks

---

## ⚙️ Task Requirements Breakdown

The playbook should perform the following on **all app servers**:

1. Install the `httpd` package
2. Start and enable the `httpd` service
3. Create `/var/www/html/index.html` with initial content:
   
   `This is a Nautilus sample file, created using Ansible!`

4. Add another line at the **top** of the file using `lineinfile`:

   `Welcome to Nautilus Group!`

5. Set file ownership to `apache:apache`
6. Set file permissions to `0755`

The playbook must run successfully using:

```bash
ansible-playbook -i inventory playbook.yml
```

---
![Screenshot](./assets/Screenshot%202025-12-15%20200924.png)

---

## 🛠️ Steps I Followed to Complete the Task

1. Logged into the **jump host**
2. Navigated to `/home/thor/ansible`
3. Created the playbook file `playbook.yml`
4. Used the `yum` module to install Apache (`httpd`)
5. Ensured the service was started and enabled using the `service` module
6. Created the `index.html` file with base content using `copy`
7. Inserted an additional welcome line at the **top of the file** using `lineinfile`
8. Set correct ownership and permissions for the web file
9. Executed the playbook using the provided inventory file
10. Verified successful execution across all app servers

---

## 📄 playbook.yml (Final Solution)

```yaml
---
- name: Install and configure httpd web server on all app servers
  hosts: all
  become: yes
  tasks:
    - name: Install httpd
      yum:
        name: httpd
        state: present

    - name: Start and enable httpd service
      service:
        name: httpd
        state: started
        enabled: yes

    - name: Create index.html with initial content
      copy:
        dest: /var/www/html/index.html
        content: |
          This is a Nautilus sample file, created using Ansible!
        owner: apache
        group: apache
        mode: '0755'

    - name: Add welcome line at the top of index.html
      lineinfile:
        path: /var/www/html/index.html
        line: Welcome to Nautilus Group!
        insertbefore: BOF
        owner: apache
        group: apache
        mode: '0755'
```

---
![Screenshot](./assets/Screenshot%202025-12-15%20200859.png)

---
![Screenshot](./assets/Screenshot%202025-12-15%20201032.png)

---

## 🧾 Summary of Commands Used

```bash
cd /home/thor/ansible
ansible-playbook -i inventory playbook.yml
```

---

## ✅ Key Takeaways

- Ansible makes web server setup **repeatable and consistent**
- `lineinfile` is ideal for safely editing existing files
- Proper ownership and permissions are critical for web servers
- One playbook can fully automate service setup and configuration

---

🚀 **Day 91 completed successfully!**

Continuing the journey in the **100 Days KodeKloud Engineer Challenge** 💪

