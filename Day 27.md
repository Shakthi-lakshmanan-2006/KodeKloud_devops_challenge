# Day 27 Challenge - Git Revert Task

The Nautilus application development team was working on a git repository `/usr/src/kodekloudrepos/official`.  
They reported an issue with the recent commits being pushed to this repo.  
They have asked the DevOps team to revert repo HEAD to the last commit.  

**Task:**  
In `/usr/src/kodekloudrepos/official` git repository, revert the latest commit (HEAD) to the previous commit.  
The previous commit should be the initial commit.  
Use commit message:  
```
revert official
```

---

## 🔹 Steps to Complete the Task

### 1. Go to the repo directory
```bash
cd /usr/src/kodekloudrepos/official
```

### 2. Check the commit history
```bash
git log --oneline
```

Example output:
```
a1b2c3d (HEAD -> master) some recent change
e4f5g6h initial commit
```

Here:  
- `a1b2c3d` → latest commit (we want to revert this)  
- `e4f5g6h` → previous commit (initial commit)  

---

### 3. Revert the latest commit
```bash
git revert HEAD --no-edit
```

This creates a new commit that undoes the changes from the latest commit.  
We use `--no-edit` so Git doesn’t open the editor.  

---

### 4. Amend the commit message
The task requires the commit message to be exactly:
```
revert official
```

So, run:
```bash
git commit --amend -m "revert official"
```

---

### 5. Verify commit history
```bash
git log --oneline
```

Expected output:
```
1234567 revert official
a1b2c3d some recent change
e4f5g6h initial commit
```

---
<img width="1912" height="1040" alt="Screenshot 2025-09-05 194205" src="https://github.com/user-attachments/assets/e31c26c5-9b1b-41b5-b463-609239a83ef7" />

---

## 📌 Summary
- Worked on the repo: `/usr/src/kodekloudrepos/official`.  
- Checked commit history to identify the latest commit (HEAD) and the initial commit.  
- Reverted the latest commit using:
  ```bash
  git revert HEAD --no-edit
Amended the commit message to the required one:

  ```bash
  git commit --amend -m "revert official"
  ```
Verified the commit history to confirm the revert commit was created successfully.

---
<img width="1720" height="938" alt="Screenshot 2025-09-05 194308" src="https://github.com/user-attachments/assets/fa156b20-465f-48bc-9b1e-9332943d5127" />
