# 🚀 Day 68: Set Up Jenkins Server (xFusionCorp CI/CD Project)

The DevOps team at **xFusionCorp Industries** is initiating the setup of
CI/CD pipelines and has decided to use **Jenkins** for automation.\
This guide helps you **install, configure, and start Jenkins** from
scratch.

------------------------------------------------------------------------

## 🧠 Basics You Should Know

### 🔹 What is Jenkins?

**Jenkins** is an open-source automation server used for building,
testing, and deploying software.\
It helps automate repetitive tasks like code compilation, testing, and
deployment --- forming the backbone of **CI/CD pipelines**.

### 🔹 Key Concepts

  -----------------------------------------------------------------------
  Term                       Meaning
  -------------------------- --------------------------------------------
  **CI (Continuous           Frequently integrating and testing code
  Integration)**             changes automatically.

  **CD (Continuous           Automatically deploying tested builds to
  Deployment)**              production environments.

  **Pipeline**               A series of automated steps defined in a
                             `Jenkinsfile`.

**Admin Setup**            Jenkins requires creating an admin user on
                             the first launch
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## ⚙️ Step-by-Step Jenkins Installation (RHEL/CentOS)

### **Step 1: SSH into Jenkins Server**

Use the jump host to connect to the Jenkins node.

``` bash
ssh root@jenkins
# Password: S3curePass
```

------------------------------------------------------------------------

### **Step 2: Install wget**

`wget` is used to download files from the internet.

``` bash
yum install wget -y
```

------------------------------------------------------------------------

### **Step 3: Add Jenkins Repository**

Add the official Jenkins repository for RedHat-based systems.

``` bash
wget -O /etc/yum.repos.d/jenkins.repo \
    https://pkg.jenkins.io/redhat-stable/jenkins.repo
```

------------------------------------------------------------------------

### **Step 4: Import Jenkins GPG Key**

The GPG key verifies the authenticity of the downloaded packages.

``` bash
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
```

------------------------------------------------------------------------

### **Step 5: Upgrade System Packages**

``` bash
yum upgrade -y
```

------------------------------------------------------------------------

### **Step 6: Install Java**

Jenkins runs on Java, so it must be installed first.

``` bash
yum install fontconfig java-21-openjdk -y
```

------------------------------------------------------------------------

### **Step 7: Install Jenkins**

``` bash
yum install jenkins -y
```

------------------------------------------------------------------------

### **Step 8: Reload System Daemon**

Ensures systemd recognizes new services.

``` bash
systemctl daemon-reload
```

------------------------------------------------------------------------

### **Step 9: Start and Enable Jenkins**

``` bash
systemctl start jenkins
systemctl enable jenkins
```

------------------------------------------------------------------------

### **Step 10: Verify Jenkins Status**

``` bash
systemctl status jenkins
```

You should see output indicating Jenkins is **"active (running)"**.

------------------------------------------------------------------------

### **Step 11: Get the Initial Admin Password**

After installation, Jenkins creates a temporary admin password.

``` bash
cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy this password --- you'll need it to unlock Jenkins in the browser.

------------------------------------------------------------------------

### **Step 12: Access Jenkins Web UI**

Open Jenkins in your browser:

    http://<jenkins-server-ip>:8080

- Paste the **initial admin password** when prompted.
- Install suggested plugins.
- Create the **admin user** as shown below.

------------------------------------------------------------------------

## 👤 Jenkins Admin User Setup

Fill out the admin details when prompted:

  Field           Value
  --------------- ----------------------------------------
  **Username**    `theadmin`
  **Password**    `Adm!n321`
  **Full Name**   `Siva`
  **Email**       `siva@jenkins.stratos.xfusioncorp.com`

Click **Save and Finish** → **Start using Jenkins**.

---

![Screenshot 1](./assets/Screenshot%202025-10-21%20144810.png)

---

![Screenshot 2](./assets/Screenshot%202025-10-21%20144833.png)

---

## 🧩 Jenkins Service Management Commands

  Task                     Command
  ------------------------ -----------------------------
  Start Jenkins            `systemctl start jenkins`
  Stop Jenkins             `systemctl stop jenkins`
  Restart Jenkins          `systemctl restart jenkins`
  Enable Jenkins at boot   `systemctl enable jenkins`
  Check Jenkins logs       `journalctl -u jenkins -f`

------------------------------------------------------------------------

## ✅ Summary

You've successfully: 1. Learned Jenkins fundamentals and CI/CD basics.\
2. Installed and configured Jenkins on a Linux server.\
3. Retrieved the admin password and set up your first admin user.\
4. Started Jenkins via the browser and verified it's running.

🎉 **Jenkins is now ready to automate your builds and deployments!**

------------------------------------------------------------------------

### Reference: 

- Jenkins Official Docs: <https://www.jenkins.io/doc/>
- RedHat Jenkins Repo: <https://pkg.jenkins.io/redhat-stable/>

---

![Screenshot 3](./assets/Screenshot%202025-10-21%20145026.png)

---
