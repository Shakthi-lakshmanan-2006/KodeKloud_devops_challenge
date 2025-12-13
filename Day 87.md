# **Day 87 – Installing Packages on App Servers Using Ansible**

## **🔹 Task Title: Install `wget` on All Application Servers Using Ansible**

---

## **📘 Introduction**

In this task from the *100 Days of KodeKloud Challenge*, the Nautilus Application Development team needed to install the `wget` package on all app servers in the **Stratos Datacenter**.  
Since automation is preferred, we use **Ansible** to perform this installation seamlessly across multiple servers.

---

## **🔍 Task Explanation (Simplified)**

You are asked to:

1. **Create an Ansible inventory file** that lists all application servers.
2. **Create an Ansible playbook** that installs the `wget` package on all listed app servers.
3. **Run the playbook** from the jump host as user `thor`.

In simple words:  
- *Tell Ansible where the servers are (inventory).*  
- *Tell Ansible what to do (playbook).*  
- *Run the instruction and let Ansible handle the rest.*

---

## **📝 Things to Know Before Starting**

Before attempting this task, you should understand:

### **1. What is an Inventory File?**
A file where you list all your target machines.  
Example:
```
[app_servers]
server1
server2
server3
```

### **2. What is a Playbook?**
A YAML file that defines tasks Ansible should perform, such as installing software.

### **3. Why Use Ansible for Package Installation?**
- Works on multiple servers at once  
- Ensures consistency  
- Easy to repeat  
- No need to manually ssh into every server

### **4. What is `yum` Module?**
Used to install packages on RHEL-based systems like CentOS.

---
![Screenshot](./assets/Screenshot%202025-12-10%20190309.png)

---

## **📂 My Task Steps (What I Did to Complete It)**

### **🔸 Step 1 — List and enter the playbook directory**
```bash
ls
cd playbook
```

### **🔸 Step 2 — Create and edit the inventory file**
```
[app_servers]
stapp01 ansible_user=tony ansible_password=Ir0nM@n
stapp02 ansible_user=steve ansible_password=Am3ric@
stapp03 ansible_user=banner ansible_password=BigGr33n
```

### **🔸 Step 3 — Test connectivity with ping module**
```bash
ansible all -i inventory -m ping
```
All servers responded successfully with `"ping": "pong"`.

---

### **🔸 Step 4 — Create the playbook (`playbook.yml`)**

```yaml
---
- name: Install wget on all app servers
  hosts: app_servers
  become: true
  tasks:
    - name: Install wget
      yum:
        name: wget
        state: present
```

---

### **🔸 Step 5 — Run the playbook**

```bash
ansible-playbook -i inventory playbook.yml
```

#### **Observed Behavior During Execution**

- Initially, some hosts failed due to temporary connection or yum lock issues.
- Re-running the playbook eventually completed the installation on all servers.
- Final run showed all servers with `ok=2` and no failures.

---
![Screenshot](./assets/Screenshot%202025-12-10%20191720.png)

---
![Screenshot](./assets/Screenshot%202025-12-10%20191735.png)

---

## **📚 Summary of Commands Used**

| Purpose | Command |
|--------|---------|
| Check directory contents | `ls` |
| Enter the playbook directory | `cd playbook` |
| Edit inventory | `vi inventory` |
| Edit playbook | `vi playbook.yml` |
| Test server connectivity | `ansible all -i inventory -m ping` |
| Run the Ansible playbook | `ansible-playbook -i inventory playbook.yml` |

---

## ✅ **Final Result**
All three application servers successfully received the `wget` package using an automated Ansible playbook.

---

### **🎉 End of Day 87 Task — Another Step Forward in DevOps Automation!**

