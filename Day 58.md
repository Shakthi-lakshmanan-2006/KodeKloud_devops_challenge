# Day 58: Deploy Grafana on Kubernetes

## **Introduction**

The Nautilus DevOps team is planning to set up **Grafana** to collect and analyze analytics from some applications. Grafana is a powerful visualization and monitoring tool used to track metrics and performance. In this challenge, we are tasked with deploying Grafana on a **Kubernetes cluster** and exposing it so that it can be accessed externally.

---

## **Task**

1. Create a **Deployment** named `grafana-deployment-nautilus` using any Grafana image.
2. Create a **NodePort Service** with NodePort `32000` to expose the Grafana app.
3. Access the Grafana login page using the NodePort.

> **Note:** No configuration changes inside Grafana are required for this challenge.

---

## **Basics You Should Know**

Before starting, here are some key concepts:

* **Deployment:** A Kubernetes resource to run and manage **pods**. It ensures the specified number of pods are always running.
* **Pod:** The smallest deployable unit in Kubernetes, typically running a single container.
* **Service:** A way to expose pods to other services or outside the cluster.

  * **NodePort Service:** Exposes the pod on a specific port of every node in the cluster, allowing external access.
* **Grafana default port:** 3000 (used inside the container).

---

## **Step-by-Step Commands and Explanation**

### **Step 1: Create Deployment YAML**

Create a file `grafana.yaml`:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: grafana-deployment-nautilus
spec:
  replicas: 1
  selector:
    matchLabels:
      app: grafana
  template:
    metadata:
      labels:
        app: grafana
    spec:
      containers:
      - name: grafana
        image: grafana/grafana:latest
        ports:
        - containerPort: 3000
---
apiVersion: v1
kind: Service
metadata:
  name: grafana-service
spec:
  type: NodePort
  selector:
    app: grafana
  ports:
    - protocol: TCP
      port: 3000
      targetPort: 3000
      nodePort: 32000
```

**Explanation:**

* **Deployment:** Runs 1 replica of Grafana container.
* **Service:** Exposes Grafana on NodePort `32000` to access externally.

---

### **Step 2: Apply YAML**

```bash
kubectl apply -f grafana.yaml
```

**Note:** If you get the error about `spec.selector: field is immutable`, it means a deployment with the same name already exists.

---

### **Step 3: Resolve Deployment Errors (If Any)**

If the selector error occurs:

```bash
kubectl delete deployment grafana-deployment-nautilus
kubectl delete service grafana-service
kubectl apply -f grafana.yaml
```

**Explanation:** Deletes the old deployment and service, then reapplies the YAML cleanly.

---

### **Step 4: Verify Deployment and Service**

```bash
kubectl get pods
kubectl get svc
```

* Check that the pod is `Running`.
* Check that the service has NodePort `32000`.

---

### **Step 5: Access Grafana**

Open your browser and go to:

```
http://<NODE_IP>:32000
```

**Login credentials (default):**

* Username: `admin`
* Password: `admin`

You should now see the Grafana login page.

---
![Screenshot 1](assets/Screenshot%202025-10-01%20231414.png)

---
![Screenshot 2](assets/Screenshot%202025-10-01%20231435.png)

---

## **Difficulties Faced and Solutions**

1. **Error: `--node-port` unknown flag**

   * **Cause:** `kubectl expose` does not support `--node-port`.
   * **Solution:** Use YAML file or `kubectl create service nodeport` to specify NodePort.

2. **Error: `spec.selector: field is immutable`**

   * **Cause:** A deployment with the same name already exists with a different selector.
   * **Solution:** Delete the existing deployment and service, then reapply YAML.

3. **Warnings about `last-applied-configuration`**

   * **Cause:** Resources were not created declaratively initially.
   * **Solution:** Ignored; patched automatically by Kubernetes.

---

## **Summary of Commands**

```bash
# Step 1: Create YAML
vi grafana.yaml  # Add deployment + service content

# Step 2: Apply deployment and service
kubectl apply -f grafana.yaml

# Step 3: (If error occurs) Delete and reapply
kubectl delete deployment grafana-deployment-nautilus
kubectl delete service grafana-service
kubectl apply -f grafana.yaml

# Step 4: Verify pods and service
kubectl get pods
kubectl get svc

# Step 5: Access Grafana in browser
# URL: http://<NODE_IP>:32000
# Login: admin / admin
```

---
![Screenshot 3](assets/Screenshot%202025-10-01%20231525.png)

---
