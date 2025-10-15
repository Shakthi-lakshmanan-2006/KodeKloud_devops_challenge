# 🚀 Day 63: Iron Gallery App Deployment (Kubernetes Challenge)

## 🧠 Basics Before Starting

### 🧩 Namespace

A namespace is like a folder that helps organize Kubernetes resources separately.

Example:

```bash
kubectl create namespace my-namespace
```

### ⚙️ Deployment

A Deployment manages pods and ensures the desired number are running. It defines:

- Container image
- Replica count
- Labels and selectors
- Volumes

### 🧱 Pod and Volumes

- A Pod runs one or more containers.
- Volumes provide storage to containers.  
  - `emptyDir`: temporary storage deleted when pod stops.

### 💾 Environment Variables

You can define environment variables for containers (e.g., database credentials).

### 🌐 Service

A Service exposes a pod or set of pods:

- `ClusterIP`: internal access.
- `NodePort`: external access.

---

## 🪜 Steps to Complete the Task

### Step 1: Create Namespace

```bash
kubectl create namespace iron-namespace-datacenter
```

---

### Step 2: Create Iron Gallery Deployment

Create file `iron-gallery-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-gallery-deployment-datacenter
  namespace: iron-namespace-datacenter
  labels:
    run: iron-gallery
spec:
  replicas: 1
  selector:
    matchLabels:
      run: iron-gallery
  template:
    metadata:
      labels:
        run: iron-gallery
    spec:
      containers:
      - name: iron-gallery-container-datacenter
        image: kodekloud/irongallery:2.0
        resources:
          limits:
            memory: "100Mi"
            cpu: "50m"
        volumeMounts:
        - name: config
          mountPath: /usr/share/nginx/html/data
        - name: images
          mountPath: /usr/share/nginx/html/uploads
      volumes:
      - name: config
        emptyDir: {}
      - name: images
        emptyDir: {}
```

Apply it:

```bash
kubectl apply -f iron-gallery-deployment.yaml
```

---

### Step 3: Create Iron DB Deployment

Create file `iron-db-deployment.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iron-db-deployment-datacenter
  namespace: iron-namespace-datacenter
  labels:
    db: mariadb
spec:
  replicas: 1
  selector:
    matchLabels:
      db: mariadb
  template:
    metadata:
      labels:
        db: mariadb
    spec:
      containers:
      - name: iron-db-container-datacenter
        image: kodekloud/irondb:2.0
        env:
        - name: MYSQL_DATABASE
          value: database_web
        - name: MYSQL_ROOT_PASSWORD
          value: MyRootPass@123
        - name: MYSQL_PASSWORD
          value: MyDBPass@123
        - name: MYSQL_USER
          value: myuser
        volumeMounts:
        - name: db
          mountPath: /var/lib/mysql
      volumes:
      - name: db
        emptyDir: {}
```

Apply it:

```bash
kubectl apply -f iron-db-deployment.yaml
```

---

### Step 4: Create Iron DB Service

Create file `iron-db-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: iron-db-service-datacenter
  namespace: iron-namespace-datacenter
spec:
  selector:
    db: mariadb
  ports:
  - protocol: TCP
    port: 3306
    targetPort: 3306
  type: ClusterIP
```

Apply it:

```bash
kubectl apply -f iron-db-service.yaml
```

---

### Step 5: Create Iron Gallery Service

Create file `iron-gallery-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: iron-gallery-service-datacenter
  namespace: iron-namespace-datacenter
spec:
  selector:
    run: iron-gallery
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 32678
  type: NodePort
```

Apply it:

```bash
kubectl apply -f iron-gallery-service.yaml
```

---

### Step 6: Verify Everything

```bash
kubectl get all -n iron-namespace-datacenter
```

You should see:

- 2 Deployments
- 2 Pods
- 2 Services

---

### Step 7: Access the Application

If using KodeKloud environment:

- Get the node IP
- Open in browser:

```
http://<node-ip>:32678
```

✅ You should see the **Iron Gallery installation page**.

---
![Screenshot 1](assets/Screenshot%202025-10-05%20202703.png)

---

## 🔍 Summary of Commands

```bash
kubectl create namespace iron-namespace-datacenter
kubectl apply -f iron-gallery-deployment.yaml
kubectl apply -f iron-db-deployment.yaml
kubectl apply -f iron-db-service.yaml
kubectl apply -f iron-gallery-service.yaml
kubectl get all -n iron-namespace-datacenter
```

---

## ⚙️ Common Mistakes to Avoid

1. Forgetting to include the namespace in YAML files.
2. Typing wrong labels or selectors (they must match exactly).
3. Missing `nodePort` when creating the NodePort service.

---
![Screenshot 2](assets/Screenshot%202025-10-05%20203024.png)

---

🎯 **Goal:** If the Iron Gallery installation page appears,then your deployment is successful.!

--
