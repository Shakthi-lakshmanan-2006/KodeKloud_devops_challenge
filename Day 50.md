# 🚀 Day 50 – Setting Resource Limits in Kubernetes Pods  

## 🔹 Basics You Should Know  

1. **Pod**  
   - A Pod is the smallest deployable unit in Kubernetes.  
   - It can run one or more containers.  
   - In this task, we’ll run a single container (`httpd:latest`).  

2. **Resource Requests & Limits**  
   - **Requests** = Minimum resources a Pod needs to run.  
     - If the node doesn’t have these, the Pod won’t be scheduled.  
   - **Limits** = Maximum resources the Pod is allowed to use.  
     - If it tries to use more, Kubernetes will restrict it.  
   - Units:  
     - **Memory** is written in `Mi` (Mebibytes).  
     - **CPU** is written in `m` (millicores). For example, `100m = 0.1 CPU core`.  

3. **Why use them?**  
   - To prevent one container from consuming too many resources.  
   - To keep applications running smoothly.  
   - To share resources fairly among all Pods.  

---

## 🔹 Steps & Commands  

We’ll use a **YAML file** to define the Pod.

---

### 1️⃣ Create the YAML file  

```bash
vi httpd-pod.yaml
```

👉 This opens a new file named `httpd-pod.yaml` in the editor.  
Inside this file, paste the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: httpd-pod
spec:
  containers:
    - name: httpd-container
      image: httpd:latest
      resources:
        requests:
          memory: "15Mi"
          cpu: "100m"
        limits:
          memory: "20Mi"
          cpu: "100m"
```

Then save and exit (`:wq` in `vi`).  

---

### 2️⃣ Apply the YAML to create the Pod  

```bash
kubectl apply -f httpd-pod.yaml
```

👉 This command tells Kubernetes to create the Pod using the instructions in the file.  

---

### 3️⃣ Check if the Pod is running  

```bash
kubectl get pods
```

👉 This shows the list of Pods and their status. Look for `httpd-pod` and check if it’s **Running**.  

---

### 4️⃣ Describe the Pod to see resource settings  

```bash
kubectl describe pod httpd-pod
```

This gives detailed information about the Pod, including the **resource requests and limits** we will set.  

---
![Pod YAML](assets/Screenshot%202025-09-25%20185957.png)

---
![Pod Created](assets/Screenshot%202025-09-25%20190017.png)

---

## 🔹 Summary of Commands  

```bash
# Step 1: Create YAML file
vi httpd-pod.yaml

# Step 2: Apply YAML file
kubectl apply -f httpd-pod.yaml

# Step 3: Verify pod status
kubectl get pods

# Step 4: Check details of pod
kubectl describe pod httpd-pod
```

---
![Task Completion Confirmation](assets/Screenshot%202025-09-25%20190141.png)

---
