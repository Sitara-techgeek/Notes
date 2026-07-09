
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

## One Example

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

```
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

# File States in Git

> **📅 Written:** 08 Jul 2026

### Definition

A **file state** represents the current status of a file in Git.

A file can be in one of **four states**.

---

## 1. Untracked

### Definition

A file that Git has **never tracked before**.

Example:

```
touch notes.md
```

Now,

```
git status
```

Output:

```
Untracked files:
    notes.md
```

Git knows the file exists but isn't managing it yet.

---

## 2. Staged

### Definition

A file that is **ready to be committed**.

Command:

```
git add notes.md
```

or

```
git add .
```

The file is now in the **Staging Area**.

---

## 3. Committed (Tracked)

### Definition

A file whose current version has been **saved in the Local Repository**.

Command:

```
git commit -m "Add notes"
```

Git now tracks this version.

---

## 4. Modified

### Definition

A **tracked file** that has been edited after the last commit.

Example:

```
git.md
```

You add another topic.

Run:

```
git status
```

Output:

```text
modified: git.md
```

The changes exist only in the Working Directory until you stage and commit them.

---

## Lifecycle

```
New File
    │
    ▼
Untracked
    │
 git add
    ▼
  Staged
    │
git commit
    ▼
Committed
    │
  Edit
    ▼
Modified
    │
git add
    ▼
Staged
```

---

## Quick Revision

| State     | Meaning                                | Command to Reach  |
| --------- | -------------------------------------- | ----------------- |
| Untracked | Git doesn't track the file             | Create a new file |
| Staged    | Ready for commit                       | `git add`         |
| Committed | Saved in Local Repository              | `git commit`      |
| Modified  | Tracked file changed after last commit | Edit the file     |

---

## Interview Definition ⭐

> **A file state in Git represents the current status of a file during its lifecycle, such as Untracked, Staged, Committed (Tracked), or Modified.**

---

## One small correction

Many resources say there are only **three states**:

* Modified
* Staged
* Committed

That's because they assume the file is **already being tracked** by Git.

It's much clearer to include **Untracked** as the first state, since every new file starts there. It's also the state you'll frequently see in `git status` when adding new files to your repository.

---

## Can we change a file from staged to not staged (Modified) ? 

Yes. Absolutely. In fact, this is something developers do quite often.

Suppose you staged a file:

```
git add git.md
```

Now the file is in the **Staged** state.

Later you realize:

> "Oops! I don't want this change in my next commit."

You can **unstage** it.

### Command

```
git restore --staged git.md
```

or, for all staged files:

```
git restore --staged .
```

### Lifecycle

```
Modified
    │
git add
    ▼
Staged
    │
git restore --staged
    ▼
Modified
```

Notice something important:

> **The file contents are NOT lost.**

The file still contains your edits. Only its **state** changes from **Staged** back to **Modified**.

---

### Example

Suppose you have:

```
git.md
```

You edit it and run:

```
git add git.md
```

`git status` shows:

```
Changes to be committed:
    modified: git.md
```

Now you decide not to include it in the next commit.

Run:

```
git restore --staged git.md
```

Now `git status` shows:

```
Changes not staged for commit:
    modified: git.md
```

The changes are still in your file—they're just no longer staged.

---

### Interview point ⭐

A file can move **both forward and backward** between states.

```
 Untracked
    │
 git add
    ▼
 Staged
    │
git restore --staged
    ▼
Modified (or Untracked, depending on the file)
```

Git is flexible—you can stage, unstage, edit again, stage again, and only commit when you're satisfied.

This is one of the reasons the **Staging Area** exists: it gives you precise control over what goes into each commit.

---

# Essential Git Commands ⭐

> **📅 Written:** 08 Jul 2026

We'll learn them in workflow order.

---

## 1. `git status` 

### Definition

`git status` shows the current state of your repository.

It tells you:

* Modified files
* Untracked files
* Staged files
* Current branch
* Whether you have changes to commit

Example:

```
git status
```

Output:

```
Changes not staged for commit:
    modified: git.md
```

Meaning:

> "You changed this file, but you haven't added it to staging yet."

---

## 2. `git add`

### Definition

`git add` moves changes from the **Working Directory** to the **Staging Area**.

Example:

```
git add git.md
```

Add all files:

```
git add .
```

Flow:

```
Working Directory
        |
        | git add
        ↓
