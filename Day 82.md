# Day 82: Ansible Inventory Setup and Playbook Execution

## 📌 Title  
**Creating an Ansible Inventory and Running Playbooks on App Server 1**

## 📘 Introduction  
In Day 82 of the 100 Days KodeKloud Challenge, the Nautilus DevOps team wants to test Ansible playbooks stored on the jump host. These playbooks must be executed on **App Server 1**, but Ansible cannot run anything unless a proper **inventory file** is created.  
So, the main goal is to create an inventory file with the correct server details and then run the playbook successfully.

## 🤔 Simplified Explanation of the Task  
The task is asking you to:

1. Create a file named **inventory** inside `/home/thor/playbook/`.
2. Add the details of **App Server 1** using the correct hostname (`stapp01`), IP, username, password, SSH port, etc.
3. Ensure that the hostname matches what is given in the Stratos DC documentation.
4. Run the Ansible playbook using:
   ```bash
   ansible-playbook -i inventory playbook.yml
   ```

## 🧠 Things You Should Know Before Doing This Task  

### ✔ What is an Inventory File?  
It tells Ansible:  
- **Which servers to connect to**, and  
- **How to connect to them** (credentials, ports, IPs, etc.).

### ✔ Basic Variables Used in Inventory  
- `ansible_user`
- `ansible_password`
- `ansible_host`
- `ansible_port`
- `ansible_connection`

### ✔ Important Ansible Commands  
Ping all hosts:  
```bash
ansible all -m ping -i inventory
```

Run the playbook:  
```bash
ansible-playbook -i inventory playbook.yml
```

### ✔ Playbook Understanding  
The playbook installs and starts the `httpd` service.

---

## 🛠 Steps You Followed to Complete the Task  

### **1. Created the inventory file**
```bash
touch /home/thor/playbook/inventory
```

### **2. Moved into the playbook directory**
```bash
cd /home/thor/playbook
ls
```

### **3. Opened and edited the inventory file**
You added the following content:
```ini
[app_servers]
stapp01 ansible_user=tony ansible_password='Ir0nM@n' ansible_host=172.16.238.10 ansible_connection=ssh ansible_port=22
```

### **4. Tested connection using ping module**
```bash
ansible all -m ping -i inventory
```

### **5. Viewed the playbook**
```bash
cat playbook.yml
```

### **6. Executed the playbook**
```bash
ansible-playbook -i inventory playbook.yml
```

### **7. Verified idempotency**
You ran the playbook again and observed:
- No changes  
- Which means everything is already configured correctly.

---

## 📌 Summary of Commands Used  

| Purpose | Command |
|--------|---------|
| Create inventory file | `touch /home/thor/playbook/inventory` |
| Navigate to directory | `cd /home/thor/playbook` |
| List files | `ls` |
| Edit inventory | `vi inventory` |
| Test Ansible connection | `ansible all -m ping -i inventory` |
| View playbook | `cat playbook.yml` |
| Run playbook | `ansible-playbook -i inventory playbook.yml` |

---

## 🎉 Final Notes  
This task strengthens your understanding of:  
- Creating Ansible inventories  
- Connecting to remote servers  
- Running playbooks  
- Idempotency in configuration management  

Day 82 successfully completed! 🚀
