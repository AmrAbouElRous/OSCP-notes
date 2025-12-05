# OSCP Git Guide

A simple personal guide to set up and push an OSCP notes repository.

---

## Create the Repository

```bash
mkdir oscp && cd oscp

git init

git remote add origin git@github.com:AmrAbouElRous/OSCP-notes.git

echo "# OSCP-notes" >> README.md

git add .

git commit -m "initial commit"

git branch -M main

git push -u origin main
```

---

## Auto Push Script

File: `auto_push_oscp.sh`

```bash
#!/bin/bash

# Set the path to your repo
REPO_PATH="$HOME/Documents/oscp"
COMMIT_MSG="Auto-update on $(date '+%Y-%m-%d %H:%M:%S')"

# Change to repo directory
cd "$REPO_PATH" || { echo "Repo path not found!"; exit 1; }

# Add all changes
git add .

# Commit with a timestamp
git commit -m "$COMMIT_MSG"

# Pull first to avoid push conflicts
git pull origin main --rebase

# Push to the main branch
git push origin main
```

---

## Notes

* Run the script manually or with cron if you want automation.
* Make sure your SSH keys are configured for GitHub.
* Default branch renamed to `main` for consistency.

---
## More Safe Auto Push Script
File: `auto_push_oscp_safe`

```bash
#!/bin/bash

# Path to your repository
REPO_PATH="$HOME/Documents/oscp"
COMMIT_MSG="Auto-update on $(date '+%Y-%m-%d %H:%M:%S')"

echo "[*] Switching to repo: $REPO_PATH"
cd "$REPO_PATH" || { echo "[ERROR] Repo path not found!"; exit 1; }

echo "[*] Adding changes..."
git add -A

echo "[*] Committing..."
git commit -m "$COMMIT_MSG" --quiet || echo "[*] No changes to commit."

echo "[*] Pulling latest changes (rebase, no edit)..."
git pull origin main --rebase --no-edit --autostash --quiet \
  || { echo "[ERROR] Pull failed! Resolve manually."; exit 1; }

echo "[*] Pushing to GitHub..."
git push origin main --quiet \
  || { echo "[ERROR] Push failed!"; exit 1; }

echo "[✔] Auto-push complete at $(date '+%Y-%m-%d %H:%M:%S')"

```
---

## 1️⃣ Git LFS For Large binary/files
- Git Large File Storage (LFS) is designed for this exact case. It replaces large files with pointers in the repo and uploads the binaries separately.
- Install Git LFS:
```bash
sudo apt install git-lfs   # Debian/Ubuntu
git lfs install
```
- Track image files:
```bash
cd ~/Documents/oscp
git lfs track "*.png"
git add .gitattributes
git commit -m "Track PNG images with Git LFS"
```
- Then, for future image additions, Git will automatically use LFS. This prevents your pushes from hanging.

## 2️⃣ Increase SSH/Git buffer size (if needed)
- For large pushes:
``` bash
git config --global http.postBuffer 524288000  # 500MB
git config --global core.compression 0
```
- This helps Git not choke on big uploads.

## 3️⃣ Future-proof auto-push script
### Once you switch to LFS:
- All big images are pushed quickly
- Script won’t hang
- No need for splitting commits manually
