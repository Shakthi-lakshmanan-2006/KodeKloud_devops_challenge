# 🧩 Day 70 : Configure Jenkins User Access

> *"Automation is good, but only if you know exactly where to put the human touch."*  
> — **Elon Musk**

---

## 📘 Before You Begin

Before starting, ensure the following prerequisites are met:

- Jenkins server is **installed and running**.  
- You have access to the **Jenkins dashboard** via the provided URL.  
- Credentials for the admin user are available.

```bash
# Jenkins login credentials
Username: admin
Password: Adm!n321
```

---

## ⚙️ Task Overview

The Nautilus DevOps team is integrating Jenkins into their CI/CD pipeline.  
You are required to configure **user access control** by creating a new user and applying role-based authorization using the **Project-based Matrix Authorization Strategy**.

---

## 🚀 Step-by-Step Procedure

### 1️⃣ Login to Jenkins

Click the **Jenkins** icon on the top bar to access the Jenkins UI.  
Then login using:

```bash
Username: admin
Password: Adm!n321
```

---

### 2️⃣ Create a New Jenkins User

Go to:  
`Manage Jenkins → Users → Add User`

Fill in the details as follows:

| Field | Value |
|--------|--------|
| Username | mariyam |
| Password | YchZHRcLkL |
| Full Name | Mariyam |

---

### 3️⃣ Install Required Plugin

Navigate to:  
`Manage Jenkins → Plugins → Available Plugins`

Search for and install:

```bash
Matrix Authorization Strategy Plugin
```

Wait until the plugin installs completely, then restart Jenkins if prompted.

---

### 4️⃣ Configure Authorization Strategy

After Jenkins restarts:

1. Go to `Manage Jenkins → Security → Authorization`
2. Change **Authorization** to:

   ```
   Project-based Matrix Authorization Strategy
   ```

3. Click **Add user** → Enter `mariyam`  
   - Under **Overall**, check ✅ **Read**

4. Again, click **Add user** → Enter `admin`  
   - Under **Overall**, check ✅ **Administer**

---

### 5️⃣ Configure Existing Job Permissions

Now go to:

`Dashboard → Helloworld → Configure (left side menu)`

Ensure **mariyam** has only **read** permissions for this job, disregarding SCM or Agent-related permissions.

---
![Screenshot 1](./assets/Screenshot%202025-10-21%20170247.png)

---
![Screenshot 2](./assets/Screenshot%202025-10-21%20164526.png)

---

## 🧠 Summary

✅ You successfully:

* Logged into Jenkins as the admin user.  
* Created a new user `mariyam`.  
* Installed and applied **Project-based Matrix Authorization Strategy**.  
* Granted appropriate role permissions to both `admin` and `mariyam`.  
* Validated access on the **Helloworld** job.

---

## 🏁 End of Day 70 — Jenkins User Access Configuration Complete 🎯

---
![Screenshot 3](./assets/Screenshot%202025-10-21%20170335.png)

---
