# Day 80 : Jenkins Upstream–Downstream Deployment Job (Nautilus App)

## 1. Intro

In this task you are acting as a DevOps engineer for the Nautilus project.  
The goal is to:

1. Use **Jenkins** to automatically deploy the latest version of the `web` application from a Git repository to the shared web root (`/var/www/html` on the Storage server).
2. Create a **downstream Jenkins job** that restarts the **Apache (`httpd`) service** on all application servers **only when the deployment job is successful**.

This gives you a small but complete CI/CD flow:

- **Upstream job** → deploy code to shared storage  
- **Downstream job** → restart web service on all app servers

---

## 2. Things to Know Before Doing the Task

### 2.1 Lab Credentials & Endpoints

- **Jenkins UI**
  - Access: Top bar → **Jenkins** button
  - URL: (given in lab, usually `http://jenkins:8080/`)
  - Username: `admin`
  - Password: `Adm!n321`

- **Gitea UI (Git Server)**
  - Access: Top bar → **Gitea** button or `http://<host>:8080`
  - Username: `sarah`
  - Password: `Sarah_pass123`
  - Repository: `web` (under user **sarah**)

### 2.2 Application & Servers

- **Apache (httpd)** is already installed and configured on **all app servers**.
- The **document root** for all app servers is:

  ```bash
  /var/www/html
  ```

- This directory is **shared from the Storage server**, typically via NFS.  
  That means:
  - Jenkins only needs to update `/var/www/html` on the **Storage server**.
  - Changes automatically show up on **all application servers**.

### 2.3 Git Layout

- On the **Storage server**, `/var/www/html` is **already a local Git repository**.
- It tracks the remote repo: `web` on Gitea.
- We just need Jenkins to **pull the latest changes** from the `master` branch into this directory.

### 2.4 Jenkins Concepts Used

- **Freestyle jobs** – classic Jenkins jobs where you configure SCM and build steps.
- **Upstream / Downstream jobs**
  - *Upstream*: the job that triggers another job when it finishes.
  - *Downstream*: the job that gets triggered by the upstream job.
- **Post-build actions** – used to trigger other jobs, and to control on which result (success/unstable/failure) they should run.

---
![Screenshot](./assets/Screenshot%202025-11-30%20194803%20-%20Copy%20(2).png)

---

## 3. Step-by-Step: Completing the Task

### Step 1 – Log in to Jenkins

1. From the top bar in the lab UI, click **Jenkins**.
2. Log in with:
   - User: `admin`
   - Password: `Adm!n321`

---

### Step 2 – Create the Upstream Job: `nautilus-app-deployment`

1. In Jenkins, click **New Item**.
2. Enter **Item name**: `nautilus-app-deployment`.
3. Choose **Freestyle project**, click **OK**.

#### 2.1 Configure Git (SCM)

1. In the job configuration page, go to **Source Code Management** → select **Git**.
2. Set **Repository URL** to the Gitea repo of `web`. Example (pattern):

   ```text
   http://<gitea-host>:8080/sarah/web.git
   ```

   (Use the exact URL shown in Gitea in your lab.)

3. Under **Credentials**, add Jenkins credentials for `sarah` if not already present:
   - Username: `sarah`
   - Password: `Sarah_pass123`
4. Choose **Branch Specifier** as:

   ```text
   */master
   ```

> Note: Even though `/var/www/html` is already a Git repo on the Storage server, using the **remote Gitea URL** in Jenkins is fine; Jenkins will clone and then we can deploy to `/var/www/html` in the build step.

#### 2.2 Configure the Build Step (Deploy to `/var/www/html`)

1. Scroll down to **Build**.
2. Click **Add build step** → **Execute shell**.
3. Add a script similar to:

   ```bash
   # Workspace where Jenkins checked out the 'web' repo
   APP_SRC_DIR="$WORKSPACE"

   # Target directory on Storage server (mounted locally or reachable via SSH)
   TARGET_DIR="/var/www/html"

   echo "Deploying Nautilus web app..."
   sudo rm -rf "${TARGET_DIR:?}/"*        # Clean existing content (careful with paths)
   sudo cp -r "${APP_SRC_DIR}/"* "${TARGET_DIR}/"

   echo "Deployment completed. Current files in target directory:"
   ls -l "${TARGET_DIR}"
   ```

> If Jenkins is **not** running directly on the Storage server, you would instead use `scp` or `rsync` over SSH to copy from the Jenkins host to the Storage server. The lab usually has Jenkins and Storage connected in a simple way so the above local copy works.

4. Click **Save**.

---

### Step 3 – Create the Downstream Job: `manage-services`

1. From Jenkins main page, click **New Item**.
2. Name: `manage-services`.
3. Type: **Freestyle project** → **OK**.

