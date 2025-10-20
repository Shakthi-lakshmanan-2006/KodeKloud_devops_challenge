# 🚀 Redis Deployment on Kubernetes (KodeKloud Lab)

This is a **tested 100% working step-by-step guide** for completing the **Redis Deployment task** on **KodeKloud Kubernetes Lab environment**.

---

## 🧩 Step 1: Create the ConfigMap

The Redis config map needs to store a setting: `maxmemory 2mb`.

Run this command on the jump host terminal:

```bash
kubectl create configmap my-redis-config --from-literal=redis-config="maxmemory 2mb"
```

✅ This creates a ConfigMap named **my-redis-config** with a key-value pair:

```
redis-config = maxmemory 2mb
```

You can verify:

```bash
kubectl get configmap my-redis-config -o yaml
```

---

## 🧱 Step 2: Create the Redis Deployment YAML File

Now create a file called **redis-deployment.yaml**:

```bash
vi redis-deployment.yaml
```

Paste the following YAML code exactly:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
  template:
    metadata:
      labels:
        app: redis
    spec:
      containers:
      - name: redis-container
        image: redis:alpine
        ports:
        - containerPort: 6379
        resources:
          requests:
            cpu: "1"
        volumeMounts:
        - name: data
          mountPath: /redis-master-data
        - name: redis-config
          mountPath: /redis-master
      volumes:
      - name: data
        emptyDir: {}
      - name: redis-config
        configMap:
          name: my-redis-config
```

Save and exit:
```bash
Press Esc, then type :wq and press Enter
```

---

## 🚀 Step 3: Apply the Deployment

Run:

```bash
kubectl apply -f redis-deployment.yaml
```

✅ This creates the deployment with:

- 1 replica  
- Image `redis:alpine`  
- Container name `redis-container`  
- CPU request of 1  
- Volumes and port configuration as required  

---

## 🧠 Step 4: Verify the Deployment

Check deployment status:

```bash
kubectl get deployments
```

Check pods:

```bash
kubectl get pods -o wide
```

✅ Make sure the pod status shows **Running**.

You can also describe it to check details:

```bash
kubectl describe pod <pod-name>
```

---

## 🔍 Step 5: Verify ConfigMap Mount (Optional Check)

To confirm the config is mounted correctly:

Get the pod name:

```bash
kubectl get pods
```

Then check the config inside the pod:

```bash
kubectl exec -it <pod-name> -- cat /redis-master/redis-config
```

Expected output:

```
maxmemory 2mb
```

---

![Screenshot 1](./assets/WhatsApp%20Image%202025-10-08%20at%2010.15.30%20PM%20(1).jpeg)

---

![Screenshot 2](./assets/WhatsApp%20Image%202025-10-08%20at%2010.15.30%20PM.jpeg)

---

## ✅ Final Checklist for KodeKloud Verification

| Requirement | Status |
|--------------|---------|
| ConfigMap `my-redis-config` created | ✅ |
| Deployment name `redis-deployment` | ✅ |
| Image `redis:alpine` | ✅ |
| Container name `redis-container` | ✅ |
| Replica count = 1 | ✅ |
| CPU request = 1 | ✅ |
| EmptyDir volume at `/redis-master-data` | ✅ |
| ConfigMap volume at `/redis-master` | ✅ |
| Port 6379 exposed | ✅ |
| Pod running successfully | ✅ |

Once all are ✅, click “Check” on the KodeKloud task window —  
🎯 You’ll get a **100% successful validation!**

---

![Screenshot 3](./assets/WhatsApp%20Image%202025-10-08%20at%2010.15.31%20PM.jpeg)

---
