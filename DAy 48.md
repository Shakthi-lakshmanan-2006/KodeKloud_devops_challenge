# 💻 Day 48: Create a Kubernetes Pod with Nginx and Set Labels

This guide covers the fundamental Kubernetes operation of creating a single Pod, setting its container image to **Nginx**, and attaching **Labels** for easy identification and selection.

## 🚀 **Basics You Should Know**

| Concept | Description |
| :--- | :--- |
| **Kubernetes Pod** | The smallest deployable unit; runs one or more containers. |
| **Docker Images** | Containers are built from images (e.g., `nginx:latest`). |
| **Labels** | Key-value pairs (e.g., `app=nginx_app`) attached to objects for filtering and grouping. |
| **kubectl** | The command-line tool for interacting with your Kubernetes cluster. |
| **Namespace** | The scope for Kubernetes resources (default is `default`). |

***

## 🛠️ **Commands / Steps to Complete the Task**

### **Step 1: Create the Pod**

We use the `kubectl run` command to quickly create a single Pod. The `--restart=Never` flag is crucial here as it tells Kubernetes to create a standalone Pod rather than a Deployment.

| Command Part | Purpose |
| :--- | :--- |
| `kubectl run pod-nginx` | Initiates the creation of a Pod named **`pod-nginx`**. |
| `--image=nginx:latest` | Specifies the container image to use: the latest **Nginx** image from Docker Hub. |
| `--labels="app=nginx_app"` | Attaches the label **`app=nginx_app`** to the Pod. |
| `--restart=Never` | Instructs Kubernetes to create a single **Pod** object (not a Deployment or other controller). |
| `--container-name=nginx-container` | Sets the name of the container *inside* the Pod to **`nginx-container`**. |

```bash
kubectl run pod-nginx \
  --image=nginx:latest \
  --labels="app=nginx_app" \
  --restart=Never \
  --container-name=nginx-container
```

  ---

### **Step 2: Verify the Pod**

Check the status of the newly created Pod. Look for the Pod named `pod-nginx` to have a status of **`Running`**.

```bash
kubectl get pods
```

---

### **Step 3: Describe the Pod (Optional, for details)** 📝

Use the `kubectl describe` command to retrieve detailed configuration information, including the **Labels**, **Image**, and **Container Name**, and recent **Events** related to the Pod's lifecycle. This is the primary tool for debugging.

```bash
kubectl describe pod pod-nginx
```

***
![Screenshot 2025-09-23 233252](assets/Screenshot%202025-09-23%20233252.png)

---

## ✅ **Summary of Commands Used**

This table provides a quick reference for the commands executed in this task:

| Command | Purpose | 
| :--- | :--- |
| `kubectl run pod-nginx --image=nginx:latest --labels="app=nginx_app" --restart=Never --container-name=nginx-container` | Create Pod with Nginx container, labels, and container name. |
| `kubectl get pods` | Check the current status of the Pod. |
| `kubectl describe pod pod-nginx` | View detailed configuration and events for the Pod. |

---
![Screenshot 2025-09-23 232950](assets/Screenshot%202025-09-23%20232950.png)

---