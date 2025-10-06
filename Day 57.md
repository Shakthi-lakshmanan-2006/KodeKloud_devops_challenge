
# 🧠 KodeKloud Challenge - Day 57: Environment Variables Pod Setup

## 🎯 Task Overview

The Nautilus DevOps team is working to set up some pre-requisites for an application that sends greetings to users. As part of the setup, we need to create a Kubernetes Pod that uses environment variables to print a message.

---

## 🧩 Basics You Should Know Before Starting

### 1. What is a Pod?
- A **Pod** is the smallest deployable unit in Kubernetes.
- It runs one or more containers (usually one).
- Here, we will create one Pod with a single container.

### 2. What is a Container Image?
- Containers are based on **images** that define what software and environment they contain.
- We will use the **`bash` image** so we can execute shell commands easily.

### 3. Environment Variables
- Environment variables allow you to store small bits of data inside your container.
- For example:
  ```yaml
  env:
    - name: GREETING
      value: "Welcome to"
  ```
- Inside the container, you can access it using `$GREETING`.

### 4. Commands in Kubernetes
- A container can be told to run a specific command instead of its default one.
- We’ll use this command:
  ```yaml
  command: ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
  ```
- `/bin/sh -c` tells the shell to run the command that follows.
- The `echo` command prints the values of the environment variables.

### 5. Restart Policy
- Normally, Kubernetes restarts a Pod if it stops.
- But here, our Pod should just run once and exit.
- So we’ll set:
  ```yaml
  restartPolicy: Never
  ```

---

## 🛠️ Step-by-Step Solution

### Step 1: Create the YAML file
Create a file called **`print-envars-greeting.yaml`** with the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: print-envars-greeting
spec:
  restartPolicy: Never
  containers:
    - name: print-env-container
      image: bash
      command: ["/bin/sh", "-c", 'echo "$(GREETING) $(COMPANY) $(GROUP)"']
      env:
        - name: GREETING
          value: "Welcome to"
        - name: COMPANY
          value: "Stratos"
        - name: GROUP
          value: "Group"
```

---

### Step 2: Apply the YAML
```bash
kubectl apply -f print-envars-greeting.yaml
```

This command tells Kubernetes to create the Pod defined in the file.

---

### Step 3: Check Pod Status
```bash
kubectl get pods
```
You should see:
```
NAME                    READY   STATUS      RESTARTS   AGE
print-envars-greeting   0/1     Completed   0          10s
```

---

### Step 4: View the Output
```bash
kubectl logs -f print-envars-greeting
```
Output should be:
```
Welcome to Stratos Group
```

---
![Screenshot 1](assets/Screenshot%202025-10-01%20223851.png)

---
![Screenshot 2](assets/Screenshot%202025-10-01%20223957.png)

---

## ⚠️ Challenges Faced and How I Solved Them

### 🧩 Issue 1: Kubeconfig Error
When trying to apply the YAML, I got:
```
error: error loading config file "/home/thor/.kube/config": read /home/thor/.kube/config: is a directory
```
#### Cause:
The kubeconfig file (which stores cluster access info) was missing or replaced by a directory.

#### Solution:
1. Tried checking `/root/.kube/config` – not found.
2. Searched the system:
   ```bash
   sudo find / -name config 2>/dev/null | grep kube
   ```
   Only `/home/thor/.kube/config` existed, but it was a directory.
3. Realized the lab environment was broken.
4. **Solution:** Restarted the KodeKloud lab using the **Reset** button on the top-right corner.
   This restored a valid kubeconfig file and made `kubectl` work again.

---

## 🧾 Summary of Commands

```bash
# Create YAML file
vi print-envars-greeting.yaml

# Apply configuration
kubectl apply -f print-envars-greeting.yaml

# Check pod status
kubectl get pods

# View output
kubectl logs -f print-envars-greeting

# (If kubeconfig broken)
sudo find / -name config 2>/dev/null | grep kube
# Then reset lab environment
```

---

## ✅ Final Output
```
Welcome to Stratos Group
```

---

## 💡 Key Takeaways
- Environment variables help pass dynamic values to containers.
- `restartPolicy: Never` is used for one-time jobs or scripts.
- Always check kubeconfig path when `kubectl` throws configuration errors.
- Resetting the lab often fixes broken environments quickly.

---
![Screenshot 3](assets/Screenshot%202025-10-01%20224532.png)

---
