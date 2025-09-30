# Day 51: Rolling Update of Nginx Deployment

## ✅ What they want in this task
You already have a **Kubernetes Deployment** running called `nginx-deployment`.

- This deployment is using the **nginx image** (currently an older version).
- The Dev team has released a **new image: `nginx:1.17`** with updates.
- Your job is to **update the deployment** to use this new image.
- You must use a **rolling update** (the default behavior in Kubernetes deployments).
- Finally, you need to ensure that **all Pods are running successfully** after the update.

---

## 🧑‍🏫 Basics you should know before doing this task

1. **Deployment in Kubernetes**
   - A **Deployment** manages Pods and ReplicaSets.
   - It allows you to **update applications** without downtime using a **rolling update strategy** (default).

2. **Rolling Update**
   - A rolling update replaces old Pods with new ones gradually.
   - This means your app stays available while updating.
   - Example: If 3 Pods are running, Kubernetes will terminate one old Pod and start one new Pod until all are updated.

3. **kubectl set image** command
   - You use this to change the container image inside a deployment.
   - Format:
     ```bash
     kubectl set image deployment/<deployment-name> <container-name>=<new-image>
     ```

4. **kubectl rollout status** command
   - Used to check whether the rolling update is completed successfully.

5. **kubectl get pods**
   - To confirm the Pods are running and using the updated image.

---

## 📝 Steps with Beginner-Friendly Explanation

### 🔹 Step 1: Check existing deployment
See if the `nginx-deployment` is running and what image it’s using.
```bash
kubectl get deployments
```

To check details of the deployment (image version currently running):
```bash
kubectl describe deployment nginx-deployment
```

---

### 🔹 Step 2: Perform the rolling update
Update the deployment to use the new image `nginx:1.17`.
```bash
kubectl set image deployment/nginx-deployment nginx=nginx:1.17
```

---

### 🔹 Step 3: Verify rollout status
Check if the update is progressing:
```bash
kubectl rollout status deployment/nginx-deployment
```

---

### 🔹 Step 4: Verify Pods are updated
List the Pods to ensure they are all running with the new image:
```bash
kubectl get pods -o wide
```

---

### 🔹 Step 5: Double-check image version
Confirm that Pods are indeed using the new image:
```bash
kubectl describe deployment nginx-deployment | grep Image
```

---
![Rolling Update Terminal Output](assets/Screenshot%202025-09-26%20202149.png)

---

## 📌 Summary of Commands Used
```bash
kubectl get deployments
kubectl describe deployment nginx-deployment
kubectl set image deployment/nginx-deployment nginx=nginx:1.17
kubectl rollout status deployment/nginx-deployment
kubectl get pods -o wide
kubectl describe deployment nginx-deployment | grep Image
```
