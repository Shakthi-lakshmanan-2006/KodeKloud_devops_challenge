# Day 86 – Ansible Inventory & Ping Test on Jump Host

## 📘 Introduction
In this task, the Nautilus DevOps team wants to test several Ansible playbooks on different app servers in the **Stratos DC**.  
Before running any playbooks, certain prerequisites must be met—mainly setting up password-less or password-based SSH connectivity between the **Jump Host (Ansible Controller)** and the **managed app servers**.

One of the tasks assigned is to verify that the Ansible controller (Jump Host) can successfully connect to all app servers using the inventory file provided.

---

## 📝 Task Explanation (Simplified)
You are given an inventory file located at:

```
/home/thor/ansible/inventory
```

Using this inventory file, you must run an **Ansible ping** command from the Jump Host to all app servers (`stapp01`, `stapp02`, `stapp03`) to ensure the SSH connectivity is working correctly.

If the ping response is **pong**, the connection is successful.

---

## 🧠 Things You Should Know Before Doing This Task

### ✔ Ansible Controller  
This is where Ansible commands and playbooks are executed. Here, it is the **Jump Host**.

### ✔ Inventory File  
Defines the hosts (servers) Ansible will manage.  
Each entry includes:
- Hostname  
- IP address  
- SSH username  
- SSH password  

### ✔ Ansible Ping Module  
Used to test SSH connectivity.

Command:
```
ansible all -i inventory -m ping
```

### ✔ What a Successful Ping Looks Like  
```
"ping": "pong"
```

---

## 🛠 Steps I Followed to Complete the Task

### **1. Navigated to the Ansible directory**
```bash
cd ansible/
ls
```

### **2. Verified the inventory file**
```bash
cat inventory
```

Inventory content:
```
stapp01 ansible_host=172.16.238.10 ansible_ssh_pass=Ir0nM@n ansible_user=tony
stapp02 ansible_host=172.16.238.11 ansible_ssh_pass=Am3ric@ ansible_user=steve
stapp03 ansible_host=172.16.238.12 ansible_ssh_pass=BigGr33n ansible_user=banner
```

### **3. Listed the hosts recognized by Ansible**
```bash
ansible all -i inventory --list-hosts
```

Output:
```
hosts (3):
  stapp01
  stapp02
  stapp03
```

### **4. Tested connectivity to stapp02 (initial attempt failed due to incorrect formatting)**
```bash
ansible stapp02 -i inventory -m ping
```

Result:
```
UNREACHABLE! Invalid/incorrect password
```

### **5. Fixed the inventory formatting issue**
The issue was caused by missing spacing/newlines. After correcting it, connectivity worked.

### **6. Re-ran the ping test**
```bash
ansible stapp02 -i inventory -m ping
```

Success:
```
"ping": "pong"
```

---

## ✅ Summary of Commands Used

| Purpose | Command |
|---------|---------|
| Navigate to Ansible directory | `cd ansible/` |
| List files | `ls` |
| View inventory | `cat inventory` |
| List hosts | `ansible all -i inventory --list-hosts` |
| Test ping to a specific server | `ansible stapp02 -i inventory -m ping` |
| Test ping to all servers | `ansible all -i inventory -m ping` |

---

## 🎉 Final Notes
This task mainly tests your understanding of:
- How the Ansible inventory works  
- SSH connectivity  
- Running basic Ansible ad-hoc commands  

With the corrected inventory file, Ansible successfully pinged all servers, confirming that the environment is ready for further playbook executions.
