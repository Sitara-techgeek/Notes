
# Git

> **📅 Written:** 06 Jul 2026

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

> **📅 Written:** 06 Jul 2026


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



I'm glad you switched to Markdown. 😄 Once you start writing notes in `.md`, it's hard to go back to `.txt`. GitHub renders Markdown beautifully, and it's the standard format for documentation.

Let's restart **Topic 2** in a way that's concise enough for your notes.

---

# Repository & Git File Lifecycle

> **📅 Written:** 07 Jul 2026

## Repository (Repo)

### Definition

A **Git Repository** is a folder that Git tracks and manages.

When you run:

```
git init
```

Git creates a hidden **`.git`** folder inside your project.

### Example:

```
My_Notes/
│
├── git.md
└── .git/
```

The `.git` folder stores:

* Commit history
* Branches
* Configuration
* Metadata

> **Without `.git`, it's just a normal folder.**

---

# Git File Lifecycle

> **📅 Written:** 07 Jul 2026

## 1. Working Directory

The **Working Directory** is the folder where you create and edit files.

Example:

```
git.md
```

Suppose you add:

```md
## Git Definition

Git is a Distributed Version Control System.
```

The file is modified, but Git hasn't saved the changes yet.

---

## 2. Staging Area (Index)

### Definition

The **Staging Area** is a temporary area where you select changes before committing them.

Command:

```
git add git.md
```

or to add all changes (of all files)

```
git add .
```

Think of it as a **waiting room** before a commit.

---

## 3. Local Repository

### Definition

The **Local Repository** is where Git permanently stores your commits on **your computer.**

Command:

```
git commit -m "Add Git definition"
```

A **commit** is a snapshot of your project at that moment.

---

## 4. Remote Repository

A **Remote Repository** is a copy of your repository stored on a server like GitHub.

Command:

```
git push
```

uploads your commits to GitHub.

---

## Complete Lifecycle

```
Edit File
     │
     ▼
Working Directory
     │
  git add
     ▼
Staging Area
     │
 git commit
     ▼
Local Repository
     │
  git push
     ▼
GitHub (Remote Repository)
```

---

## Why do we need the Staging Area?

Suppose you modified:

```
git.md
github.md
random.md
```

Only `git.md` is ready.

Stage only that file:

```
git add git.md
```

Then commit:

```
git commit -m "Update Git notes"
```

The other two files remain unchanged in the Working Directory.

This lets you create **small, meaningful commits** instead of mixing unrelated changes.

---

## Quick Revision

* **Repository** → Folder managed by Git.
* **Working Directory** → Where you edit files.
* **Staging Area** → Where you prepare selected changes for a commit.
* **Local Repository** → Stores commits on your computer.
* **Remote Repository** → Repository hosted on GitHub.

---

### One important clarification

Many beginners think:

> **Working Directory → Local Repository**

But that's **not** how Git works.

Every change must go through the **Staging Area** first:

```
Working Directory
        ↓
   Staging Area
        ↓
 Local Repository
        ↓
Remote Repository
```

That staging step is what gives Git its flexibility and control over what gets included in each commit. It's one of Git's defining features and worth remembering.


---


## Why doesn't Git allow pushing without a commit?

`git push` sends **commits**, not individual file changes.
Think about what GitHub stores.

GitHub stores a sequence of commits:

```
Commit 1
↓
Commit 2
↓
Commit 3
```

It **doesn't store your working directory**.

If Git allowed pushing uncommitted changes:

* There would be no commit message.
* No author information.
* No timestamp tied to a snapshot.
* No clear project history.

Commits are the fundamental units of Git history, so Git requires one before pushing.

---

