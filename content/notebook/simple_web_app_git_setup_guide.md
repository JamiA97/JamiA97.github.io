
---
title: Simple Web App Development Tech Stack
tags: [internet]
---
# Simple Web App Development Tech Stack

## Stack Components

1. **Use vanilla HTML**
2. **Use JavaScript** - Access external libraries as needed.
3. **Use SQLite** for database actions.
4. **Use Node.js** for server actions.
5. **Use Git/GitHub** for version control.

### Project Structure
```
Project
├── package.json
├── package-lock.json
├── public
│   ├── index.html
│   ├── script.js
│   └── styles.css
├── database.db
└── server.js
```

Testing will be done locally on an Ubuntu laptop using Firefox or Chrome. Files will be edited using VSCode.

## Git Setup Guide

### Step 1: Install Git
```bash
sudo apt update
sudo apt install git
```
Verify installation:
```bash
git --version
```

### Step 2: Configure Git
```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### Step 3: Initialize Git in Your Project Directory
```bash
cd /path/to/your/project
git init
```

### Step 4: Create a `.gitignore` File
Add a `.gitignore` file to prevent unwanted files from being committed.
Example:
```
node_modules/
*.log
database.db
.env
```
Add and commit:
```bash
touch .gitignore
git add .gitignore
git commit -m "Add .gitignore"
```

### Step 5: Create a New Repository on GitHub
1. Create a new repository on GitHub.
2. Copy the repository URL (HTTPS or SSH).

### Step 6: Connect Local Repo to GitHub
```bash
git remote add origin https://github.com/your-username/your-repo.git
```

### Step 7: First Commit and Push
```bash
git add .
git commit -m "Initial commit"
git push -u origin master
```

### Step 8: Git Workflow

1. Check status:
    ```bash
    git status
    ```

2. Stage changes:
    ```bash
    git add <file>
    ```

3. Commit changes:
    ```bash
    git commit -m "Commit message"
    ```

4. Push changes:
    ```bash
    git push
    ```

### Branching
- Create a new branch:
    ```bash
    git checkout -b new-feature-branch
    ```

- Merge branch:
    ```bash
    git checkout master
    git merge new-feature-branch
    git push
    ```

- Pull updates:
    ```bash
    git pull
    ```

---

This guide helps set up a simple web app development environment using Node.js, SQLite, Git, and GitHub.
