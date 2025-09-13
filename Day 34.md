# Git Hooks Challenge – Post-Update Hook with Release Tags

## 📌 Task Details  
The Nautilus application development team has a bare Git repository located at `/opt/news.git`, which is cloned under `/usr/src/kodekloudrepos/news`.  
Your task is to:  
1. Create a **post-update hook** in this Git repository.  
2. Ensure that whenever any changes are pushed to the **master branch**, the hook automatically creates a release tag named:  
   ```
   release-YYYY-MM-DD
   ```  
   where `YYYY-MM-DD` is the current date.  
3. Test the hook at least once and verify that the release tag is created successfully.  
4. Push all changes to the remote repository.  

---

## 🧑‍💻 Things a Beginner Should Know  

Before starting, make sure you understand these concepts:  

- **Bare Repository (`.git`)**: A repository without a working directory, used for collaboration (e.g., `/opt/news.git`).  
- **Git Hooks**: Scripts that Git executes at specific events like commit, push, or merge. Stored inside the `.git/hooks` folder.  
- **post-update Hook**: Runs after a successful `git push`. Perfect for automating tasks like creating release tags.  
- **Tags in Git**: Tags are like fixed points (snapshots) in history, usually used for marking releases.  
- **Branch Checkout**: Switching between branches (`git checkout master`).  
- **Merge**: Combining changes from one branch into another.  
- **Push**: Sending local commits to the remote repository.  

---

## 🚀 Step-by-Step Solution  

### ✅ Step 1: Navigate to the hooks directory  
```bash
cd /opt/news.git/hooks
```
- This moves into the hooks folder of the repository.  
- Every Git repository has a `.git/hooks` folder where hook scripts live.  

---

### ✅ Step 2: Create the `post-update` hook file  
```bash
vi post-update
```
- Opens the file `post-update` in the `vi` editor.  
- If the file doesn’t exist, this command will create it.  

Inside the file, add the following script:  
```bash
#!/bin/bash
BRANCH=$(git rev-parse --abbrev-ref HEAD)
if [ "$BRANCH" = "master" ]; then
  TODAY=$(date +%F)
  echo "Creating release tag for $TODAY..."
  git tag -a "release-$TODAY" -m "Release for $TODAY"
  echo "Tag release-$TODAY created successfully"
fi
```  
This script:  
- Gets the current branch.  
- If it is `master`, creates a release tag with today’s date.  

---

### ✅ Step 3: Make the hook executable  
```bash
chmod +x post-update
```
- Hooks need **execution permission**.  
- Without this, Git won’t run the script.  

---

### ✅ Step 4: Go to the working copy of the repository  
```bash
cd /usr/src/kodekloudrepos/news
```
- This is where developers work and make changes.  
- `/opt/news.git` is the bare repo, but `/usr/src/kodekloudrepos/news` is the cloned repo.  

---

### ✅ Step 5: Merge feature branch into master  
```bash
git checkout master
git merge --no-ff feature -m "Merge feature into master"
```
- `git checkout master`: Switch to the master branch.  
- `git merge --no-ff feature`: Merge the `feature` branch into master without fast-forwarding.  
- `-m "..."`: Adds a commit message for the merge.  

---

### ✅ Step 6: Push changes to trigger the hook  
```bash
git push origin master
```
- Sends the merged changes from `master` to the remote (`/opt/news.git`).  
- The **post-update hook** runs automatically after the push.  
- It creates a release tag like `release-2025-09-11`.  

---

### ✅ Step 7: Verify the release tag  
```bash
git fetch --tags
git tag
git show release-$(date +%F)
```
- `git fetch --tags`: Downloads tags from the remote.  
- `git tag`: Lists all tags in the repository.  
- `git show release-$(date +%F)`: Displays details of today’s release tag.  

---

<img width="1280" height="689" alt="image" src="https://github.com/user-attachments/assets/e3774c26-adec-4f06-b996-670a7de7bb30" />

---

## 📑 Summary of Commands  

| Command | Explanation |
|---------|-------------|
| `cd /opt/news.git/hooks` | Navigate to hooks directory |
| `vi post-update` | Create/edit the hook file |
| `chmod +x post-update` | Make the hook executable |
| `cd /usr/src/kodekloudrepos/news` | Move to the cloned repo |
| `git checkout master` | Switch to master branch |
| `git merge --no-ff feature -m "..."` | Merge feature branch |
| `git push origin master` | Push changes to remote (triggers hook) |
| `git fetch --tags` | Fetch all tags |
| `git tag` | List available tags |
| `git show release-$(date +%F)` | Show details of today’s release tag |

---

<img width="1280" height="783" alt="image" src="https://github.com/user-attachments/assets/274cc442-b3ac-45af-883c-a0735a7a0792" />


✅ With this setup, every push to the master branch automatically creates a release tag with today’s date!  
