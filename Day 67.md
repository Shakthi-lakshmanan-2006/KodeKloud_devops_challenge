# 🗓️ Day 67: Guestbook Application Deployment on Kubernetes

## 🧾 Introduction

In this task, you’ll deploy a **multi-tier Guestbook application** on a **Kubernetes cluster**.  
It has:

- A **Redis backend** (Master + Slave)
- A **PHP frontend**  

The Redis master handles write operations, while Redis slaves handle read operations.  
The frontend interacts with Redis via services to display and store guestbook entries.  

---

## ⚙️ Basics You Should Know Before Starting

### 🧱 1. Deployments

A **Deployment** manages replicated Pods, ensuring desired replicas are always running.

### 🌐 2. Services

A **Service** provides stable network access to pods.  
Types used here:

- **ClusterIP** → internal access (for Redis)
- **NodePort** → external access (for frontend)

### 🧩 3. Environment Variables

Used to configure pod behavior (e.g., how frontend connects to Redis).

### ⚡ 4. Resource Requests

```yaml
resources:
  requests:
    cpu: "100m"
    memory: "100Mi"
```

### 📦 5. Redis Ports

Redis default port: **6379**

---

## 🪜 Steps to Complete the Task

### Step 1: Redis Master Deployment

`redis-master-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-master
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
      role: master
  template:
    metadata:
      labels:
        app: redis
        role: master
    spec:
      containers:
      - name: master-redis-xfusion
        image: redis
        ports:
        - containerPort: 6379
        resources:
          requests:
            cpu: "100m"
            memory: "100Mi"
```

Apply and verify:

```bash
kubectl apply -f redis-master-deployment.yaml
kubectl get pods -l app=redis,role=master
```

---

### Step 2: Redis Master Service

`redis-master-service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-master
spec:
  selector:
    app: redis
    role: master
  ports:
  - port: 6379
    targetPort: 6379
```

```bash
kubectl apply -f redis-master-service.yaml
kubectl get svc redis-master
```

---

### Step 3: Redis Slave Deployment

`redis-slave-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-slave
spec:
  replicas: 2
  selector:
    matchLabels:
      app: redis
      role: slave
  template:
    metadata:
      labels:
        app: redis
        role: slave
    spec:
      containers:
      - name: slave-redis-xfusion
        image: gcr.io/google_samples/gb-redisslave:v3
        ports:
        - containerPort: 6379
        env:
        - name: GET_HOSTS_FROM
          value: dns
        resources:
          requests:
            cpu: "100m"
            memory: "100Mi"
```

```bash
kubectl apply -f redis-slave-deployment.yaml
kubectl get pods -l app=redis,role=slave
```

---

### Step 4: Redis Slave Service

`redis-slave-service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-slave
spec:
  selector:
    app: redis
    role: slave
  ports:
  - port: 6379
    targetPort: 6379
```

```bash
kubectl apply -f redis-slave-service.yaml
kubectl get svc redis-slave
```

---

### Step 5: Frontend Deployment

`frontend-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontend
spec:
  replicas: 3
  selector:
    matchLabels:
      app: guestbook
      tier: frontend
  template:
    metadata:
      labels:
        app: guestbook
        tier: frontend
    spec:
      containers:
      - name: php-redis-xfusion
        image: gcr.io/google-samples/gb-frontend@sha256:a908df8486ff66f2c4daa0d3d8a2fa09846a1fc8efd65649c0109695c7c5cbff
        ports:
        - containerPort: 80
        env:
        - name: GET_HOSTS_FROM
          value: dns
        resources:
          requests:
            cpu: "100m"
            memory: "100Mi"
```

```bash
kubectl apply -f frontend-deployment.yaml
kubectl get pods -l app=guestbook
```

---

### Step 6: Frontend Service

`frontend-service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: frontend
spec:
  type: NodePort
  selector:
    app: guestbook
    tier: frontend
  ports:
  - port: 80
    targetPort: 80
    nodePort: 30009
```

```bash
kubectl apply -f frontend-service.yaml
kubectl get svc frontend
```

---

## 🔍 Verification

```bash
kubectl get deployments
kubectl get svc
kubectl get pods
```

Then open the **App button** or access it via:

```
http://<NodeIP>:30009
```

---

![Screenshot 1](./assets/Screenshot%202025-10-10%20205310.png)

---

![Screenshot 2](./assets/Screenshot%202025-10-10%20205333.png)

---

## ⚠️ Common Issues & Fixes

| Issue | Cause | Fix |
|-------|--------|-----|
| Pods not running | Wrong image or label mismatch | Verify image name and labels |
| Frontend not loading | Wrong nodePort or selector | Ensure NodePort=30009 and labels match |
| Redis connection error | Missing GET_HOSTS_FROM | Ensure env variable exists |

---

## 📜 Summary Commands

```bash
kubectl apply -f redis-master-deployment.yaml
kubectl apply -f redis-master-service.yaml
kubectl apply -f redis-slave-deployment.yaml
kubectl apply -f redis-slave-service.yaml
kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml

kubectl get all
```

---
![Screenshot 3](./assets/Screenshot%202025-10-10%20205448.png)

---
