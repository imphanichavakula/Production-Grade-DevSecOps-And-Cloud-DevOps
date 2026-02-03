# Git & GitHub Project Assignment — BoardgameListingWebApp

You will build this project **using Git + GitHub workflows exactly like a corporate team**.

**Base Repository (Public):** `jaiswaladi246/Boardgame`  
This repository is a Spring Boot full-stack web app (board games + reviews) and already contains files like `Dockerfile`, `Jenkinsfile`, GitHub workflows

---

## What You Will Learn

- Forking and cloning a real-world repo
- Branching (feature/release/hotfix)
- Commit discipline and meaningful messages
- Pull Requests + reviews + approvals
- Merge conflicts and how to resolve them
- Rebase, squash, cherry-pick, stash
- Tags and release flow
- Git error recovery (detached HEAD, non-fast-forward, etc.)
- A corporate-style GitHub workflow end-to-end

---

## 📌 Rules

- This is a **public repo project** — your work must be visible on your GitHub fork.
- You must use **branches + PRs** (no direct pushes to `main` except where explicitly told).
- Keep commits small and meaningful.
- Each task must be verifiable through Git history and PRs.

---

## 🍴 Step 0 — Forking & Naming (Mandatory)

1. Fork the repository to your GitHub.
2. Rename your fork using your name as prefix:

**Example**
- `rahul-boardgame-git-project`
- `priya-boardgame-git-project`

3. Clone **your fork** locally (not the original).

---

## Project Tasks (Do in Sequence)

### Task 1 — Local Setup & Git Identity
**Objective:** Prepare your Git environment.

**What You Need to Do**
- Install Git
- Set `user.name` and `user.email`
- Verify Git version

**Deliverables**
- Screenshot/output of git version + config

**Interview Connect**
- Why Git config matters in teams?

---

### Task 2 — Clone & Inspect Repository
**Objective:** Understand repo structure.

**What You Need to Do**
- Clone your fork
- Explore file structure
- Identify default branch and latest commit

**Deliverables**
- `git log --oneline` output (latest 10)

**Interview Connect**
- What does `git clone` create locally?

---

### Task 3 — Create Branch Policy Locally
**Objective:** Practice working like corporate teams.

**What You Need to Do**
- Create branch naming format locally:
  - `feature/<name>-<topic>`
  - `bugfix/<name>-<topic>`
  - `hotfix/<name>-<topic>`

**Deliverables**
- `git branch` output showing branch names

**Interview Connect**
- Why naming conventions matter?

---

### Task 4 — Feature Branch: Documentation Change
**Objective:** Make your first feature using a branch.

**What You Need to Do**
- Create `feature/<name>-readme-update`
- Add a new section in the repo README called **“Student Notes”**
- Commit with a meaningful message
- Push branch to remote

**Deliverables**
- Commit hash + branch pushed proof

**Interview Connect**
- What makes a “good commit message”?

---

### Task 5 — Open Your First Pull Request
**Objective:** Use PR workflow.

**What You Need to Do**
- Create PR from your feature branch → your `main`
- Add PR title + description
- Merge PR using **Squash merge** (if available)

**Deliverables**
- PR link (in your fork) + merged proof

**Interview Connect**
- Why PRs are mandatory in enterprises?

---

### Task 6 — Git Diff Mastery
**Objective:** Learn how to review changes properly.

**What You Need to Do**
- Create `feature/<name>-diff-demo`
- Modify 2 files (small changes)
- Use:
  - `git diff`
  - `git diff --staged`
  - `git diff main..branch`

**Deliverables**
- Copy/paste diff outputs into a new file: `git-notes/diff-output.md`

**Interview Connect**
- How do you verify what you’re pushing?

---

### Task 7 — Merge vs Rebase Demo (Real)
**Objective:** Practice both strategies.

**What You Need to Do**
- Create two feature branches with small commits
- Merge one branch into `main` using merge commit
- Rebase the second branch on latest `main`
- Push and create PR for the rebased branch

**Deliverables**
- `git log --graph --oneline --decorate` screenshot/output

**Interview Connect**
- When to use merge vs rebase?

---

### Task 8 — Stash & Pop (Context Switching)
**Objective:** Handle unfinished work cleanly.

**What You Need to Do**
- Start editing a file but **don’t commit**
- `git stash` changes
- Switch branch and do a small commit
- Come back and `git stash pop`

**Deliverables**
- `git stash list` output + proof changes restored

**Interview Connect**
- When stash is useful in real teams?

---

### Task 9 — Cherry Pick (Hotfix Backport)
**Objective:** Apply one commit across branches.

**What You Need to Do**
- Create a `release/<name>-v1` branch from `main`
- Create a `hotfix/<name>-critical-fix` branch
- Make 1 commit on hotfix branch
- Cherry-pick that commit into release branch

**Deliverables**
- Commit hash shown on both branches

**Interview Connect**
- Where is cherry-pick used in production?

---

### Task 10 — Tags & Release Practice
**Objective:** Learn release tagging.

