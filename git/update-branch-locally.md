Switching from `main` to `production`
```bash
git branch -m main production
git fetch origin
git branch -u origin/production production
git remote set-head origin -a
```
