# 🚀 Day 59: Fixing the Redis Deployment Issue in Kubernetes

## 🧩 Task Name
**Troubleshooting and Fixing a Redis Deployment**

### 🧾 Task Explanation
The Nautilus DevOps team had a Redis application deployed on the Kubernetes cluster.  
It was running fine until one of the team members made some incorrect changes, causing the Redis pods to fail and stop running.  
Our task was to identify the issue in the existing Redis deployment (`redis-deployment`) and fix it to bring the application back to a **Running** state.

---

## 📚 Basics to Know Before Starting

Before working on this task, it’s important to understand a few Kubernetes basics:

### 1. **Kubernetes Deployment**
A Deployment ensures that the specified number of pod replicas are running at all times.  
If something goes wrong, Kubernetes automatically recreates the pods to maintain the desired state.

Command:
```bash
kubectl get deployments
```

### 2. **Pods**
Pods are the smallest deployable units in Kubernetes that contain your containerized applications (like Redis).

Command:
```bash
kubectl get pods
```

### 3. **ConfigMaps**
A ConfigMap allows you to externalize configuration data from your container image.  
If a ConfigMap is referenced incorrectly, the pod will fail to start.

### 4. **Common Pod Errors**
- `ImagePullBackOff` → Wrong image name or unavailable image.
- `CrashLoopBackOff` → Application crashing repeatedly.
- `CreateContainerConfigError` → Missing ConfigMap or Secret.

---

## 🧠 Step-by-Step Commands and Explanations

### 🔹 Step 1: Check the Deployment
We first verify if the deployment exists and how many replicas it manages:
```bash
kubectl get deployments
```

Then, we inspect the details of the Redis deployment:
```bash
kubectl get deployment redis-deployment -o yaml
```

📘 *This helps us identify any misconfigurations, such as incorrect image names or missing ConfigMaps.*

---

### 🔹 Step 2: Check Pod Status
List all pods related to Redis:
```bash
kubectl get pods -l app=redis
```

If the pods are not running, describe them to see the error:
```bash
kubectl describe pod <pod-name>
```

💡 *This command shows detailed information such as event logs, image pull errors, or configuration issues.*

---

### 🔹 Step 3: Identify the Mistakes
From the deployment YAML, we found two mistakes:
```yaml
image: redis:alpin        # ❌ Wrong image tag
name: redis-cofig         # ❌ Typo in ConfigMap name
```

---

### 🔹 Step 4: Fix the Deployment
We can fix the deployment using:
```bash
kubectl edit deployment redis-deployment
```

Inside the editor, make the following corrections:
```yaml
image: redis:alpine       # ✅ Correct image tag
name: redis-config        # ✅ Correct ConfigMap name
```

Save and exit (`:wq`).

---

### 🔹 Step 5: Verify and Restart the Deployment
After editing, Kubernetes automatically rolls out the new configuration.  
We can check the rollout status:
```bash
kubectl rollout status deployment redis-deployment
```

Check pod status again:
```bash
kubectl get pods -l app=redis
```

All pods should now be **Running**.

---
![Screenshot 2](assets/Screenshot%202025-10-02%20144821.png)

---

## ⚙️ Challenges Faced

### 1. **Image Not Found**
When checking the pod status, the error `ImagePullBackOff` appeared.  
After inspecting the YAML, it was found that the image name was incorrectly spelled as `redis:alpin` instead of `redis:alpine`.

✅ **Solution:** Updated the image to `redis:alpine`.

---

### 2. **Missing ConfigMap**
Pods also failed because the ConfigMap name was misspelled (`redis-cofig`).

✅ **Solution:** Corrected the ConfigMap reference to `redis-config` in the YAML file.

---

## 🏁 Summary of Commands Used

| Purpose | Command |
|----------|----------|
| Get deployments | `kubectl get deployments` |
| Get pods | `kubectl get pods -l app=redis` |
| Describe pod details | `kubectl describe pod <pod-name>` |
| Get deployment YAML | `kubectl get deployment redis-deployment -o yaml` |
| Edit the deployment | `kubectl edit deployment redis-deployment` |
| Check rollout status | `kubectl rollout status deployment redis-deployment` |
| Verify final status | `kubectl get pods -l app=redis` |

---
![Screenshot 1](assets/Screenshot%202025-10-02%20151039.png)

---

## ✅ Conclusion
By analyzing the deployment YAML and identifying typos in the image name and ConfigMap reference, we were able to fix the Redis deployment and restore the pods to a **Running** state.  
This task demonstrated the importance of YAML accuracy and how powerful `kubectl describe` and `kubectl edit` can be in troubleshooting Kubernetes issues.

---
