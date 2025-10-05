# 🚀 Day 56 – Deploying a Static Website using Nginx on Kubernetes

## 🟢 Objective
Deploy a static website using the **Nginx** image in a **highly available and scalable** way by using a **Deployment** and exposing it through a **NodePort Service**.

---

## 🧠 Basic Concepts

### 1. Pod
- Smallest unit in Kubernetes. Runs your container (in this case, `nginx`).  
- Example: Running 1 nginx server.

### 2. Deployment
- A controller that manages multiple Pods for you.
- Ensures **high availability** (recreates pods if they fail) and **scalability** (easy to scale replicas).  
- Example: Running 3 nginx servers.

### 3. Replica
- The number of copies of a Pod.
- In this task → **3 replicas**.

### 4. Service
- Provides network access to your Pods.
- **NodePort** type service exposes your app to the outside world.

### 5. NodePort
- A special port range (30000–32767) on cluster nodes.
- In this task → **30011**.

---

## 🧩 Step-by-Step Implementation

### Step 1: Create Deployment YAML

Create a file named `nginx-deployment.yaml` and add the following content:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3               # number of pods
  selector:
    matchLabels:
      app: nginx
  template:                 # pod template
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
        ports:
        - containerPort: 80
```

### Step 2: Apply the Deployment

```bash
kubectl apply -f nginx-deployment.yaml
```

Check if the pods are running:

```bash
kubectl get pods
```

You should see **3 pods** like `nginx-deployment-xxxx`.

---

### Step 3: Create Service YAML

Create a file named `nginx-service.yaml` and add this:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80        # service port (inside cluster)
      targetPort: 80  # pod’s containerPort
      nodePort: 30011 # external port on node
```

### Step 4: Apply the Service

```bash
kubectl apply -f nginx-service.yaml
```

Check service status:

```bash
kubectl get svc
```

You’ll see `nginx-service` with NodePort `30011`.

---

### Step 5: Test It

Get your **node IP**:

```bash
kubectl get nodes -o wide
```

Access in browser or with curl:

```bash
curl http://<node-ip>:30011
```

If successful, you'll see the default **Nginx welcome page** 🎉

---
![Screenshot 1](assets/Screenshot%202025-09-30%20193220.png)

---

## 🧾 Summary of Commands

```bash
kubectl apply -f nginx-deployment.yaml
kubectl get pods
kubectl apply -f nginx-service.yaml
kubectl get svc
kubectl get nodes -o wide
curl http://<node-ip>:30011
```

---

✅ **Result:**  
A scalable, highly available Nginx web app exposed on NodePort `30011`.

---
![Screenshot 2](assets/Screenshot%202025-09-30%20193321.png)

---