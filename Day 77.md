# Day 77: Jenkins Deploy Pipeline

## Introduction
In this task, we work with Gitea, Jenkins, and a Storage Server to configure a Jenkins pipeline job that deploys a web application. The goal is to set up the required plugins, create a Jenkins node, connect it using the agent, and create a basic pipeline job.

## What You Should Know Before Starting
- Basic Linux commands (ssh, ls, yum installation, screen)
- Understanding of Jenkins UI navigation
- Jenkins pipeline basics
- Basic Git operations
- How Jenkins agents connect using agent.jar

---
![Screenshot](./assets/Screenshot%202025-11-01%20185412.png)

---

## Steps Performed

### 1. Login to Gitea
- Username: `sarah`
- Password: `Sarah_pass123`

### 2. Login to Jenkins
- Username: `admin`
- Password: `Adm!n321`

### 3. Install Pipeline Plugin
Manage Jenkins → Plugins → Search “Pipeline” → Install

### 4. Prepare Storage Server
```bash
ssh natasha@ststor01
ls -lah /var/www/html/
sudo yum install -y java-17-openjdk
```

### 5. Update Jenkins URL
Set Jenkins URL → `https://jenkins:8080/`

### 6. Create Node
- Name: Storage Server  
- Root Directory: `/var/www/html`
- Label: `ststor01`

### 7. Update URL Back to HTTP
`http://jenkins:8080/`

### 8. Connect Agent
```bash
echo ef1ac8aaa5bf1ce835c94a7bbf26e318dc6222406decf98f0fbe7edafd09289f > secret-file
curl -sO http://jenkins:8080/jnlpJars/agent.jar
```

### 9. Screen + Agent Connection
```bash
sudo yum install screen -y
screen -S jenkin
sudo java -jar agent.jar -url http://jenkins:8080/ -secret @secret-file -name "Storage Server" -webSocket -workDir "/var/www/html"
```

### 10. Create Pipeline Job
- Name: `xfusion-webapp-job`
- Type: Pipeline
- Script: “Hello world”

### 11. Git Fix
```bash
cd /var/www/html/
git remote -v
git config --global --add safe.directory /var/www/html
sudo !!
git remote -v
```
---
![Screenshot](./assets/Screenshot%202025-11-01%20185204.png)

---

![Screenshot](./assets/Screenshot%202025-11-01%20185115.png)

---

## Summary
- Logged into Gitea and Jenkins
- Installed Pipeline plugin
- Prepared storage server
- Configured and connected node
- Created pipeline job
* Fixed Git permissions

---
![Screenshot](./assets/Screenshot%202025-11-01%20185508.png)

---