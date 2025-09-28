# Day 49: Deploy Applications with Kubernetes

## Basics to Know

Before working on this task, you should understand the following concepts:

1. **Kubernetes Deployment**: A Deployment manages a set of identical pods, ensuring the desired number of pods are running and updated.
2. **Pod**: The smallest deployable unit in Kubernetes, containing one or more containers.
3. **Container Image**: A lightweight, standalone, and executable software package that includes everything needed to run a piece of software, like `nginx:latest`.
4. **kubectl**: The command-line utility to interact with Kubernetes clusters.
5. **Labels and Selectors**: Labels are key-value pairs attached to objects, and selectors are used to identify a group of objects.

## Steps to Complete the Task

### Step 1: Create a Deployment

Create a deployment named `nginx` using the `nginx:latest` image.

```bash
kubectl create deployment nginx --image=nginx:latest
```

**Explanation:** This command creates a Deployment called `nginx` and sets it to use the latest version of the nginx image. Kubernetes automatically starts a pod with this container.

### Step 2: Verify Deployment

Check if the deployment was created successfully.

```bash
kubectl get deployments
```

**Explanation:** Lists all deployments in the cluster, showing how many pods are up-to-date and available.

### Step 3: Verify Pod

Check if the pod created by the deployment is running.

```bash
kubectl get pods
```

**Explanation:** Lists all pods in the cluster. You should see a pod with a name starting with `nginx-` and its status as `Running`.

### Step 4: Describe Deployment

Get detailed information about the deployment.

```bash
kubectl describe deployment nginx
```

**Explanation:** Shows all details about the deployment, including the image used, labels, number of replicas, and pod template.

---
![Task Instructions and Terminal Output](assets/Screenshot%202025-09-23%20233252.png)

---

## Summary of Commands Used

```bash
kubectl create deployment nginx --image=nginx:latest
kubectl get deployments
kubectl get pods
kubectl describe deployment nginx

```

---
![Kubernetes Deployment Created](assets/Screenshot%202025-09-23%20233848.png)

---
