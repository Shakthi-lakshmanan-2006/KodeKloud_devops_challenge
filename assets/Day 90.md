# Day 90 – Managing File Ownership and ACLs using Ansible (KodeKloud Engineer)

## 📌 Challenge Title
**Create Files with Specific ACL Permissions on App Servers using Ansible**

---

## 🧩 Introduction

On **Day 90** of my **100 Days KodeKloud Engineer Challenge**, the task focused on managing **file creation, ownership, and ACL (Access Control List) permissions** across multiple application servers using **Ansible**.

In real-world DevOps environments, it’s common that files must be owned by `root` for security reasons, while still allowing application-specific users or groups limited access. This challenge simulates that exact requirement and enforces automation using Ansible only.

---

## 🧠 Problem Explained in Simple Terms

We are working with **three app servers** in Stratos DC. On each server, we need to:

- Create an empty file in `/opt/dba/`
- Ensure the file is **owned by root**
- Grant **specific read or read+write permissions** to a particular user or group using **ACLs**

Each server has **different requirements**, so we must carefully target the correct host and permission type.

---

## 📚 Things to Know Before Doing This Task

Before attempting this task, it’s important to understand:

- Basics of **Ansible playbooks and inventory files**
- Difference between **standard Linux permissions** and **ACLs**
- Ansible modules:
  - `file` → for creating files and setting ownership
  - `acl` → for assigning fine-grained permissions
- Meaning of ACL parameters:
  - `entity` → user or group name
  - `etype` → `user` or `group`
  - `permissions` → `r`, `w`, `rw`
- Using `become: yes` for root-level tasks

---

## ⚙️ Task Requirements Breakdown

1. **App Server 1**
   - Create `blog.txt`
   - Location: `/opt/dba/`
   - Owner: `root`
   - ACL: Read (`r`) permission to **group tony**

2. **App Server 2**
   - Create `story.txt`
   - Location: `/opt/dba/`
   - Owner: `root`
   - ACL: Read + Write (`rw`) permission to **user steve**

3. **App Server 3**
   - Create `media.txt`
   - Location: `/opt/dba/`
   - Owner: `root`
   - ACL: Read + Write (`rw`) permission to **group banner**

The playbook must run using:

```bash
ansible-playbook -i inventory playbook.yml
```

---

## 🛠️ Steps I Followed to Complete the Task

1. Logged into the **jump host**
2. Navigated to `/home/thor/ansible`
3. Created `playbook.yml`
4. Wrote separate plays targeting:
   - `stapp01`
   - `stapp02`
   - `stapp03`
5. Used the `file` module to create empty files with root ownership
6. Used the `acl` module to assign exact permissions as required
7. Executed the playbook using the provided inventory file
8. Verified successful execution via play recap

---

## 📄 playbook.yml (Final Solution)

```yaml
---
- name: Create blog.txt on app server 1 and set ACL
  hosts: stapp01
  become: yes
  tasks:
    - file:
        path: /opt/dba/blog.txt
        state: touch
        owner: root
        group: root

    - acl:
        path: /opt/dba/blog.txt
        entity: tony
        etype: group
        permissions: r
        state: present

- name: Create story.txt on app server 2 and set ACL
  hosts: stapp02
  become: yes
  tasks:
    - file:
        path: /opt/dba/story.txt
        state: touch
        owner: root
        group: root

    - acl:
        path: /opt/dba/story.txt
        entity: steve
        etype: user
        permissions: rw
        state: present

- name: Create media.txt on app server 3 and set ACL
  hosts: stapp03
  become: yes
  tasks:
    - file:
        path: /opt/dba/media.txt
        state: touch
        owner: root
        group: root

    - acl:
        path: /opt/dba/media.txt
        entity: banner
        etype: group
        permissions: rw
        state: present
```

---

## 🧾 Summary of Commands Used

```bash
cd /home/thor/ansible
ansible-playbook -i inventory playbook.yml
```

---

## ✅ Key Takeaways

- ACLs provide **fine-grained permission control** beyond standard Linux permissions
- Ansible makes it easy to enforce **security and consistency** across servers
- Separate plays help maintain **clarity and correctness** when host requirements differ
- This task closely mirrors **real-world DevOps access control scenarios**

---

🚀 **Day 90 completed successfully!**

On to the next challenge in the 100 Days KodeKloud journey 💪