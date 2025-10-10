# Day 61 KodeKloud DevOps Challenge - Using Init Containers

## **Title:** Using Init Containers in Kubernetes Deployment

## **Introduction:**

Some applications require certain configurations or initialization steps before the main container starts. These steps may not be possible to include inside the application image. Kubernetes provides **init containers** to perform such tasks before the main container starts.

This challenge demonstrates how to use an init container to create a file, which the main container reads continuously.

---

## **Basics You Should Know:**

1. **Kubernetes Deployment:**

   * Ensures the desired number of pods are running.
   * Use `kubectl get deployments` and `kubectl get pods` to verify.

2. **Init Containers:**

   * Run **before** main containers.
   * Useful for initialization, configuration, or waiting for conditions.

3. **Volumes:**

   * `emptyDir` is a temporary storage shared between containers in the same pod.
   * Exists only while the pod is running.

4. **Volume Mounts:**

   * Mount volumes to container paths using `volumeMounts`.
   * Must match the volume name and mount path correctly.

5. **Pod Labels:**

   * Labels help identify pods and services.
   * `metadata.labels` in template must match deployment selector.

6. **Container Commands:**

   * `/bin/bash -c "command"` executes commands inside the container.
   * `echo "..." > file` writes text to a file.
   * `while true; do cat file; sleep 5; done` continuously prints file content every 5 seconds.

---

## **Steps and Commands:**

### **1️⃣ Create Deployment YAML**

Create a file named `ic-deploy-nautilus.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ic-deploy-nautilus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: ic-nautilus
  template:
    metadata:
      labels:
        app: ic-nautilus
    spec:
      volumes:
        - name: ic-volume-nautilus
          emptyDir: {}
      initContainers:
        - name: ic-msg-nautilus
          image: ubuntu:latest
          command: ["/bin/bash", "-c", "echo 'Init Done - Welcome to xFusionCorp Industries' > /ic/ecommerce"]
          volumeMounts:
            - name: ic-volume-nautilus
              mountPath: /ic
      containers:
        - name: ic-main-nautilus
          image: ubuntu:latest
          command: ["/bin/bash", "-c", "while true; do cat /ic/ecommerce; sleep 5; done"]
          volumeMounts:
            - name: ic-volume-nautilus
              mountPath: /ic
```

**Explanation:**

* `volumes:` defines `emptyDir` for temporary storage.
* `initContainers:` writes a message to `/ic/ecommerce` before main container starts.
* `containers:` main container prints the message continuously.
* `volumeMounts:` ensures both containers can access the same path.

---

### **2️⃣ Apply Deployment**

```bash
kubectl apply -f ic-deploy-nautilus.yaml
```

Check deployment and pods:

```bash
kubectl get deployments
kubectl get pods
```

### **3️⃣ Verify Init Container**

```bash
kubectl describe pod <pod-name>
```

* Check under **Init Containers**: should show `ic-msg-nautilus` completed successfully.

### **4️⃣ Verify Main Container**

```bash
kubectl logs <pod-name>
```

* You should see the message printed every 5 seconds:

```
Init Done - Welcome to xFusionCorp Industries
Init Done - Welcome to xFusionCorp Industries
...
```

### **5️⃣ Cleanup (Optional)**

```bash
kubectl delete deployment ic-deploy-nautilus
```

---
![Screenshot 1](assets/Screenshot%202025-10-03%20192443.png)

---
![Screenshot 2](assets/Screenshot%202025-10-03%20192344.png)

---

## **Summary of Commands:**

| Step             | Command                                        | Explanation                                          |
| ---------------- | ---------------------------------------------- | ---------------------------------------------------- |
| Create YAML      | `vi ic-deploy-nautilus.yaml`                   | Write deployment config with init and main container |
| Apply Deployment | `kubectl apply -f ic-deploy-nautilus.yaml`     | Deploy the pod                                       |
| Check Deployment | `kubectl get deployments`                      | Verify deployment exists                             |
| Check Pods       | `kubectl get pods`                             | See running pods                                     |
| Describe Pod     | `kubectl describe pod <pod-name>`              | Check init container status                          |
| Check Logs       | `kubectl logs <pod-name>`                      | Verify main container prints message                 |
| Cleanup          | `kubectl delete deployment ic-deploy-nautilus` | Remove deployment                                    |

---

## **Tips for Beginners:**

* Ensure volume names and mount paths match between init and main containers.
* Init containers always finish before main containers start.
* `emptyDir` is ephemeral; data disappears when pod is deleted.
* Use `kubectl logs -f <pod-name>` to watch live output.

---
![Screenshot 3](assets/Screenshot%202025-10-03%20192404.png)

---