Staging Area
```

---

## 3. `git commit`

### Definition

`git commit` saves the staged changes as a snapshot in the Local Repository.

Example:

```
git commit -m "Add Git lifecycle notes"
```

Important:

A commit is **local**.

It is not yet on GitHub.

Flow:

```
Staging Area
        |
        | git commit
        ↓
Local Repository
```

---

## 4. `git push`

### Definition

`git push` uploads your local commits to a remote repository like GitHub.

Example:

```
git push
```

Flow:

```
Local Repository
        |
        | git push
        ↓
     GitHub
```

---

## 5. `git pull`

### Definition

`git pull` downloads the latest changes from GitHub and updates your local repository.

Example:

```
git pull
```

Used when:

* Team members pushed new changes.
* You want the latest version.

Flow:

```
GitHub
   |
   | git pull
   ↓
Your Computer
```

---

## 6. `git log`

### Definition

Shows commit history.

Example:

```
git log
```

Output:

```
commit a82f91
Author: K.Sravani

Add Git lifecycle notes

commit b91ac3

Initial commit
```

Useful when you want to know:

* What changes happened?
* When?
* By whom?

---

## 7. `git diff`

### Definition

Shows the difference between your current file and the last committed version.

Example:

```
git diff
```

You edited:

```
int a=5;
```

Changed to:

```
int a = 5;
```

Git shows the difference.

---

## 8. `git restore`

### Definition

Used to undo changes.

Example:

Discard modifications:

```
git restore git.md
```

Unstage a file:

```
git restore --staged git.md
```

---

## Your Daily Git Workflow

Most of your life as a developer will look like this:

```
# Check what changed
git status

# Prepare changes
git add .

# Save snapshot
git commit -m "Meaningful message"

# Upload
git push
```

---


# Advanced Git Commands

> **📅 Written:** 09 Jul 2026

---

## 1. `git clone`

### Definition

`git clone` downloads a **copy of an existing remote repository** (like one on GitHub) onto your computer, including its full commit history.

Example:

```
git clone https://github.com/username/My_Notes.git
```

Unlike `git init`, which creates a brand-new empty repo, `git clone` copies one that already exists.

Flow:

```
GitHub (Remote Repository)
        │
     git clone
        ▼
Your Computer (Local Repository)
```

---

## 2. `git config`

### Definition

`git config` sets your identity and preferences for Git. Every commit needs an author name and email attached to it.

Example:

```
git config --global user.name "K.Sravani"
git config --global user.email "you@example.com"
```

`--global` applies this to every repo on your machine. Drop it to set config for just one repo.

---

# Branching

## 3. `git branch`

### Definition

`git branch` lists, creates, or deletes branches.

Examples:

```
git branch              # list branches
git branch new-feature  # create a branch
git branch -d old-branch  # delete a branch
```

A branch is just a separate line of development — changes made there don't affect `main` until merged.

---

## 4. `git switch` / `git checkout`

### Definition

Moves you **into** a different branch.

Examples:

```
git switch new-feature
```

Older syntax (still widely used):

```
git checkout new-feature
```

Create and switch in one step:

```
git switch -c new-feature
```

or

```
git checkout -b new-feature
```

---

## 5. `git merge`

### Definition

`git merge` combines the changes from one branch into another.

Example:

Suppose you finished work on `new-feature` and want it in `main`:

```
git switch main
git merge new-feature
```

Flow:

```
main            new-feature
  │                  │
  │                  ▼
  │           (commits made here)
  │                    │
  ◀──────── git merge ┘
  │
