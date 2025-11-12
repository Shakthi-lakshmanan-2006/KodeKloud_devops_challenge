# Day 73 : Jenkins Scheduled Jobs

## Overview

In this task, we configure a centralized logging setup using Jenkins to periodically collect Apache logs from one of the application servers. This helps the DevOps team monitor, analyze, and troubleshoot issues efficiently by automating the log retrieval process.

## Key Concepts to Know Before Starting

| Concept | Description |
|--------|-------------|
| **Jenkins** | An automation tool used for building, deploying, and managing jobs. |
| **SSH Key Authentication** | Used to allow password-less secure communication between systems. |
| **SCP (Secure Copy Protocol)** | Command-line utility to securely transfer files between remote machines. |
| **Cron Schedule (*/5 * * * *)** | A scheduler expression indicating the job runs **every 5 minutes**. |

Understanding these makes the job configuration process smooth and error-free.

---
![Screenshot ](./assets/Screenshot%202025-10-25%20223426.png)

---

## Steps to Complete the Task

### 1. Log in to Jenkins

- Open Jenkins.
- Login with:

  ```
  Username: admin
  Password: Adm!n321
  ```

### 2. Install SSH Plugin

- Go to **Manage Jenkins → Plugins → Available Plugins**.
- Search for **SSH** and install it.
- Restart Jenkins if required.

### 3. Set Up SSH Key Access on the App Server

Go to the KodeKloud terminal:

```bash
ssh-keygen
ssh-copy-id banner@stapp03
ssh banner@stapp03   # should login without password now
cat ~/.ssh/id_rsa     # copy the private key
```

### 4. Add SSH Credentials in Jenkins

- Go to **Manage Jenkins → Credentials → System → Global → Add Credentials**
- Set:
  - **Kind:** SSH Username with Private Key
  - **ID:** stapp03
  - **Username:** banner
  - **Private Key:** paste copied key
- Click **Create**.

### 5. Configure SSH Remote Host

- Go to **Manage Jenkins → System**
- Scroll to **SSH Remote Hosts**
- Add:
  - **Hostname:** stapp03
  - **Credentials:** banner
- Save.

### 6. Create Jenkins Job

- Click **Create a job**
- Name: `copy-logs`
- Select **Freestyle Project**
- Go to **Build Triggers → Build periodically**
- Add schedule:

  ```
  */5 * * * *
  ```

### 7. Add Build Step

* Go to **Build Steps → Execute shell script on remote host using SSH**
* Command to run:

  ```bash
  scp /var/log/httpd/access_log /var/log/httpd/error_log natasha@ststor01:/usr/src/devops
  ```

- Save the job.

---
![Screenshot](./assets/Screenshot%202025-10-25%20223541%20(%20Day%2073).png)

---

## Summary of Commands Used

```bash
ssh-keygen
ssh-copy-id banner@stapp03
ssh banner@stapp03
cat ~/.ssh/id_rsa
ls -lah /var/log/
ls -lah /var/log/httpd/
scp /var/log/httpd/access_log /var/log/httpd/error_log natasha@ststor01:/usr/src/devops
```

---

## Result

The Jenkins job **copy-logs** will now run **every 5 minutes** and automatically copy Apache log files from **stapp03** to **ststor01**, enabling streamlined log monitoring.

---
![Screenshot ](./assets/Screenshot%202025-10-26%20103926.png)

---
