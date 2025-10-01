# Day 52: Kubernetes Deployment Rollback  

## 🟢 Basics You Should Know  

1. **Deployment** → A Deployment in Kubernetes manages Pods and ReplicaSets. It ensures your app runs with the correct number of pods and the right container image.  

2. **Revision** → Every time you update a Deployment (for example, change the image), Kubernetes saves a new **revision**. Think of revisions like saved checkpoints.  

3. **Rollback** → If the latest update has a bug, you can easily **rollback** to the previous stable version using Kubernetes commands.  

4. **Key Commands** →  
   - `kubectl rollout history` → See revision history.  
   - `kubectl rollout undo` → Rollback to previous revision.  
   - `kubectl rollout status` → Verify rollback success.  

---

## 🟢 Steps with Commands (Beginner Friendly)  

### Step 1: Check Deployment Rollout History  
See the history of revisions for the deployment:  
```bash
kubectl rollout history deployment/nginx-deployment
```

👉 This will show the different revisions. The latest one is the buggy release.  

---

### Step 2: Rollback to the Previous Revision  
Run this command to revert the deployment back to the last stable version:  
```bash
kubectl rollout undo deployment/nginx-deployment
```

👉 This automatically rolls back to the **previous revision**.  

---

### Step 3: Verify the Rollback Status  
Check if the rollback was successful:  
```bash
kubectl rollout status deployment/nginx-deployment
```

👉 You should see that the deployment has been successfully rolled out.  

---
![Rollout History](assets/Screenshot%202025-09-26%20203011.png)

---

## 🟢 Summary of Commands  

```bash
# Check history of revisions
kubectl rollout history deployment/nginx-deployment

# Rollback to the previous revision
kubectl rollout undo deployment/nginx-deployment

# Verify rollback status
kubectl rollout status deployment/nginx-deployment
```

---
![Rollback Success](assets/Screenshot%202025-09-26%20203052.png)

---
