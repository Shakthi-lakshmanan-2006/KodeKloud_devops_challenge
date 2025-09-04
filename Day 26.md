# Git Remote and File Commit Task

## Task  
Update the Git repository cloned at `/usr/src/kodekloudrepos/games` by adding a new remote, committing a file, and pushing the changes.  

### Requirements:
1. Add a new remote named **`dev_games`** pointing to `/opt/xfusioncorp_games.git`.  
2. Copy the file `/tmp/index.html` into the repo and commit it to the **master** branch.  
3. Push the **master** branch to the new remote.  

---

## Steps  

1. **Navigate to the repo directory**:  
   ```bash
   cd /usr/src/kodekloudrepos/games
   ```

2. **Add the new remote**:  
   ```bash
   git remote add dev_games /opt/xfusioncorp_games.git
   ```
   - `git remote add` → adds a new remote repository.  
   - `dev_games` → the name we assign to the remote.  
   - `/opt/xfusioncorp_games.git` → location of the remote repo.  

   🔍 **Verify the remote**:  
   ```bash
   git remote -v
   ```
   Expected output:  
   ```bash
   dev_games   /opt/xfusioncorp_games.git (fetch)
   dev_games   /opt/xfusioncorp_games.git (push)
   ```

3. **Copy the file into the repo**:  
   ```bash
   cp /tmp/index.html .
   ```
   - `cp` → Linux copy command.  
   - `/tmp/index.html` → the source file.  
   - `.` → destination is the current directory (the repo).  

   🔍 **Verify the file exists**:  
   ```bash
   ls
   ```
   Expected output (should include `index.html`):  
   ```bash
   index.html
   ```

4. **Add and commit the file**:  
   ```bash
   git add index.html
   git commit -m "Added index.html file"
   ```
   - `git add index.html` → stages the file.  
   - `git commit -m "message"` → records the change in history.  

   🔍 **Verify the commit**:  
   ```bash
   git log --oneline | head -5
   ```
   Expected output:  
   ```bash
   abcd123 Added index.html file
   ```

5. **Push the master branch to the new remote**:  
   ```bash
   git push dev_games master
   ```
   - `git push` → uploads commits to a remote.  
   - `dev_games` → the remote we just added.  
   - `master` → the branch to push.  

   🔍 **Verify remote branches**:  
   ```bash
   git branch -r
   ```
   Expected output:  
   ```bash
   dev_games/master
   ```

---

![WhatsApp Image 2025-09-04 at 08 32 29_9b83e305](https://github.com/user-attachments/assets/f107654f-f6ca-4f86-ad6e-2394a66ee38e)
![WhatsApp Image 2025-09-04 at 08 32 41_73d13662](https://github.com/user-attachments/assets/29028c58-fbf7-4032-abe7-1723e56d3faf)

## Command Summary  

| Command | Purpose |
|---------|---------|
| `cd /usr/src/kodekloudrepos/games` | Move into the project repo. |
| `git remote add dev_games /opt/xfusioncorp_games.git` | Add a new remote named `dev_games`. |
| `git remote -v` | Verify remotes configured in the repo. |
| `cp /tmp/index.html .` | Copy the `index.html` file into the repo directory. |
| `git add index.html` | Stage the file for commit. |
| `git commit -m "Added index.html file"` | Commit the file with a message. |
| `git log --oneline | head -5` | Verify the recent commit was created. |
| `git push dev_games master` | Push the master branch to the new remote `dev_games`. |
| `git branch -r` | Confirm that the branch was pushed to the new remote. |
