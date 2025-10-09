# Day 60: Kubernetes Persistent Volumes and NodePort Service Challenge

## Introduction

In this challenge, the Nautilus DevOps team is tasked with deploying a web application on a Kubernetes cluster using **persistent storage**. The goal is to create a **PersistentVolume (PV)**, a **PersistentVolumeClaim (PVC)**, a **Pod running Nginx** with the PVC mounted as the document root, and finally expose the web server via a **NodePort service**.

This challenge tests your understanding of Kubernetes storage, pod configuration, service exposure, and practical troubleshooting.

---

## Basics You Should Know Before Doing This Task

1. **PersistentVolume (PV)**

   * A PV is a cluster-wide storage resource representing actual storage (like hostPath, NFS, or cloud disks).
   * PVs are created by the cluster admin and define storage capacity, access modes, and storage class.

2. **PersistentVolumeClaim (PVC)**

   * PVC is a user's request for storage.
   * It is bound to a matching PV automatically.
   * Access modes (ReadWriteOnce, ReadOnlyMany, ReadWriteMany) define how pods can access storage.

3. **Access Modes**

   * `ReadWriteOnce (RWO)`: Single node can mount in read-write mode.
   * `ReadOnlyMany (ROX)`: Multiple nodes can mount in read-only mode.
   * `ReadWriteMany (RWX)`: Multiple nodes can mount in read-write mode.

4. **hostPath Volume**

   * Uses a directory from the node’s filesystem for storage.
   * Useful for testing or single-node clusters.

5. **Pod with PVC**

   * Mounting a PVC inside a pod allows the container to read/write persistent data.
   * Nginx default document root: `/usr/share/nginx/html`.

6. **Service (NodePort)**

   * Exposes pod externally via a static port (30000–32767).
   * NodePort maps a port on all nodes to the pod’s port.

---

## Step-by-Step Instructions with Beginner-Level Explanation

### Step 1: Create the PersistentVolume (PV)

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv-nautilus
spec:
  storageClassName: manual
  capacity:
    storage: 3Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: /mnt/security
```

Apply PV:

```bash
kubectl apply -f pv.yaml
```

---

### Step 2: Create the PersistentVolumeClaim (PVC)

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-nautilus
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
```

Apply PVC:

```bash
kubectl apply -f pvc.yaml
kubectl get pvc
```

Check that STATUS shows `Bound`.

---

### Step 3: Create the Pod with PVC Mounted

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: pod-nautilus
  labels:
    app: nginx
spec:
  containers:
    - name: container-nautilus
      image: nginx:latest
      ports:
        - containerPort: 80
      volumeMounts:
        - mountPath: /usr/share/nginx/html
          name: nautilus-storage
  volumes:
    - name: nautilus-storage
      persistentVolumeClaim:
        claimName: pvc-nautilus
```

Apply Pod:

```bash
kubectl apply -f pod.yaml
kubectl get pods --show-labels
```

Add a test file to verify:

```bash
kubectl exec -it pod-nautilus -- /bin/sh
echo "Hello Nautilus" > /usr/share/nginx/html/index.html
chmod 644 /usr/share/nginx/html/index.html
exit
```

---

### Step 4: Create NodePort Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: web-nautilus
spec:
  type: NodePort
  selector:
    app: nginx
  ports:
    - port: 80
      targetPort: 80
      nodePort: 30009   # choose a free port
```

Apply Service:

```bash
kubectl apply -f service.yaml
kubectl get svc web-nautilus
```

---

### Step 5: Access Nginx Web Server

If NodePort is not reachable externally, use port-forward:

```bash
kubectl port-forward pod/pod-nautilus 8080:80
curl http://localhost:8080
```

You should see:

```
Hello Nautilus
```

---
![Screenshot 2](assets/Screenshot%202025-10-03%20171255.png)

---

## Challenges Faced and Solutions

1. **Service not found / Pod without labels**

   * Initial Pod had no labels → Service could not select it.
   * **Solution:** Add `labels: app: nginx` to Pod metadata.

2. **NodePort already allocated**

   * Port 30008 was taken → Kubernetes rejected Service.
   * **Solution:** Chose a free NodePort (e.g., 30009).

3. **403 Forbidden Error**

   * Nginx could not read `/mnt/security` → default permissions blocked access.
   * **Solution:** Added a test `index.html` and set proper permissions (644 for file, 755 for directory).

4. **NodePort not reachable externally in lab**

   * Used `kubectl port-forward` to test Nginx inside the cluster.

---

## Summary of Commands

```bash
kubectl apply -f pv.yaml
kubectl apply -f pvc.yaml
kubectl get pvc
kubectl apply -f pod.yaml
kubectl get pods --show-labels
kubectl exec -it pod-nautilus -- /bin/sh
# inside pod
echo "Hello Nautilus" > /usr/share/nginx/html/index.html
chmod 644 /usr/share/nginx/html/index.html
exit
kubectl apply -f service.yaml
kubectl get svc web-nautilus
kubectl port-forward pod/pod-nautilus 8080:80
curl http://lo
```

---
![Screenshot 1](assets/Screenshot%202025-10-03%20171341.png)

---
