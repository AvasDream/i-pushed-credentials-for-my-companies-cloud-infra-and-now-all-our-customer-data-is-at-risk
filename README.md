### How to remove credentials from a git repository

```bash
# 1. Remove .env from the current commit
git rm --cached .env

# 2. Add .env to .gitignore
echo ".env" >> .gitignore
git add .gitignore
git commit -m "Add .env to .gitignore"

# 3. Rewrite history to remove .env from all commits
git filter-branch --force --index-filter \
'git rm --cached --ignore-unmatch .env' \
--prune-empty --tag-name-filter cat -- --all

# 4. Clean up repository
rm -rf .git/refs/original/
git reflog expire --expire=now --all
git gc --prune=now --aggressive

# 5. Force-push cleaned history to GitHub
git push --force --all
git push --force --tags

# 6. Inform collaborators to reset their local repositories
# (they can use the following commands)
git fetch --all
git reset --hard origin/main
```
