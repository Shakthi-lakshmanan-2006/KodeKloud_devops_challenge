# 🚀 Day 31 Challenge – Restore Stashed Changes in Git

## 🧑‍🏫 Step 1: Basics You Need to Know
- `git stash` temporarily saves uncommitted changes.
- Use `git stash list` to view saved stashes.
- Use `git stash apply stash@{id}` to restore a specific stash.

---

## 📝 Step 2: The Task Breakdown
- ✅ Repository: `/usr/src/kodekloudrepos/demo`
- ✅ Restore stash with **stash@{1}**
- ✅ Commit and push the changes to origin

---
<img width="1919" height="1001" alt="Screenshot 2025-09-08 191410" src="https://github.com/user-attachments/assets/1d619318-2b04-40a2-877b-27538afd0f81" />



## 🚀 Step 3: Step-by-Step Solution

```bash
# 1. Go to the repository
cd /usr/src/kodekloudrepos/demo

# 2. List available stashes
git stash list

# 3. Apply the stash with stash@{1}
git stash apply stash@{1}

# 4. Verify the changes
git status

# 5. Stage the restored changes
git add .

# 6. Commit the changes
git commit -m "Restored stashed changes from stash@{1}"

# 7. Push the changes to the origin
git push origin master
```
<img width="1919" height="1008" alt="Screenshot 2025-09-08 191501" src="https://github.com/user-attachments/assets/9cf36dee-918d-4fca-9d94-c71f8d6d22f4" />

