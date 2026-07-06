
# Git

### Definition

**Git is a distributed Version Control System (VCS) that tracks changes in your project over time.**

**In one sentence:**

> Git keeps a history of your code so you can undo mistakes, work on new features safely, and collaborate with others.

**Example:**

```
Day 1 → Login Page
Day 2 → Added Signup
Day 3 → Fixed Login Bug
```

Git remembers each stage.

---

## Key Features of Git

* Tracks changes in files
* Stores project history (commits)
* Allows rollback to previous versions
* Supports branching (parallel development)
* Merges changes from different branches
* Works offline
* Fast because most operations are local

---

# GitHub

### Definition

**GitHub is a cloud platform that hosts Git repositories and enables collaboration among developers.**

**In one sentence:**

> GitHub stores your Git projects online so they can be backed up, shared, and worked on by multiple people.

---

## Key Features of GitHub

* Cloud storage for Git repositories
* Backup of projects
* Collaboration with teams
* Pull Requests for code review
* Issue tracking
* Open-source hosting
* Access from anywhere

---

# Git vs GitHub
---------------------------------------------------------------
| Git                        | GitHub                         |
| -------------------------- | ------------------------------ |
| Version Control System     | Cloud hosting platform         |
| Installed on your computer | Website                        |
| Tracks project history     | Stores Git repositories online |
| Works offline              | Requires internet for syncing  |
| Used to commit changes     | Used to push/pull repositories |
---------------------------------------------------------------
---

# One Example

Imagine you're building a Calculator app.

You write the code on your laptop.

```
Calculator Project
        │
        ▼
      Git
(remembers every change)
        │
git push
        │
        ▼
    GitHub
(stores it online)
```

Later,

* Your laptop crashes → Download it again from GitHub.
* Your friend wants to contribute → Invite them on GitHub.
* You accidentally break the code → Restore an earlier version using Git.

---

## The only distinction you must never forget

> **Git = Version Control Tool**

> **GitHub = Online Hosting Platform for Git Repositories**

---