combined history
```

---

## Quick Revision — Branching

| Command | Purpose |
| --- | --- |
| `git branch` | List/create/delete branches |
| `git switch <name>` | Move to a branch |
| `git switch -c <name>` | Create + move to a new branch |
| `git merge <name>` | Bring another branch's changes into current branch |

---

# Remote Management

## 6. `git remote`

### Definition

`git remote` manages the connections between your local repo and remote repos like GitHub.

Examples:

```
git remote -v              # view connected remotes
git remote add origin https://github.com/username/My_Notes.git
```

`origin` is just the conventional name for your main remote — you could call it anything.

---

## 7. `git push -u origin main`

### Definition

The `-u` (`--set-upstream`) flag links your local branch to a specific remote branch, so future pushes only need `git push`.

Example:

```
git push -u origin main
```

After this, plain `git push` and `git pull` know exactly where to send/fetch from.

---

# .gitignore

### Definition

A **`.gitignore`** file tells Git which files or folders to **never track** — things like dependencies, secrets, or build output.

Example `.gitignore`:

```
node_modules/
.env
*.log
```

Without it, Git would try to track and commit things it shouldn't — bloating your repo or leaking secrets.

> **Important:** `.gitignore` only stops *untracked* files from being tracked. If a file is already committed, you must remove it from tracking first (`git rm --cached <file>`).

---

# Undoing Changes

## 8. `git reset`

### Definition

`git reset` moves your branch pointer backward, undoing commits. It has three modes:

```
git reset --soft <commit>    # undo commit, keep changes staged
git reset --mixed <commit>   # undo commit, keep changes unstaged (default)
git reset --hard <commit>    # undo commit, discard changes completely
```

Example — undo your last commit but keep the edits:

```
git reset --soft HEAD~1
```

> **Caution:** `--hard` permanently deletes uncommitted work. Use carefully.

---

## 9. `git revert`

### Definition

`git revert` undoes a commit by creating a **new commit** that reverses its changes — the original commit stays in history.

Example:

```
git revert a82f91
```

---

## `git reset` vs `git revert` ⭐

This is a classic interview question.

| | `git reset` | `git revert` |
| --- | --- | --- |
| History | Rewrites/removes history | Preserves history |
| Safe for shared branches? | No — risky if others pulled the commit | Yes — safe to use anywhere |
| How it undoes | Moves branch pointer backward | Adds a new "undo" commit |
| Typical use | Local commits not yet pushed | Commits already pushed/shared |

> **Rule of thumb:** Use `reset` for your own local, unpushed mistakes. Use `revert` once a commit is on a shared/remote branch.

---

# Fetch vs Pull

## 10. `git fetch`

### Definition

`git fetch` downloads new commits from the remote **without merging them** into your working branch.

Example:

```
git fetch origin
```

Your local branch stays untouched — you just now have the latest remote data available to inspect.

---

## `git fetch` vs `git pull` ⭐

```
git pull = git fetch + git merge
```

| | `git fetch` | `git pull` |
| --- | --- | --- |
| Downloads new commits | Yes | Yes |
| Merges into your branch | No | Yes |
| Risk | Safe — lets you review first | Can cause surprise merges/conflicts |

> **Interview point:** `fetch` is the "look before you leap" version of `pull`.

---

# Stashing

## 11. `git stash`

### Definition

`git stash` temporarily shelves your uncommitted changes so you can switch branches with a clean working directory — without committing half-finished work.

Examples:

```
git stash          # save current changes
git stash list     # view stashed changes
git stash pop      # reapply the most recent stash and remove it from the list
git stash apply    # reapply without removing from the list
```

Example scenario:

You're mid-edit on `git.md` but need to urgently switch to `main` to fix something.

```
git stash
git switch main
# fix the urgent issue
git switch new-feature
git stash pop
```

Your half-finished edits come right back.

---

# Merge Conflicts

### Definition

A **merge conflict** happens when Git can't automatically combine changes — usually because the same lines were edited differently on two branches.

Example — Git marks the conflicting section in the file:

```
<<<<<<< HEAD
This is the version on your current branch.
=======
This is the version from the branch you're merging.
>>>>>>> new-feature
```

### How to resolve:

1. Open the file and manually edit it — keep, combine, or rewrite the conflicting lines.
2. Remove the `<<<<<<<`, `=======`, `>>>>>>>` markers.
3. Stage the resolved file:
   ```
   git add git.md
   ```
4. Commit to complete the merge:
   ```
   git commit
   ```

---

## Quick Revision — Advanced Commands

| Command | Purpose |
| --- | --- |
| `git clone` | Copy a remote repo locally |
| `git config` | Set your Git identity |
| `git branch` | List/create/delete branches |
| `git switch` / `checkout` | Move between branches |
| `git merge` | Combine branch changes |
| `git remote` | Manage remote connections |
| `.gitignore` | Exclude files from tracking |
| `git reset` | Undo commits (rewrites history) |
| `git revert` | Undo commits (new commit, safe for shared history) |
| `git fetch` | Download remote changes without merging |
| `git pull` | Fetch + merge |
| `git stash` | Temporarily shelve uncommitted changes |
| Merge conflict markers | `<<<<<<<`, `=======`, `>>>>>>>` |

---

## Interview Definition ⭐

> **Advanced Git commands extend basic version control with branching for parallel development, remote management for collaboration, and tools like reset, revert, fetch, and stash for controlling exactly what enters your history and when.**

---
