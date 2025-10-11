# 🗓️ Day 62 — Kubernetes Secrets & Pod Mounting (KodeKloud Challenge)

## 📘 Introduction

The Nautilus DevOps team is working to deploy some tools in the Kubernetes cluster. Some of the tools are license-based, so their license information must be stored securely within the cluster. The team decided to use **Kubernetes Secrets** for this purpose.

In this challenge, we will:

- Create a **Secret** to store a license file.
- Create a **Pod** that mounts this secret into its filesystem.

---

## 🔑 Basics You Should Know Before Doing This Task

### 1. What is a Secret?

A **Secret** in Kubernetes is an object that stores sensitive data such as passwords, tokens, or keys. The data in a Secret is **base64 encoded**, not encrypted by default.

### 2. Why Use Secrets?

Secrets keep sensitive information out of Pod definitions or container images and are safer to handle.

### 3. Ways to Use Secrets

- As **environment variables**.
- As **mounted files** inside containers.

In this challenge, we will use **mounted file secrets**.

### 4. What is a Generic Secret?

A **generic secret** can be created from literal values, files, or directories using the `kubectl create secret generic` command.

---

## 🧩 Step-by-Step Solution

### Step 1: Create a Secret from the File

We already have a file `/opt/ecommerce.txt` containing the license key.  
Create a Kubernetes Secret using:

```bash
kubectl create secret generic ecommerce --from-file=/opt/ecommerce.txt
```

**Explanation:**

- `generic` → tells Kubernetes it’s a generic secret type.
- `ecommerce` → the name of the secret.
- `--from-file` → source file whose contents will be stored in the secret.

Verify the secret:

```bash
kubectl get secrets
kubectl describe secret ecommerce
```

---

### Step 2: Create a Pod that Mounts the Secret

Create a YAML file for the Pod:

```bash
vi secret-pod.yaml
```

Paste the following configuration:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: secret-nautilus
spec:
  containers:
    - name: secret-container-nautilus
      image: fedora:latest
      command: [ "sleep", "3600" ]
      volumeMounts:
        - name: secret-volume
          mountPath: /opt/apps
  volumes:
    - name: secret-volume
      secret:
        secretName: ecommerce
```

**Explanation:**

- `image: fedora:latest` → Fedora OS with latest tag.
- `command: ["sleep", "3600"]` → Keeps container running for 1 hour.
- `volumeMounts` → Mounts the secret volume inside `/opt/apps`.
- `secretName: ecommerce` → References the secret created in Step 1.

Save and exit (`ESC + :wq`).

Now apply the Pod:

```bash
kubectl apply -f secret-pod.yaml
```

Check pod status:

```bash
kubectl get pods
```

---

### Step 3: Verify Secret Mounting

Exec into the running Pod:

```bash
kubectl exec -it secret-nautilus -- /bin/bash
```

Inside the container, check the mounted path:

```bash
ls /opt/apps
cat /opt/apps/ecommerce.txt
```

You should see the same content from `/opt/ecommerce.txt` file.

---
![Screenshot 1](assets/Screenshot%202025-10-04%20221532.png)

---
![Screenshot 2](assets/Screenshot%202025-10-04%20221602.png)

---

## ⚠️ Common Issues & Fixes

| Issue | Cause | Solution |
|-------|--------|-----------|
| Secret not visible inside pod | Secret not mounted properly | Check volumeMounts and secretName in YAML |
| Pod stuck in `CrashLoopBackOff` | Wrong image or missing command | Ensure `fedora:latest` and `sleep` command exist |
| Secret shows base64 gibberish | Secrets are base64 encoded | Decode using `echo <value> | base64 --decode` |

---

## ✅ Summary of Commands

```bash
# Step 1: Create secret
kubectl create secret generic ecommerce --from-file=/opt/ecommerce.txt

# Step 2: Apply Pod YAML
kubectl apply -f secret-pod.yaml

# Step 3: Verify Pod is running
kubectl get pods

# Step 4: Check secret inside container
kubectl exec -it secret-nautilus -- /bin/bash
ls /opt/apps
cat /opt/apps/ecommerce.txt
```

---

## 🎯 Summary

In this challenge, you learned:

- How to securely store data using **Kubernetes Secrets**.
- How to **mount secrets** into Pods as files.
- How to verify the secret content from within a running container.

This forms a strong foundation for handling secure configurations in Kubernetes!

---
![Screenshot 3](assets/Screenshot%202025-10-04%20221642.png)

---
