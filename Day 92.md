# Day 92 – Managing Jinja2 Templates Using Ansible (KodeKloud Engineer)

## 📌 Challenge Title
**Using Ansible Roles and Jinja2 Templates to Deploy Dynamic Web Content**

---

## 🧩 Introduction

On **Day 92** of my **100 Days KodeKloud Engineer Challenge**, the task moved a step deeper into Ansible by introducing **roles** and **Jinja2 templates**.

Instead of writing everything inside a single playbook, this challenge focuses on structuring automation using Ansible roles and dynamically generating configuration files using templates — a very common and powerful practice in real-world DevOps workflows.

---

## 🧠 Problem Explained in Simple Terms

The Nautilus DevOps team already had an Ansible role for installing and configuring **httpd**, but the work was incomplete.

The missing pieces were:

- Creating a **Jinja2 template** for `index.html`
- Making the content dynamic instead of hardcoding server names
- Updating the role to deploy this template correctly
- Running the role only on **App Server 1**

The final output should display a message showing **which server created the file**, using Ansible variables.

---

## 📚 Things to Know Before Doing This Task

Before attempting this task, you should be familiar with:

- Ansible **roles structure** (`tasks`, `templates`)
- What **Jinja2 templates** are and why they are used
- Common Ansible modules:
  - `yum` → package installation
  - `service` → service management
  - `template` → render Jinja2 templates
- Ansible facts and variables
- `inventory_hostname` variable
- Linux file ownership and permissions
- Running playbooks using an existing inventory

---

## ⚙️ Task Requirements Breakdown

The task required completing the following steps:

1. Update `playbook.yml` to run the **httpd role** on **App Server 1**
2. Create a Jinja2 template `index.html.j2` under:
   
   `/home/thor/ansible/role/httpd/templates/`

3. Add a dynamic line inside the template:

   `This file was created using Ansible on <server-name>`

   The server name must be fetched using `inventory_hostname`

4. Update the role task file to copy the template to:

   `/var/www/html/index.html`

5. Set correct ownership (sudo user of the server)
6. Set file permissions to `0644`

---

## 🛠️ Steps I Followed to Complete the Task

1. Logged into the **jump host**
2. Navigated to the Ansible directory
3. Verified the existing inventory file
4. Updated `playbook.yml` to apply the `httpd` role on `stapp01`
5. Created the Jinja2 template file `index.html.j2`
6. Added dynamic content using `{{ inventory_hostname }}`
7. Updated `role/httpd/tasks/main.yml` to deploy the template
8. Ensured correct permissions and ownership
9. Ran a syntax check on the playbook
10. Executed the playbook successfully
11. Verified output using `curl`

---

## 📄 Jinja2 Template: index.html.j2

```html
This file was created using Ansible on {{ inventory_hostname }}
```

---

## 📄 Role Task File: role/httpd/tasks/main.yml

```yaml
- name: install the latest version of HTTPD
  yum:
    name: httpd
    state: latest

- name: Start service httpd
  service:
    name: httpd
    state: started

- name: Create index.html by using template
  template:
    src: index.html.j2
    dest: /var/www/html/index.html
    mode: '0644'
    owner: "{{ ansible_user }}"
    group: "{{ ansible_user }}"
```

---

## 📄 playbook.yml

```yaml
---
- hosts: stapp01
  become: yes
  roles:
    - httpd
```

---

## 🧾 Summary of Commands Used

```bash
cd ~/ansible
ansible-playbook -i inventory playbook.yml --syntax-check
ansible-playbook -i inventory playbook.yml
curl stapp01
```

---

## ✅ Key Takeaways

- Ansible roles help keep automation **clean and reusable**
- Jinja2 templates allow **dynamic configuration generation**
- `inventory_hostname` avoids hardcoding server names
- Templates + roles are widely used in real production environments

---

🚀 **Day 92 completed successfully!**

Continuing the **100 Days KodeKloud Engineer Challenge** with deeper Ansible concepts 💪