#### 3.1 Configure Build Trigger (Downstream Relationship)

We want `manage-services` to run **after** and only when `nautilus-app-deployment` is successful.  
There are two ways; we will configure it as a true **downstream** job from the upstream.

1. **Do NOT** set any “Build Triggers” here yet.
2. Scroll down to **Build** – we’ll add the restart logic first (next subsection).
3. We will connect it from the upstream job in **Step 4**.

#### 3.2 Configure the Build Step (Restart `httpd` on All App Servers)

1. In `manage-services` job configuration, go to **Build**.
2. Click **Add build step** → **Execute shell**.
3. Add a script similar to (example with three app servers):

   ```bash
   # List of app servers – replace with actual hostnames from the lab if different
   APP_SERVERS=("app01" "app02" "app03")
   SSH_USER="root"   # or appropriate user if sudo is needed

   echo "Restarting httpd on all app servers..."

   for host in "${APP_SERVERS[@]}"; do
     echo "Restarting httpd on ${host}..."
     ssh ${SSH_USER}@${host} "systemctl restart httpd && systemctl status httpd --no-pager | head -n 5"
   done

   echo "All restarts attempted."
   ```

> In some KodeKloud labs you might instead run an **Ansible playbook** from this job.  
> In that case, the build step would look like:

```bash
cd /home/thor/ansible
ansible-playbook restart-httpd.yml
```

(Use the exact paths and inventory from your lab if they are provided.)

4. Click **Save**.

---

### Step 4 – Make `manage-services` a Downstream Job of `nautilus-app-deployment`

1. Open the `nautilus-app-deployment` job configuration (**Configure**).
2. Scroll to **Post-build Actions**.
3. Click **Add post-build action** → **Build other projects**.
4. In **Projects to build**, type: `manage-services`.
5. In the drop-down **Trigger only if build is stable**, select **“Trigger only if build is stable”** (or equivalent wording in your Jenkins version).
6. Click **Save**.

Now the flow is:

- Run `nautilus-app-deployment`.
- If it completes **successfully**, Jenkins automatically triggers `manage-services`.
- `manage-services` restarts `httpd` on all app servers.

---

### Step 5 – Test the Pipeline

1. Make a change in the `web` repo via **Gitea**:
   - Log in as `sarah` → open `web` repo.
   - Edit an HTML file (for example `index.html`) and commit the change to `master`.
2. In Jenkins, open `nautilus-app-deployment` and click **Build Now**.
3. Verify:
   - The build for `nautilus-app-deployment` is **SUCCESS**.
   - A new build for `manage-services` is automatically started and is **SUCCESS**.
4. From the top bar, click **App** to open the application through the **Load Balancer (LB)**.
5. Check that:
   - The required content appears on the main URL:

     ```text
     https://<LBR-URL>/
     ```

   - It should **not** be served from a subdirectory like `/web` (i.e. *not* `https://<LBR-URL>/web`).

If everything looks good, the task is complete.

---

## 4. Summary of Key Commands

Below is a quick list of shell commands you are likely to use or see in this task:

```bash
# 1. Git operations (on Gitea side or local dev, conceptually)
git status
git add <files>
git commit -m "Update web content"
git push origin master

# 2. Deployment (inside 'nautilus-app-deployment' Jenkins job)
APP_SRC_DIR="$WORKSPACE"
TARGET_DIR="/var/www/html"
sudo rm -rf "${TARGET_DIR:?}/"*
sudo cp -r "${APP_SRC_DIR}/"* "${TARGET_DIR}/"
ls -l "${TARGET_DIR}"

# 3. Restart httpd on app servers (inside 'manage-services' Jenkins job)
APP_SERVERS=("app01" "app02" "app03")
for host in "${APP_SERVERS[@]}"; do
  ssh root@${host} "systemctl restart httpd && systemctl status httpd --no-pager | head -n 5"
done

# 4. Basic Apache service checks (manual troubleshooting)
systemctl status httpd
journalctl -u httpd --no-pager | tail
```

---
![Screenshot](./assets/Screenshot%202025-11-30%20202306.png)

---
![Screenshot](./assets/Screenshot%202025-11-30%20202347.png)

---

### Final Idea

By the end of **Day 80**, you have:

- A **deployment job** (`nautilus-app-deployment`) that:
  - Pulls code from the `web` Git repository
  - Deploys it into `/var/www/html` on the Storage server

- A **service management job** (`manage-services`) that:
  - Restarts `httpd` on all app servers
  - Runs automatically as a **downstream job** **only when** deployment is successful

This pattern is very common in real DevOps pipelines: separate **deploy** and **service-management** responsibilities, but tie them together with Jenkins job chaining.

---
![Screenshot](./assets/Screenshot%202025-11-30%20202721.png)

---
