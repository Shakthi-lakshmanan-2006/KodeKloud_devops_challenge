# 🚀 Day 71 : Automate Package Installation using Jenkins Job

> _"Automation is the key to efficiency — let Jenkins do the heavy lifting for you."_  
> — KodeKloud DevOps Challenge 🧑‍💻

---

## 🧩 Task Overview

The **Nautilus DevOps** team wants to automate the installation of packages on the **Nautilus Infrastructure** within the **Stratos Datacenter**.  
They’ve set up a **Jenkins Server**, and our goal is to create a Jenkins job that installs a specific package automatically on the **storage server**.

---

## 🧠 Basics to Know Before Starting

Before you begin, ensure you understand these core concepts:

### 🔹 Jenkins

- **Jenkins** is an open-source automation tool used for **Continuous Integration (CI)** and **Continuous Delivery (CD)**.
- It automates repetitive tasks like build, test, and deployment.

### 🔹 Jenkins Job

- A **Jenkins Job** is a unit of work that Jenkins executes.
- It can be a Freestyle, Pipeline, or Multibranch job.

### 🔹 Parameters in Jenkins

- Jenkins allows adding parameters (like `PACKAGE`) to make jobs dynamic and reusable.

### 🔹 SSH & Remote Execution

- Jenkins can run commands on remote servers (like the **storage server**) using SSH authentication.

---
![Screenshot](./assets/Screenshot%202025-10-22%20215749.png)

---

## ⚙️ Step-by-Step Procedure

### 🖥️ Part 1 — Jenkins UI Setup

1. **Access Jenkins**
   - Click on the **Jenkins** button in the top navigation bar.
   - Log in using the credentials:

     ```bash
     Username: admin
     Password: Adm!n321
     ```

2. **Create a New Job**
   - Go to **“New Item”** → Enter job name:  

     ```bash
     install-packages
     ```

   - Select **Freestyle project** → Click **OK**.

3. **Add Parameter**
   - Scroll to **“This project is parameterized”** → Check the box.
   - Click **Add Parameter → String Parameter**.
   - Set:

     ```bash
     Name: PACKAGE
     Default Value: httpd
     Description: Enter the package name to install
     ```

4. **Configure Build Step**
   - Click on **“Add Build Step” → Execute shell**.
   - Enter the following script:

     ```bash
     ssh natasha@ststor01 "sudo yum install -y $PACKAGE"
     ```

5. **Save & Apply**
   - Click **Save** to store your configuration.

6. **Run the Job**
   - Click **Build with Parameters**.
   - Enter the package name (e.g., `git`, `tree`, `vim`, etc.)
   - Observe the console output to verify successful installation.

---

### 💻 Part 2 — Terminal (KodeKloud Lab) Commands

Perform the following in the KodeKloud terminal to verify access and connectivity.

```bash
# Switch to jump host
thor@jump_host$ ssh natasha@ststor01

# Verify sudo access
[natasha@ststor01 ~]$ sudo visudo

# Exit the storage server
[natasha@ststor01 ~]$ exit

# Back to jump host
thor@jump_host$ ssh natasha@ststor01
[natasha@ststor01 ~]$ sudo yum install -y <package_name>

# Exit once done
[natasha@ststor01 ~]$ exit

# Generate SSH key if needed
thor@jump_host$ cat ~/.ssh/id_ed25519
```

---
![Screenshot](./assets/Screenshot%202025-10-22%20215120.png)

---

## 🧾 Summary of Commands

| Command | Purpose |
|----------|----------|
| `ssh natasha@ststor01` | Connect to the storage server |
| `sudo visudo` | Check and edit sudo permissions |
| `sudo yum install -y $PACKAGE` | Install the package specified by Jenkins |
| `exit` | Logout from the remote server |
| `cat ~/.ssh/id_ed25519` | View or generate SSH private key for Jenkins connection |

---

## ✅ Verification Steps

1. The Jenkins job should execute successfully without manual SSH.
2. The specified package must be installed on **ststor01**.
3. The job should be repeatable for any package name.

---

## 🧱 Jenkinsfile (Pipeline Version)

If you want to automate using a **Pipeline (Declarative)** job instead of a Freestyle one,  
here’s the corresponding **Jenkinsfile** you can push to GitHub:

```groovy
pipeline {
    agent any
    parameters {
        string(name: 'PACKAGE', defaultValue: 'httpd', description: 'Package to be installed')
    }
    stages {
        stage('Install Package') {
            steps {
                sshagent(['storage-server-key']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no natasha@ststor01 "sudo yum install -y ${PACKAGE}"
                    '''
                }
            }
        }
    }
    post {
        success {
            echo "✅ Package ${params.PACKAGE} installed successfully on ststor01!"
        }
        failure {
            echo "❌ Package installation failed."
        }
    }
}
```

---

## 🏁 Final Summary

- Created a Jenkins job named **install-packages**.
- Added a **string parameter** `PACKAGE` for dynamic input.
- Configured the job to SSH into the **storage server** and install the given package.
- Verified successful execution and repeatability.
- Automated a critical part of infrastructure maintenance — **package deployment** across servers.

---

**🎖️ Challenge Completed Successfully — Day 72!**
> _“Automation applied to an efficient operation will magnify the efficiency.”_  
> — Bill Gates

---
![Screenshot](./assets/Screenshot%202025-10-22%20215916.png)

---
