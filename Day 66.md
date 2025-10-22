# 🗓️ Day 66 — Deploying MySQL on Kubernetes

## 🎯 Task Title: 
**Deploy a MySQL Database with Persistent Storage and Secrets**

---

## 🧠 Introduction

In this task, you’ll deploy a **MySQL database** on a **Kubernetes cluster**. The setup will involve persistent storage for data, secrets for credentials, and a service to access MySQL externally.

This is a real-world DevOps use case — databases **must not lose data** when containers restart, so you’ll use **Persistent Volumes (PV)** and **Persistent Volume Claims (PVC)**.  
You’ll also use **Secrets** to protect sensitive information like passwords instead of writing them directly in YAML.

---

## ⚙️ Basics You Should Know Before Doing This Task

### 🧩 1. Persistent Volume (PV)

A **Persistent Volume** is a storage resource in the cluster.  
It’s like a hard drive you make available to pods.

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 250Mi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

---

### 🧱 2. Persistent Volume Claim (PVC)

A **PVC** is a request for storage from a pod.  
It automatically binds to a matching PV.

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi
```

---

### 🔐 3. Secrets

Kubernetes **Secrets** store confidential data like passwords or tokens.

```bash
kubectl create secret generic mysql-root-pass --from-literal=password=YUIidhb667
```

You can reference secrets as **environment variables** inside your container.

---

### 📦 4. Deployment

A **Deployment** manages pods.  
For MySQL, it ensures your database container runs with persistent storage and required environment variables.

---

### 🌐 5. Service

A **Service** exposes your pod to the network.  
Here, you’ll use a **NodePort** service to make MySQL accessible externally on a specific port (30007).

---

## 🪜 Step-by-Step Implementation

### Step 1️⃣ – Create Persistent Volume (PV)

`mysql-pv.yaml`

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 250Mi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/mnt/data"
```

Command:
```bash
kubectl apply -f mysql-pv.yaml
```

---

### Step 2️⃣ – Create Persistent Volume Claim (PVC)

`mysql-pvc.yaml`

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pv-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 250Mi
```

Command:
```bash
kubectl apply -f mysql-pvc.yaml
```

---

### Step 3️⃣ – Create Secrets

```bash
kubectl create secret generic mysql-root-pass --from-literal=password=YUIidhb667

kubectl create secret generic mysql-user-pass --from-literal=username=kodekloud_tim --from-literal=password=BruCStnMT5

kubectl create secret generic mysql-db-url --from-literal=database=kodekloud_db7
```

---

### Step 4️⃣ – Create Deployment

`mysql-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql:5.7
          ports:
            - containerPort: 3306
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-root-pass
                  key: password
            - name: MYSQL_DATABASE
              valueFrom:
                secretKeyRef:
                  name: mysql-db-url
                  key: database
            - name: MYSQL_USER
              valueFrom:
                secretKeyRef:
                  name: mysql-user-pass
                  key: username
            - name: MYSQL_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-user-pass
                  key: password
          volumeMounts:
            - name: mysql-storage
              mountPath: /var/lib/mysql
      volumes:
        - name: mysql-storage
          persistentVolumeClaim:
            claimName: mysql-pv-claim
```

Command:
```bash
kubectl apply -f mysql-deployment.yaml
```

---

### Step 5️⃣ – Create NodePort Service

`mysql-service.yaml`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  type: NodePort
  selector:
    app: mysql
  ports:
    - port: 3306
      targetPort: 3306
      nodePort: 30007
```

Command:
```bash
kubectl apply -f mysql-service.yaml
```

---

## 🔍 Verification Commands

```bash
kubectl get pv
kubectl get pvc
kubectl get secrets
kubectl get pods
kubectl get svc
kubectl logs deployment/mysql-deployment
kubectl describe pvc mysql-pv-claim
```

---

## ⚠️ Challenges You Might Face

| Challenge | Cause | Solution |
|------------|--------|-----------|
| PVC not bound | Mismatch in storage size or access mode | Ensure PV and PVC both request 250Mi and use same access mode (`ReadWriteOnce`). |
| MySQL pod in crash loop | Missing secrets or wrong keys | Verify secret names and keys with `kubectl describe secret <name>`. |
| Port not accessible | Service type or nodePort issue | Check service type is `NodePort` and nodePort = 30007. |

---

## 🧾 Summary of Commands Used

```bash
# 1. Create PV and PVC
kubectl apply -f mysql-pv.yaml
kubectl apply -f mysql-pvc.yaml

# 2. Create Secrets
kubectl create secret generic mysql-root-pass --from-literal=password=YUIidhb667
kubectl create secret generic mysql-user-pass --from-literal=username=kodekloud_tim --from-literal=password=BruCStnMT5
kubectl create secret generic mysql-db-url --from-literal=database=kodekloud_db7

# 3. Create Deployment and Service
kubectl apply -f mysql-deployment.yaml
kubectl apply -f mysql-service.yaml

# 4. Verify
kubectl get all
```

---

✅ **End of Day 66 — MySQL on Kubernetes**