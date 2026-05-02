# GIT – GLOBAL INFORMATION TRACKER

![GIT Workflow Diagram](media/image1.png)

## 🧠 VCS – Version Control System  
Git keeps code separately for each version.

- v1 → 100 lines → repo-1  
- v2 → 200 lines → repo-2  
- v3 → 300 lines → repo-3  

## 📁 REPO  
A folder where we store code.  
Example: `index.html` in different versions.

---

## 🔰 Introduction to Git

- Tracks files  
- Maintains multiple versions of the same file  
- Platform independent  
- Free & open source  
- Handles large projects efficiently  
- 3rd generation VCS  
- Written in C  
- Released in **2005**

---

## 🏛️ CVCS vs DVCS

| CVCS (Centralized)       | DVCS (Distributed)       |
|--------------------------|--------------------------|
| Example: SVN             | Example: **Git**         |
| Single repo              | Multiple repos           |

---

## ⏪ Rollback  
Going back to a previous version of the application.

---

## 📂 Stages in Git

| Stage               | Description                         |
|---------------------|-------------------------------------|
| Working Directory   | Write source code                   |
| Staging Area        | Track files                         |
| Repository          | Store tracked source code           |

---

## 🛠️ Working with Git (Commands)

```bash
mkdir swiggy && cd swiggy
yum install git -y
git -v
git init
vim index.html
git status
git add index.html
git commit -m "commit-1" index.html
git log
git log -2
git log -2 --oneline
git diff
