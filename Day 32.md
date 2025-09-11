# Day 32 - Git Rebase Task

## 📌 Task Description
The Nautilus application development team has been working on a project repository `/opt/blog.git`.  
This repo is cloned at `/usr/src/kodekloudrepos` on the storage server in **Stratos DC**.  

One of the developers is working on the `feature` branch and their work is still in progress.  
However, some changes have been pushed into the `master` branch.  
The developer now wants to **rebase** the `feature` branch with the `master` branch without losing any data from the `feature` branch.  

Additionally, they don’t want to add any `merge commit` by simply merging the `master` branch into the `feature` branch.  

👉 Accomplish this task as per requirements, and push the changes once done.

---

## 📝 Things to Know Before Doing the Task
- **Git Branches**: Different versions of your codebase (e.g., `master`, `feature`).  
- **Rebase**: Moves your branch commits on top of another branch, giving a clean history.  
- **Merge vs Rebase**:  
  - Merge adds a merge commit.  
  - Rebase rewrites commit history without an extra commit.  
- **Force Push**: Required after rebase, because commit history changes.  

---

## 💻 Commands and Explanation

```bash
# Step 1: Go to the repo location
cd /usr/src/kodekloudrepos/blog

# Step 2: Check branches
git branch

# Step 3: Switch to feature branch
git checkout feature

# Step 4: Rebase feature branch onto master
git rebase master

# Step 5: Verify the commit history
git log --oneline --graph --all

# Step 6: Push changes to remote (force push required after rebase)
git push origin feature --force
```

### 🔎 Explanation of Commands
- `git branch` → Shows all branches in the repository.  
- `git checkout feature` → Switches to the `feature` branch.  
- `git rebase master` → Applies `feature` commits on top of `master` commits.  
- `git log --oneline --graph --all` → Shows commit history in a compact, graphical format.  
- `git push origin feature --force` → Updates the remote `feature` branch with the rebased history.  

---

## ✅ Conclusion
By rebasing, we applied the `feature` branch commits on top of `master` without creating unnecessary merge commits.  
Finally, we force-pushed the updated branch to the remote repository.

---
![399e8ad0-0f40-499d-868a-0832ea33c038](https://github.com/user-attachments/assets/bd77a0e5-1a86-402f-90c9-2008cda6d8e8)

---

## 📌 Final Summary of Commands
```bash
cd /usr/src/kodekloudrepos/blog
git branch
git checkout feature
git rebase master
git log --oneline --graph --all
git push origin feature --force
```
---
![e5d9306f-81ee-4c56-a703-7b0c77560740](https://github.com/user-attachments/assets/38fd0653-bb14-4cc3-88e4-dff235429f51)
