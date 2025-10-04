# Day 55 — Sidecar Pattern with Shared Volume

## 🔑 Basics to Know Before Doing This Task

1. **Pod**  
   - Smallest deployable unit in Kubernetes.  
   - Can contain one or more containers.  

2. **Sidecar Pattern**  
   - Extra container in the same Pod that does a helper task.  
   - Example: Nginx serves pages, Sidecar ships logs.  

3. **emptyDir Volume**  
   - Temporary storage created when a Pod starts and deleted when the Pod stops.  
   - Used to share data between containers in the same Pod.  

4. **Volume Mounts**  
   - To use a volume, you mount it inside each container.  
   - Example: Both containers mount `/var/log/nginx`.  

5. **Container Commands**  
   - Sidecar runs an infinite loop to print logs every 30 seconds:  

     ```sh
     sh -c "while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done"
     ```

---

## 📝 Steps to Solve the Task

1. Create a Pod named **webserver**.  
2. Add a shared volume named **shared-logs** of type `emptyDir`.  
3. Add two containers:  
   - `nginx-container` with image `nginx:latest`.  
   - `sidecar-container` with image `ubuntu:latest`.  
4. Mount the volume in both containers at `/var/log/nginx`.  
5. Apply the YAML file.  
6. Check logs of the sidecar container.  

---

## 🖥️ YAML File (`webserver-pod.yaml`)

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: webserver
spec:
  volumes:
    - name: shared-logs
      emptyDir: {}
  containers:
    - name: nginx-container
      image: nginx:latest
      volumeMounts:
        - name: shared-logs
          mountPath: /var/log/nginx
    - name: sidecar-container
      image: ubuntu:latest
      command: ["sh", "-c", "while true; do cat /var/log/nginx/access.log /var/log/nginx/error.log; sleep 30; done"]
      volumeMounts:
        - name: shared-logs
          mountPath: /var/log/nginx
```

---

## ✅ Commands to Run with Explanations

### 1. Create the YAML file  

```bash
vi webserver-pod.yaml
```

- `vi` is a text editor (like Notepad, but inside the terminal).  
- Here we are creating a new file called `webserver-pod.yaml`.  
- Paste the YAML into this file.  
- Save and exit by typing `:wq` and pressing **Enter**.  

👉 This step is like writing instructions for Kubernetes on paper.  

---

### 2. Apply the YAML file to Kubernetes  

```bash
kubectl apply -f webserver-pod.yaml
```

- `kubectl` = tool to communicate with Kubernetes.  
- `apply` = create/update resources from a YAML file.  
- `-f` = tells Kubernetes which file to use.  

👉 Think of this as giving your instructions to Kubernetes and saying: **“Please make this happen.”**  

---

### 3. Check if the Pod is running  

```bash
kubectl get pods
```

- Shows all running Pods.  
- You should see:  

  ```
  NAME        READY   STATUS    RESTARTS   AGE
  webserver   2/2     Running   0          1m
  ```  

- `2/2` = 2 containers inside the Pod are running.  
- `STATUS` = Running → everything is good.  

👉 This is like asking Kubernetes: **“Hey, is my Pod alive and working?”**  

---

### 4. View logs from the Sidecar container  

```bash
kubectl logs -f webserver -c sidecar-container
```

- `logs` = see container output.  
- `-f` = follow mode (live updates).  
- `webserver` = Pod name.  
- `-c sidecar-container` = specify container inside the Pod.  

👉 Here you’ll see Nginx logs printed every 30 seconds by the sidecar.  

---

### 5. (Optional) View logs directly from the Nginx container  

```bash
kubectl logs -f webserver -c nginx-container
```

- Same as above, but now we ask Nginx itself for logs.  
- These are the **raw logs** created by Nginx.  

👉 This is like going directly to the chef (nginx) and asking: **“What are you cooking and who is eating it?”**  

---
![Screenshot 1](assets/Screenshot%202025-09-30%20190539.png)

---

## 🟢 Analogy

- Pod = Kitchen  
- Nginx = Chef cooking food (serving web pages)  
- Sidecar = Helper reading out the leftovers (logs) every 30 seconds  
- emptyDir volume = Shared fridge where both put and read stuff  

---

## 🎯 Summary

- Pod `webserver` has **2 containers**: Nginx + Sidecar.  
- Logs are shared via **emptyDir volume**.  
- Sidecar prints logs every 30 seconds.  
- Both containers work independently but share the same volume.  

---
![Screenshot 2](assets/Screenshot%202025-09-30%20191206.png)

---
