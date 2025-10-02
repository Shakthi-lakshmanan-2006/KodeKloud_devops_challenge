# 🚀 Task 53: Debugging Nginx + PHP-FPM with ConfigMap in Kubernetes

Today in my **100 Days of DevOps Challenge (Day 53)**, I worked on
troubleshooting an issue with **Nginx + PHP-FPM setup** inside a
Kubernetes cluster.\
The challenge was to **identify and fix a broken configuration** and
then verify the PHP application is working.

------------------------------------------------------------------------

## 📝 Task Explanation (Beginner Level)

The setup had: - A **Pod** named `nginx-phpfpm` running two containers
(`nginx` and `php-fpm`). - A **ConfigMap** named `nginx-config` that
stores configuration for the Nginx server. - A PHP file (`index.php`)
that needed to be served properly.

The website was not working because the **ConfigMap was not mounted
properly**. My goal was to fix this issue and make sure Nginx serves the
PHP file.

------------------------------------------------------------------------

## 📚 Basics You Should Know Before Doing This Task

1. **What is a ConfigMap?**\
    A ConfigMap in Kubernetes is used to store configuration data (like
    Nginx config files) that pods can use.

2. **Multi-container Pod:**\
    A single pod can run multiple containers (here we have
    `nginx-container` and `php-fpm-container`). They usually share the
    same storage/network.

3. **kubectl cp:**\
    This command copies files from your local machine (jump host) to a
    container inside a pod.

4. **kubectl exec:**\
    This allows you to open a shell or run commands inside a running
    container.

------------------------------------------------------------------------

## 💻 Step-by-Step Commands (With Beginner Explanations)

### 1. Copy the PHP file into the Nginx container

We need to copy `/home/thor/index.php` from the jump host to the pod's
Nginx container document root (`/var/www/html`):

``` bash
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html -c nginx-container
```

👉 **Explanation:**\

- `kubectl cp` = copy command for Kubernetes.\
- `/home/thor/index.php` = source file on the jump host.\
- `nginx-phpfpm:/var/www/html` = destination inside the pod.\
- `-c nginx-container` = specifies which container inside the pod we are
copying to.

------------------------------------------------------------------------

### 2. Verify inside the container

To make sure the file is copied correctly, open a shell inside the Nginx
container:

``` bash
kubectl exec -it nginx-phpfpm -c nginx-container -- sh
```

👉 **Explanation:**\

- `kubectl exec` = run a command inside a pod.\
- `-it` = interactive terminal (so we can type commands).\
- `-- sh` = opens a shell inside the container.

Then, check the file:

``` bash
cd /var/www/html
ls
cat index.php
```

👉 You should see the `index.php` file with PHP code inside.

---
![Task Completion Screenshot 2](assets/Screenshot%202025-09-29%20195537.png)

---
![Task Completion Screenshot 3](assets/Screenshot%202025-09-29%20195550.png)

---

## 🏁 Summary of Commands

``` bash
kubectl cp /home/thor/index.php nginx-phpfpm:/var/www/html -c nginx-container
kubectl exec -it nginx-phpfpm -c nginx-container -- sh
cd /var/www/html
ls
cat index.php
```

✅ With these steps, the **website is back online** using Nginx +
PHP-FPM, and PHP is being served correctly! 🎉

------------------------------------------------------------------------

💡 Key takeaway: This task taught me how important **ConfigMaps and
multi-container debugging** are in Kubernetes.

# DevOps #Kubernetes #Nginx #PHP #Docker #KodeKloud #100DaysOfDevOps

---

![Task Completion Screenshot 1](assets/Screenshot%202025-09-29%20200017.png)
---
