# Day 28 Git Challenge

The Nautilus application development team has been working on a project repository
**/opt/blog.git**. This repo is cloned at **/usr/src/kodekloudrepos** on the storage server in Stratos
DC. There are two branches in this repository, **master** and **feature**. One of the developers is
working on the feature branch and their work is still in progress, however they want to merge one of
the commits from the feature branch to the master branch. The message for the commit that needs
to be merged into master is **Update info.txt**. Accomplish this task for them, also remember to
push your changes eventually.
# Basics You Need Before the Challenge
### 1. Branches in Git
Git branches allow multiple people to work on the same project without interfering with each other.
**Example:**
- `master` (or `main`) → Stable branch (production-ready code).
- `feature` → Developers experiment or add new things here.
So in your repo:
- You have two branches: `master` and `feature`.
---
### 2. Merging in Git
There are 2 main ways to merge changes:
**Full merge** – You merge the entire branch into another.
```bash
git merge feature
```
This would bring all commits from feature into master.
**Cherry-pick** – You merge only one specific commit from one branch into another.
```bash
git cherry-pick
```
This is useful when only one commit (not the whole branch) is needed.
---
### 3. Commit Hash
Every commit in Git has a unique ID (a long string of numbers and letters).
You can view it using:
```
git log
```
**Example:**
```
commit 3f2a8b9d7 Update info.txt
```
Here, `3f2a8b9d7` is the commit hash we’ll need.
---
# Now, Let’s Solve the Challenge
**Task:** From `feature` branch, bring only the commit with message **Update info.txt** into
`master`.
### Step 1: Go to the repository
```bash
cd /usr/src/kodekloudrepos/blog
```
### Step 2: Make sure you’re on master branch
```bash
git checkout master
```
We need to be on master because we want to merge into master.
### Step 3: Find the commit in feature
```bash
git log feature
```
This shows all commits in the `feature` branch.
Look for the commit with the message:
```bash
Update info.txt
```
Copy its commit hash (like `3f2a8b9d7`).
### Step 4: Cherry-pick the commit
```bash
git cherry-pick
```
This will bring only that specific commit into `master`.
For example:
```bash
git cherry-pick 3f2a8b9d7
```
### Step 5: Push changes
```bash
git push origin master
```
### This uploads your updated `master` branch (with the cherry-picked commit) back to the remote repository.

<img width="1918" height="1042" alt="Screenshot 2025-09-05 214045" src="https://github.com/user-attachments/assets/c35e5b1a-59c1-4f5f-9b4e-d5deca9d98b2" />

---
## Why We Use Cherry-pick Here?
### If we used `git merge feature`, it would bring all commits from `feature`.
### The requirement is to bring only one commit (**Update info.txt**).
### That’s why **git cherry-pick** is the right tool here.
---
## Final Command Summary
```
cd /usr/src/kodekloudrepos/blog
git checkout master
git log feature # find commit hash for "Update info.txt"
git cherry-pick
git push origin master
```
<img width="1635" height="939" alt="Screenshot 2025-09-05 214241" src="https://github.com/user-attachments/assets/f2885098-94f1-4001-89eb-40bda9b44363" />

