# Day 93 – Using Ansible `when` Conditionals for Host-Specific Tasks (KodeKloud Engineer)

## 📌 Challenge Title
**Conditionally Copying Files to Different App Servers Using Ansible**

---

## 🧩 Introduction

On **Day 93** of my **100 Days KodeKloud Engineer Challenge**, the focus was on understanding and using **Ansible conditionals (`when`)** to perform **different actions on different hosts** within the same playbook.

This task highlights one of Ansible’s most powerful features — the ability to make decisions dynamically based on host facts, which is extremely useful in real-world automation scenarios.

---

## 🧠 Problem Explained in Simple Terms

The Nautilus DevOps team wants to train team members to use **different Ansible approaches** for automation.

Instead of creating multiple playbooks or host-specific plays, the requirement was to:

- Run a **single playbook on all app servers**
- Use **`when` conditionals** to decide:
  - Which file to copy
  - To which server it should go
  - Which user should own it

Each app server should receive a **different file**, even though the playbook runs on all hosts.

---

## 📚 Things to Know Before Doing This Task

Before working on this task, it helps to understand:

- Ansible inventory and host naming
- What **facts** are in Ansible
- The `ansible_nodename` variable
- How `when` conditionals work
- The `copy` module
- File ownership and permissions in Linux
- Why `hosts: all` is important for conditional execution

---

## ⚙️ Task Requirements Breakdown

The playbook must:

- Run on **all app servers** (`hosts: all`)
- Use **`when` conditionals**
- Copy files from the jump host directory:

  `/usr/src/devops/`

### Server-wise Requirements

| App Server | File Name  | Destination            | Owner  | Permissions |
|----------|------------|------------------------|--------|-------------|
| stapp01  | blog.txt   | /opt/devops/blog.txt   | tony   | 0644        |
| stapp02  | story.txt  | /opt/devops/story.txt  | steve  | 0644        |
| stapp03  | media.txt  | /opt/devops/media.txt  | banner | 0644        |

The playbook must run using:

```bash
ansible-playbook -i inventory playbook.yml
```

---

## 🛠️ Steps I Followed to Complete the Task

1. Logged into the **jump host**
2. Navigated to `/home/thor/ansible`
3. Verified the inventory file
4. Created `playbook.yml`
5. Used `hosts: all` to target every app server
6. Added multiple `copy` tasks
7. Used `when` conditionals with `ansible_nodename`
8. Set correct ownership and permissions for each file
9. Executed the playbook successfully
10. Verified files were copied to the correct servers

---

## 📄 playbook.yml (Final Solution)

```yaml
---
- name: Copy files conditionally using Ansible when statements
  hosts: all
  become: yes
  tasks:

    - name: Copy blog.txt to App Server 1
      copy:
        src: /usr/src/devops/blog.txt
        dest: /opt/devops/blog.txt
        owner: tony
        group: tony
        mode: '0644'
      when: ansible_nodename == "stapp01"

    - name: Copy story.txt to App Server 2
      copy:
        src: /usr/src/devops/story.txt
        dest: /opt/devops/story.txt
        owner: steve
        group: steve
        mode: '0644'
      when: ansible_nodename == "stapp02"

    - name: Copy media.txt to App Server 3
      copy:
        src: /usr/src/devops/media.txt
        dest: /opt/devops/media.txt
        owner: banner
        group: banner
        mode: '0644'
      when: ansible_nodename == "stapp03"
```

---

## 🧾 Summary of Commands Used

```bash
cd /home/thor/ansible
ansible-playbook -i inventory playbook.yml
```

---

## ✅ Key Takeaways

- `when` conditionals allow **host-specific logic** inside one playbook
- `ansible_nodename` is useful for precise host identification
- A single playbook can handle **multiple behaviors cleanly**
- Conditionals reduce duplication and improve maintainability

---

🚀 **Day 93 completed successfully!**

Moving forward in the **100 Days KodeKloud Engineer Challenge** with stronger Ansible fundamentals 💪