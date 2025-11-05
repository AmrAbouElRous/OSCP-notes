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
