# 🚀 Day 30 Challenge – Reset Commit History in Git

## 🧑‍🏫 Step 1: Basics You Need to Know

### 1. What is a Git Commit?
- A commit is a snapshot of your code at a particular time.  
- Think of it like saving your game progress — every commit is a save point.

### 2. What is Git Commit History?
- Git keeps a chain of commits. You can see it using:

```bash
git log --oneline
```

**Example output:**
```
a1b2c3 (HEAD -> master) testing changes
d4e5f6 add data.txt file
123abc initial commit
```

- Here **a1b2c3** is the latest commit (HEAD points here).  
- **d4e5f6** is an older commit.  
- **123abc** is the very first commit.

### 3. What Does "Resetting Commit History" Mean?
- Resetting means **rewinding history** so Git forgets some commits.  
- We want to keep only 2 commits:
  1. The initial commit  
  2. The `add data.txt file` commit  

👉 Everything after that should be erased.

### 4. Git Reset vs. Git Revert
- `git reset` → Moves HEAD/branch pointer to an older commit. Changes after that are deleted (history rewrite).  
- `git revert` → Creates a new commit that undoes changes but keeps history.  

👉 Since the question says *“clean this repository along with the commit history/work tree”*, we must use **reset**.

### 5. Forcing the Remote Repository
After we rewrite history locally, the remote repo still has the old commits.  
So we need to force push:

```bash
git push origin master --force
```

⚠️ **Note:** Force push overwrites remote history — use carefully.


---

## 📝 Step 2: The Task Breakdown
- ✅ Repository: `/usr/src/kodekloudrepos/apps`  
- ✅ Goal: Keep only 2 commits (**initial commit** + **add data.txt file**)  
- ✅ Push changes so the remote repo also has only 2 commits  

---

## 🚀 Step 3: Step-by-Step Solution (with Explanations)

### 1. Go to the repo directory
```bash
cd /usr/src/kodekloudrepos/apps
```

### 2. Check commit history
```bash
git log --oneline
```

**Example output:**
```
a1b2c3 some testing changes
d4e5f6 add data.txt file
123abc initial commit
```

👉 We want to keep only **123abc** and **d4e5f6**.  

### 3. Reset branch to the commit `add data.txt file`
```bash
git reset --hard d4e5f6
```

- `--hard` → makes your working directory match that commit.  
- Now HEAD points at `d4e5f6`.  

⚡ **Why?** Because we want history to stop here (initial commit → add data.txt).  

### 4. Confirm the history
```bash
git log --oneline
```

**Output should be:**
```
d4e5f6 add data.txt file
123abc initial commit
```

✅ Now history has only **2 commits**.  

### 5. Push changes to remote (overwrite old history)
```bash
git push origin master --force
```

⚡ **Why `--force`?** Because remote has extra commits we deleted locally. Force push tells Git: *overwrite remote with my local history*.  

---
![WhatsApp Image 2025-09-07 at 10 57 56 PM](https://github.com/user-attachments/assets/cdbdc170-a240-45b4-83d7-e793ac0169fc)


## 🎯 Final Summary

**Commands to run:**
```bash
cd /usr/src/kodekloudrepos/apps
git log --oneline
git reset --hard <commit-id-of-add-data.txt>
git log --oneline
git push origin master --force
```