**What You Need to Do**
- Create tag: `v1.0.0-<name>` (annotated tag)
- Push tags to remote

**Deliverables**
- `git tag -n` output

**Interview Connect**
- Tags vs branches?

---

### Task 11 — Squash Commits Before PR
**Objective:** Clean PR history.

**What You Need to Do**
- Create a feature branch
- Make 4 small commits
- Squash them into 1 commit before PR
- Open PR and merge

**Deliverables**
- Before/after log output

**Interview Connect**
- Why teams squash before merging?

---

### Task 12 — Merge Conflict Scenario 1 (Same Line)
**Objective:** Learn conflict resolution.

**What You Need to Do**
- Create 2 branches from `main`
- Modify same line in same file in both branches
- Merge one, then attempt merge other → conflict
- Resolve manually and complete merge

**Deliverables**
- Screenshot/output showing conflict markers + final resolution

**Interview Connect**
- How do conflicts happen?

---

### Task 13 — Merge Conflict Scenario 2 (Delete vs Modify)
**Objective:** Handle tricky conflicts.

**What You Need to Do**
- In branch A: delete a file
- In branch B: modify the same file
- Merge and resolve correctly

**Deliverables**
- Proof of resolution (final repo state + merge commit)

**Interview Connect**
- How do you choose the correct resolution?

---

### Task 14 — Git Revert (Safe Undo)
**Objective:** Undo safely on shared branches.

**What You Need to Do**
- Make a commit to `main` via PR
- Revert that commit using `git revert`
- Push and PR the revert commit

**Deliverables**
- Revert commit hash + PR merged proof

**Interview Connect**
- Why revert is safer than reset?

---

### Task 15 — Git Reset (Soft/Mixed/Hard) + Recovery
**Objective:** Understand reset types and recovery.

**What You Need to Do**
- On a new branch:
  - Do a commit
  - `reset --soft` and show staged changes
  - `reset --mixed` and show unstaged changes
  - `reset --hard` (only on the branch)
- Recover a lost commit using `git reflog`

**Deliverables**
- `reflog` output + recovered commit proof

**Interview Connect**
- Why hard reset is dangerous on shared branches?

---

### Task 16 — Git Errors & Fixes Pack
**Objective:** Become confident fixing Git problems.

**What You Need to Do**
- Simulate and fix:
  - Detached HEAD
  - Non-fast-forward push rejection
  - Pull failure due to divergence

**Deliverables**
- A notes file: `git-notes/errors-and-fixes.md`

**Interview Connect**
- How do you debug Git failures under pressure?

---

### Task 17 — Corporate Branching Strategy (Mini Implementation)
**Objective:** Implement a simple enterprise branching model.

**What You Need to Do**
Create branches:
- `main`
- `develop`
- `feature/*`
- `release/*`
- `hotfix/*`

Explain your branch flow in:
- `git-notes/branching-strategy.md`

**Deliverables**
- Branch list output + notes file

**Interview Connect**
- Release vs hotfix branches?

---

### Task 18 — Pull Request Workflow (Review Checklist)
**Objective:** Standard PR discipline.

**What You Need to Do**
- Create a PR with:
  - Clear title
  - Description with “What / Why / Testing”
  - Checklist (self-review)
- Merge only after self-review checklist is complete

**Deliverables**
- PR proof + checklist text

**Interview Connect**
- What makes a PR high quality?

---

### Task 19 — Git Ignore Implementation
**Objective:** Keep repo clean and safe.

**What You Need to Do**
- Update `.gitignore` to ignore:
  - logs
  - IDE files
  - build artifacts
  - `.env` style secrets
- Prove ignored files do not appear in `git status`

**Deliverables**
- `.gitignore` update + status proof

**Interview Connect**
- Why ignoring secrets is critical?

---

### Task 20 — Final Project Proof
**Objective:** Present your work like a real engineer.

**What You Need to Do**
Create a final document:
- `git-notes/final-summary.md` including:
  - What you learned
  - Most common mistakes you faced
  - How you fixed them
  - 5 commands you now understand deeply

**Deliverables**
- `final-summary.md` committed on `main`

**Interview Connect**
- Explain your Git workflow end-to-end.

---

## Submission

- Your submission is your **GitHub fork** (public)
- Make sure your `main` branch contains:
  - `git-notes/` folder (with required notes files)
  - Clean commit history
  - PRs merged as asked

---

## 🌍 Learn in Public (Mandatory)

After completing the project:
1. Post on LinkedIn: what you built + what you learned.
2. Tag **Aditya Jaiswal** and **DevOps Shack**.
3. Once tagged, your **repository and work will be reviewed**.

Suggested hashtags:
```
#Batch13 #Git #GitHub #DevOps #LearnInPublic #devopsshack
```

---

## 🏁 Outcome

After completing this project, you will:
- Work confidently with Git in real teams
- Handle PRs, conflicts, and recovery
- Understand enterprise branching + releases
- Be interview-ready for Git scenarios
---
