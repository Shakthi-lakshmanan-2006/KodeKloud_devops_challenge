# Day 78 – Jenkins Conditional Pipeline

## 📝 Title: Configuring a Conditional Deployment Pipeline in Jenkins

### 📌 Simple Explanation  
In this task, you create a Jenkins **Pipeline job** that deploys code based on which **Git branch** you choose.  
If you select **master**, it deploys the master branch.  
If you select **feature**, it deploys the feature branch.  
This is called a **conditional pipeline**, because the steps change depending on your input.

---

## 📚 What You Should Know Before Doing This Task

### ✅ 1. **What is Jenkins?**  
Jenkins is an automation server used for CI/CD (Continuous Integration and Deployment).

### ✅ 2. **What is an Agent/Node?**  
A secondary machine where Jenkins runs jobs.  
Here, you create a node called **Storage Server**.

### ✅ 3. **What is a Pipeline?**  
A pipeline contains steps that define how your application is built or deployed.

### ✅ 4. **What are Parameters?**  
They allow users to input values (like selecting a branch to deploy).

### ✅ 5. **Basic Git Knowledge**  
You must understand:  
- `git checkout`  
- `git pull`  
- `git fetch`  

---
![Screenshot](./assets/Screenshot%202025-11-01%20194921.png)

---

## 🚀 Steps to Perform the Task

### **1. Install Pipeline Plugin**  
Go to:  
**Manage Jenkins → Plugins → Available → Search "Pipeline" → Install**

---

### **2. Configure Jenkins URL**  
Go to:  
**Manage Jenkins → System → Jenkins URL**  
Set it to:  
`http://jenkins:8080/`

---

### **3. Create a New Node**
Go to:  
**Manage Jenkins → Nodes → New Node**

- Name: **Storage Server**  
- Type: **Permanent Agent**  
- Remote directory: `/var/www/html`  
- Label: `ststor01`  
- Save

---

### **4. Connect the Node to Jenkins**
On the Storage Server machine:

```
curl -sO http://jenkins:8080/jnlpJars/agent.jar
ls
screen -S jenkins
sudo java -jar agent.jar -url http://jenkins:8080/ -secret 32eb726f5727ead1b8cdb71dd492639155ac39fa959f30af49f495256e0404e0 -name "Storage Server" -webSocket -workDir "/var/www/html"
```

---

### **5. Prepare Git Directory**

```
cd /var/www/html/
ls
git status
git config --global --add safe.directory /var/www/html
sudo !!
git status
git branch
```

---

### **6. Create Jenkins Pipeline Job**

- Dashboard → **New Item**
- Name: `nautilus-webapp-job`
- Choose: **Pipeline**
* Check: **This project is parameterized**
- Add: **String Parameter**
  - Name: `BRANCH`
  - Default: `master`

---

### **7. Add the Pipeline Script**

```
pipeline {
    agent { label 'ststor01' }

    parameters {
        string(name: 'BRANCH', defaultValue: 'master', description: 'Branch name')
    }

    stages {
        stage('Deploy') {
            steps {
                script {
                    echo 'Starting Deployment'
                    if(params.BRANCH == 'master') {
                        echo 'Deploy on master branch'
                        sh '''
                            cd /var/www/html
                            git fetch origin
                            git checkout master
                            git pull origin master
                        '''
                    } else if(params.BRANCH == 'feature') {
                        echo 'Deploy on feature branch'
                        sh '''
                            cd /var/www/html
                            git fetch origin
                            git checkout feature
                            git pull origin feature
                        '''
                    } else {
                        echo 'Nothing to do'
                    }
                }
            }
        }
    }
}
```

---
![Screenshot](./assets/Screenshot%202025-11-01%20194759.png)

---
![Screenshot](./assets/Screenshot%202025-11-01%20194829.png)

---

## 🔍 Summary of Commands

### **Agent Setup**
```
curl -sO http://jenkins:8080/jnlpJars/agent.jar
screen -S jenkins
sudo java -jar agent.jar -url http://jenkins:8080/ -secret <secret> -name "Storage Server" -webSocket -workDir "/var/www/html"
```

### **Git Commands**
```
cd /var/www/html/
git status
git config --global --add safe.directory /var/www/html
git fetch origin
git checkout <branch>
git pull origin <branch>
```

---

## ✅ Output  
A Jenkins job that deploys code from either **master** or **feature** based on the parameter you choose.

---
![Screenshot](./assets/Screenshot%202025-11-01%20195052.png)

---
