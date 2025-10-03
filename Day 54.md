# Day 54: Sharing Volumes Between Containers in a Pod

## Task Title

Sharing a volume among multiple containers within a pod to save temporary data.

## Basics You Should Know Before Doing This Task

1. **Pod in Kubernetes**

   * A pod is the smallest deployable unit in Kubernetes.
   * It can have one or more containers.

2. **Volumes in Pods**

   * Containers inside a pod are ephemeral (temporary).
   * To persist data within a pod across multiple containers, we use **Volumes**.

3. **emptyDir Volume**

   * Created when a pod starts and exists as long as the pod is running.
   * Data is shared among all containers in the pod that mount this volume.
   * Once the pod is deleted, data is gone.

4. **Sharing Volumes**

   * Multiple containers can mount the same volume at different paths.
   * Changes made in one container will be visible in the other.

## Steps and Commands with Beginner-Level Explanation

### Step 1: Create the Pod YAML File

Create a file named `volume-share-devops.yaml`:

```bash
nano volume-share-devops.yaml
```

Paste the following content:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: volume-share-devops
spec:
  containers:
    - name: volume-container-devops-1
      image: fedora:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/ecommerce

    - name: volume-container-devops-2
      image: fedora:latest
      command: ["sleep", "3600"]
      volumeMounts:
        - name: volume-share
          mountPath: /tmp/cluster

  volumes:
    - name: volume-share
      emptyDir: {}
```

* **Explanation:**

  * Two containers running `fedora:latest` image.
  * Both containers mount the same `emptyDir` volume at different paths.
  * `sleep 3600` keeps the containers running.

### Step 2: Apply the Pod

```bash
kubectl apply -f volume-share-devops.yaml
```

* **Explanation:** Creates the pod as defined in the YAML.

Check pod status:

```bash
kubectl get pods
```

### Step 3: Exec Into First Container and Create File

```bash
kubectl exec -it volume-share-devops -c volume-container-devops-1 -- /bin/bash
```

Inside the container:

```bash
cd /tmp/ecommerce
echo "Shared data between containers" > ecommerce.txt
ls -l
cat ecommerce.txt
```

Exit:

```bash
exit
```

### Step 4: Verify From Second Container

```bash
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- /bin/bash
```

Inside the container:

```bash
cd /tmp/cluster
ls -l
cat ecommerce.txt
```

* **Explanation:** You should see the same file created from container 1, confirming that the volume is shared.

---
![Screenshot 1](assets/Screenshot%202025-09-30%20185153.png)

---

## Main Challenge Faced

* **Challenge:** I initially forgot to specify the volume mounts in the second container, so the shared file was not visible.
* **Solution:** I carefully added the `volumeMounts` section in the second container, pointing to the shared volume `volume-share` at `/tmp/cluster`, which resolved the issue.

## Summary of Commands

```bash
# Create the YAML file
nano volume-share-devops.yaml

# Apply the pod
kubectl apply -f volume-share-devops.yaml

# Check pod status
kubectl get pods

# Exec into first container and create file
kubectl exec -it volume-share-devops -c volume-container-devops-1 -- /bin/bash
cd /tmp/ecommerce
echo "Shared data between containers" > ecommerce.txt

# Exec into second container and verify file
kubectl exec -it volume-share-devops -c volume-container-devops-2 -- /bin/bash
cd /tmp/cluster
ls -l
cat ecommerce.txt
```

---
![Screenshot 2](assets/Screenshot%202025-09-30%20185631.png)

---
