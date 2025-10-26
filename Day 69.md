
# 🧩 Day 69: Install Jenkins Plugins (Git & GitLab)

## 🧠 Task Explanation

The **Nautilus DevOps** team has recently set up a **Jenkins server** to automate CI/CD workflows.  
Before using it for automation jobs, some essential plugins need to be installed — particularly **Git** and **GitLab** plugins.  
These plugins allow Jenkins to integrate with **Git repositories** (to pull code) and **GitLab servers** (for CI/CD pipelines).

Your task is to log in to Jenkins, install the required plugins, and ensure that they are properly installed and active.

---

## 🏗️ Basic Concepts Before Starting

Before performing the task, understand the following key points:

### 1. Jenkins Plugins

Plugins extend Jenkins functionality. Each plugin adds new features such as:

- Integrations with tools like **Git**, **GitLab**, **Docker**, etc.
- UI enhancements.
- Build and deployment capabilities.

### 2. Git Plugin

The **Git plugin** allows Jenkins to clone, pull, and interact with Git repositories during builds.

### 3. GitLab Plugin

The **GitLab plugin** enables Jenkins to integrate with GitLab CI/CD pipelines — it allows Jenkins to trigger jobs based on GitLab commits, webhooks, and merges.

### 4. Jenkins Plugin Installation Paths

You can install plugins from:

- **Manage Jenkins → Plugins → Available Plugins tab**
- Using Jenkins CLI or terminal commands (advanced).

---
![Screenshot](./assets/Screenshot%202025-10-21%20162045.png)

---

## ⚙️ Steps to Complete the Task

Below are the exact steps to complete this Jenkins plugin installation task.

```bash
# Step 1: Access Jenkins UI
# Click on the 'Jenkins' button in the KodeKloud lab environment.
# Login with the following credentials:
Username: admin
Password: Adm!n321

# Step 2: Open Plugin Management
# In the Jenkins dashboard:
# - Click on "Manage Jenkins" (on the left side menu)
# - Then select "Plugins" (on the right side options)

# Step 3: Install Git and GitLab Plugins
# - Go to the 'Available plugins' tab.
# - In the search box, type "Git" → select it.
# - Similarly, type "GitLab" → select it.
# - Click 'Install without restart' or 'Download now and install after restart'.

# Step 4: Verify Installation
# - Once installation completes, go to the "Installed plugins" tab.
# - Check that both 'Git' and 'GitLab' plugins are listed.
# - If GitLab-related plugins fail, re-search them under 'Available' and reinstall.

# Step 5: Restart Jenkins if Required
# Some plugin installations need a Jenkins restart.
# - Choose the option: "Restart Jenkins when installation is complete and no jobs are running".
# - Wait for Jenkins to restart and re-login.

# Step 6: Final Check
# After login, recheck "Manage Jenkins → Plugins → Installed" to confirm both plugins are active.
```

---
![Screenshot](./assets/Screenshot%202025-10-21%20162034.png)

---

## ✅ Summary

| Step | Description |
|------|--------------|
| 1 | Logged into Jenkins using provided credentials |
| 2 | Accessed Manage Jenkins → Plugins |
| 3 | Installed Git and GitLab plugins |
| 4 | Verified installation success |
| 5 | Restarted Jenkins service if required |
| 6 | Confirmed both plugins are active and functional |

> 🎯 **End Goal:** Jenkins should have **Git** and **GitLab** plugins installed and visible under Installed Plugins.

---

### 💡 Additional Tips

- Always verify plugin compatibility with your Jenkins version.
- Restart Jenkins after installing major integrations like GitLab.
- If plugin installation fails, check Jenkins logs (`Manage Jenkins → System Log`).

---
![Screenshot](./assets/Screenshot%202025-10-21%20162121.png)

---
