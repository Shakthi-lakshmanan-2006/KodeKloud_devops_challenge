# 📄 Day 79 — Automating Web Application Deployment using Jenkins, Gitea & Apache

## ⭐ Introduction

In this task, the DevOps team needs to automate deployment of a simple web application stored in a **Gitea Git repository**. Whenever a developer pushes new code to the **master** branch, Jenkins should automatically:

- Pull the latest changes  
- Deploy the website files into `/var/www/html` on the Storage Server  
- Ensure the website is accessible through **Apache (httpd)** running on port **8080`

This forms a complete CI/CD workflow—**Git → Jenkins → Apache Webserver**.

---
![Screenshot](./assets/Screenshot%202025-11-26%20180318.png)

---

## ⭐ Things You Should Know Before Doing This Task

### 1️⃣ Gitea Repository Information

- Git service: **Gitea**
- Username: **sarah**
- Password: **sarah_pass123**
- Repository `web` is already cloned under Sarah’s home on Storage Server:

```
/home/sarah/web
```

---

### 2️⃣ Apache (httpd) Web Server

- Must serve the website on **port 8080**
- Document root:

```
/var/www/html
```

Apache must be installed and configured to listen on the correct port on all app servers.

---

### 3️⃣ Jenkins Job Requirements

Create a Jenkins freestyle job named:

```
nautilus-app-deployment
```

This job must:

- Pull code from the Gitea repository  
- Watch the **master** branch  
- Run automatically on every push  
- Deploy the repo contents into `/var/www/html`  
- Run deployment with proper permissions (ownership must be `sarah`)

---

### 4️⃣ File Permission Requirement

The directory `/var/www/html` must be writable by **sarah** so Jenkins can deploy files:

```
sudo chown -R sarah:sarah /var/www/html
```

---

## ⭐ Steps to Complete the Task

---

### 🔹 Step 1: Install & Configure Apache on Port 8080

```bash
sudo yum install httpd -y
sudo sed -i 's/^Listen 80/Listen 8080/' /etc/httpd/conf/httpd.conf
sudo systemctl restart httpd
sudo systemctl enable httpd
```

---

### 🔹 Step 2: Fix Ownership for Deployment

```
sudo chown -R sarah:sarah /var/www/html
```

This ensures Jenkins (running via Sarah or copying through Sarah-owned paths) can deploy the web files.

---

### 🔹 Step 3: Create Jenkins Job — `nautilus-app-deployment`

* Open Jenkins → **New Item**
* Select **Freestyle Project**
* Set name:

```
nautilus-app-deployment
```

4. Under **Source Code Management → Git**:
   - Repository URL:

```
http://gitea.stratos.xfusioncorp.com/sarah/web.git
```

- Credentials: Add Sarah's credentials  
- Branch:

```
master
```

---

### 🔹 Step 4: Add Webhook Trigger / Gitea Trigger

Enable:

```
Build when a change is pushed to Gitea
```

This ensures Jenkins builds automatically after every `git push`.

---

### 🔹 Step 5: Add Deployment Script in Jenkins

Under **Build → Execute Shell**, add:

```bash
rsync -av --delete . /var/www/html/
```

Or simpler:

```bash
cp -r * /var/www/html/
```

This deploys the entire content of the repo to the Apache document root.

---

### 🔹 Step 6: Update HTML Content on Storage Server

SSH as Sarah:

```bash
ssh sarah@ststor01
cd web/
```

Edit:

```bash
vi index.html
```

Replace content with:

```
Welcome to xFusionCorp Industries
```

Stage and commit changes:

```bash
git add index.html
git commit -m "First Deployment"
git push
```

This **push triggers the Jenkins job automatically**.

---

### 🔹 Step 7: Validate Deployment

Open:

```
http://<APP-SERVER-IP>:8080
```

You should now see your updated webpage being served from Apache.

---
![Screenshot](./assets/Screenshot%202025-11-26%20180331.png)

---
![Screenshot](./assets/Screenshot%202025-11-26%20180432.png)

---

## ⭐ Summary of All Commands Used

### Apache Setup

```bash
sudo yum install httpd -y
sudo sed -i 's/^Listen 80/Listen 8080/' /etc/httpd/conf/httpd.conf
sudo systemctl restart httpd
sudo systemctl enable httpd
```

### Permissions

```bash
sudo chown -R sarah:sarah /var/www/html
```

### Git Commands on Storage Server

```bash
cd web
git status
git add index.html
git commit -m "First Deployment"
git push
```

### Deployment Script (Jenkins)

```bash
rsync -av --delete . /var/www/html/
```

---
![Screenshot](./assets/Screenshot%202025-11-26%20180752.png)

---
