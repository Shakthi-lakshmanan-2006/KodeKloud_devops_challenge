# Day 81: Jenkins Deployment Pipeline Automation

## 📝 Introduction
This task is about automating the deployment of a web application using **Jenkins Pipeline**.  
In simple words, we will:
- Update a file in a Gitea repository  
- Deploy it to a remote server using Jenkins  
- Test if the website is working after deployment  

This is a beginner‑friendly DevOps task that teaches SCM → Deployment → Testing automation.

---
![Screenshot](./assets/Screenshot%202025-12-01%20204052.png)

---

## 📌 Things You Should Know Before Starting
### 1. **Basic Git Commands**
- `git status`
- `git add`
- `git commit -m ""`
- `git push`
- Pulling changes: `git pull origin master`

### 2. **Remote Server Access**
You must know how to SSH:
```
ssh natasha@ststor01
```

### 3. **Jenkins Pipeline Basics**
- Pipeline has **stages**
- `sh ""` runs shell commands inside Jenkins
- Using `sshpass` to execute remote commands

### 4. **curl Testing**
To verify if the deployment worked:
```
curl -s -o /dev/null -w '%{http_code}' http://stlb01:8091
```
`200` = OK

---

## 🧾 Task Steps (From the Terminal Logs You Provided)

### ✅ **1. SSH into the server**
```
ssh natasha@ststor01
```

### ✅ **2. Go to web directory**
```
cd /var/www/html
git status
ls
```

### ✅ **3. Modify `index.html`**
```
vi index.html
```

### ✅ **4. Commit and push changes**
```
git add index.html
git commit -m "Update index.html"
git push
```

### 🔁 These changes update the repository for Jenkins to deploy.

---

## 🚀 Jenkins Pipeline Script Used

```groovy
pipeline {
    agent any

    stages {
        stage('Deploy') {
            steps {
                script {
                    sh 'sshpass -p "Bl@kW" ssh -o StrictHostKeyChecking=no natasha@ststor01 "cd /var/www/html && git pull origin master" '
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    def response_code = sh(script: "curl -s -o /dev/null -w '%{http_code}' http://stlb01:8091", returnStdout: true).trim()
                    if (response_code != '200') {
                        error("App not working after deployment. HttpCode: ${response_code}")
                    } else {
                        echo "App is working"
                    }
                }
            }
        }
    }
}
```

---
![Screenshot](./assets/Screenshot%202025-12-01%20204101.png)

---
![Screenshot](./assets/Screenshot%202025-12-01%20204245.png)

---

## 📌 Summary of Commands Used

### **Git Commands**
```
git status
git add index.html
git commit -m "Update index.html"
git push
```

### **Linux Navigation**
```
cd /var/www/html
ls
```

### **SSH Commands**
```
ssh natasha@ststor01
exit
```

### **Jenkins Remote Execution**
```
sshpass -p "password" ssh user@host "commands"
```

### **curl Testing**
```
curl -s -o /dev/null -w '%{http_code}' http://stlb01:8091
```

---

## 🎉 Final Note
This completes Day 81 KodeKloud Challenge—deploying and testing a web application using a Jenkins pipeline!

---
![Screenshot](./assets/Screenshot%202025-12-01%20204347.png)

---