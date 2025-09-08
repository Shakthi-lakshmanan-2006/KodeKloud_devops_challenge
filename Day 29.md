# Day 29 Challenge – Working with Pull Requests in Gitea

## 🎯 Task (Simplified)
- Do **not** push directly to `master` (final branch).
- Max has already pushed his story **"Fox and Grapes"** into branch `story/fox-and-grapes`.
- We need to create a **Pull Request (PR)** to merge it into `master`.
- Assign **Tom** as the reviewer.
- Tom should review, approve, and merge the PR.

---

## 📝 Steps

### Step 1: SSH into storage server as Max
```bash
ssh max@<storage-server-ip>
# Password: Max_pass123
```

---

### Step 2: Check repo contents
```bash
cd ~
ls
cd <repo-folder-name>
```

---

### Step 3: View commit history
```bash
git log --oneline --decorate --graph --all
```
✔ Confirm you can see Sarah’s story commits and Max’s commit for *fox-and-grapes*.

---
<img width="859" height="1790" alt="Screenshot 2025-09-07 223920" src="https://github.com/user-attachments/assets/daa0d038-de71-4815-93fb-a26606ed0c9e" />

### Step 4: Open Gitea UI and Login as Max
- Click the **Gitea button** in the top bar.  
- Login with:
  - Username: `max`
  - Password: `Max_pass123`

---

### Step 5: Create a Pull Request
- Go to **Pull Requests → New Pull Request**  
- Fill in:
  - **From (source branch):** `story/fox-and-grapes`
  - **To (destination branch):** `master`
  - **PR Title:** `Added fox-and-grapes story`
- Click **Create Pull Request**.

---

### Step 6: Assign Reviewer (Tom)
- Inside the PR, on the right-hand side → **Reviewers**.  
- Add **tom** as reviewer.

---

### Step 7: Review and Merge as Tom
1. Logout from Max’s account.  
2. Login as Tom:
   - Username: `tom`
   - Password: `Tom_pass123`
3. Open the PR **Added fox-and-grapes story**.  
4. Approve the PR.  
5. Click **Merge Pull Request**.

---

## ✅ Final Outcome
- The branch `story/fox-and-grapes` is successfully merged into `master`.
- PR title: **Added fox-and-grapes story**.  
- Reviewer: **Tom** approved before merging.  
- `master` branch now contains the **Fox and Grapes story**.  
- ✔ This matches the required workflow (review → approve → merge).
- <img width="1883" height="861" alt="Screenshot 2025-09-07 223950" src="https://github.com/user-attachments/assets/0df2c463-c9d7-44f0-a287-13c6edda498c" />
