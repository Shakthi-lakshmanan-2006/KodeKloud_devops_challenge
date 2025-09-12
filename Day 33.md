# 🚀 Day 33 – Git Merge Conflict Resolution (KodeKloud Challenge)

## 📌 Task Details  
The Nautilus DevOps team is working on a project repository called **story-blog**. Two developers, **Max** and **Sarah**, are collaborating on the same file (`story-index.txt`).  

- Both pushed changes to the same file → this caused a **merge conflict**.  
- As **Max**, your task is to:  
  1. SSH into the storage server.  
  2. Navigate to Max’s repository (`story-blog`).  
  3. Fix the typo **"Mooose" → "Mouse"** in `story-index.txt`.  
  4. Ensure the file has **all 4 story titles**.  
  5. Resolve any merge conflicts properly.  
  6. Commit and push the fixed file to **Max’s repo** on Gitea.  
  7. Verify in the Gitea UI that changes are reflected.  

---

## 🧑‍🏫 What You Should Know Before Starting  

### 🔹 1. What is a Merge Conflict?  
- A **merge conflict** happens when two developers edit the **same part** of a file.  
- Git doesn’t know which version to keep, so it marks the conflict in the file with:  
  ```
  <<<<<<< HEAD
  your changes
  =======
  other changes
  >>>>>>> commit_id
  ```
- You must **manually edit** the file and keep the correct content.  

### 🔹 2. Basic Git Commands to Remember  
- `git status` → Check repo state.  
- `git add <file>` → Stage changes.  
- `git commit -m "message"` → Save changes locally.  
- `git pull origin master` → Get updates from remote repo.  
- `git push origin master` → Send changes to remote repo.  
- `nano <file>` → Open and edit a file in terminal.  

---

## 🛠️ Step-by-Step Solution  

### 🔑 Step 1: SSH into Storage Server  
```bash
ssh max@<storage_server_ip>
# password: Max_pass123
```

---

### 📂 Step 2: Go to the Repo  
```bash
cd /home/max/story-blog
ls
```
👉 You should see `story-index.txt`.

---

### 🔍 Step 3: Check Repo Status  
```bash
git status
```
👉 This shows if there are unstaged or conflicted files.

---

### 🖊️ Step 4: Edit the File  
```bash
nano story-index.txt
```

Final correct content should be:
```
1. The Lion and the Mouse
2. The Frogs and the Ox
3. The Fox and the Grapes
4. The Donkey and the Dog
```

👉 Remove any conflict markers like `<<<<<<<`, `=======`, `>>>>>>>`.  
Save (**CTRL+O, Enter**) and exit (**CTRL+X**).

---

### 📥 Step 5: Stage and Commit  
```bash
git add story-index.txt
git commit -m "Fixed typo and added all story titles"
```

---

### 🔄 Step 6: Pull Remote Changes  
```bash
git pull origin master
```

- If no conflict → continue.  
- If conflict appears again → repeat **Step 4** (clean file, save, commit).  

---

### 🚀 Step 7: Ensure Remote URL is Max’s Repo  
```bash
git remote -v
```

If it shows Sarah’s repo, fix it:
```bash
git remote set-url origin http://max@git.stratos.xfusioncorp.com/max/story-blog.git
```

---

### 📤 Step 8: Push Changes  
```bash
git push origin master
```

Enter:  
- Username → `max`  
- Password → `Max_pass123`  

---

### 🌐 Step 9: Verify in Gitea UI  
1. Log in at **Gitea** with:  
   - Username: `max`  
   - Password: `Max_pass123`  
2. Go to **Repositories → story-blog**.  
3. Open `story-index.txt` and confirm:  
   - 4 story titles present.  
   - Typo fixed.  
   - Commit visible.  

---
<img width="1280" height="666" alt="image" src="https://github.com/user-attachments/assets/0ac2db02-15bb-46f9-9e46-462f4b188399" />

---

## ✅ Summary of Commands Used  

```bash
# Connect to server
ssh max@<storage_server_ip>

# Navigate to repo
cd /home/max/story-blog
ls

# Check repo status
git status

# Edit file
nano story-index.txt

# Stage + commit
git add story-index.txt
git commit -m "Fixed typo and added all story titles"

# Pull remote changes
git pull origin master

# Fix remote if needed
git remote set-url origin http://max@git.stratos.xfusioncorp.com/max/story-blog.git

# Push changes
git push origin master
```

---

🎯 **In short**:  
You resolved a **merge conflict**, fixed the typo, made sure all stories are listed, and successfully pushed the changes to Max’s repo.  
