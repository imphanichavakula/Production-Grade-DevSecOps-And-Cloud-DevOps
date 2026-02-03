# Module 3 – Tasks | Git (Version Control)

These tasks simulate how Git is used in **real development teams**.

You will work with:
- Local repositories
- Remote repositories
- Branches
- Conflicts
- Recovery scenarios

---

# 🧩 PROJECT CONTEXT

You are working as a DevOps engineer on a team project.
The repository represents a production-ready application.

---

## Task 1 – Git Installation & Identity Setup

### Objective
Prepare your system for Git usage.

### What You Need to Do
- Install Git
- Configure username and email
- Verify installation

### Deliverables
- Git version output
- Git config output

### Interview Connect
- Why is Git configuration important?

---

## Task 2 – Understanding Git vs GitHub vs GitLab

### Objective
Understand the Git ecosystem.

### What You Need to Do
- Explain differences between Git, GitHub, GitLab, Bitbucket
- Explain where CI/CD fits in GitLab/GitHub

### Deliverables
- Short explanation in README or notes file

### Interview Connect
- Git vs GitHub?

---

## Task 3 – Create Local Repository

### Objective
Initialize a local Git repository.

### What You Need to Do
- Create a project directory
- Initialize Git repository
- Inspect `.git` folder

### Deliverables
- Repository initialization proof

### Interview Connect
- What is stored inside `.git`?

---

## Task 4 – Git Architecture (Blobs, Trees, Commits)

### Objective
Understand how Git stores data.

### What You Need to Do
- Make file changes
- Add and commit
- Inspect objects using Git plumbing commands

### Deliverables
- Commit hash proof
- Object inspection output

### Interview Connect
- What is a Git commit internally?

---

## Task 5 – Basic Git Workflow

### Objective
Follow the standard Git workflow.

### What You Need to Do
- Create files
- Stage changes
- Commit changes
- Modify and recommit

### Deliverables
- Commit history output

### Interview Connect
- Working directory vs staging vs repository?

---

## Task 6 – Connecting to Remote Repository

### Objective
Work with remote repositories.

### What You Need to Do
- Create GitHub repo
- Add remote origin
- Push commits
- Pull changes

### Deliverables
- Remote configuration output

### Interview Connect
- What happens during git push?

---

## Task 7 – Branch Creation & Switching

### Objective
Work with branches.

### What You Need to Do
- Create feature branch
- Switch branches
- Verify branch pointers

### Deliverables
- Branch listing output

### Interview Connect
- Why branches are lightweight in Git?

---

## Task 8 – Git Diff Usage

### Objective
Compare changes.

### What You Need to Do
- Diff working directory vs staging
- Diff staging vs last commit
- Diff between branches

### Deliverables
- Diff outputs

### Interview Connect
- How do you review changes before commit?

---

## Task 9 – Push Feature Branch to Remote

### Objective
Collaborate using branches.

### What You Need to Do
- Push feature branch
- Set upstream branch

### Deliverables
- Remote branch proof

### Interview Connect
- What is upstream tracking?

---

## Task 10 – Git Merge (Fast-Forward)

### Objective
Merge changes safely.

### What You Need to Do
- Merge feature branch using fast-forward
- Verify commit history

### Deliverables
- Merge output

### Interview Connect
- What is fast-forward merge?

---

## Task 11 – Git Merge (Three-Way)

### Objective
Handle divergent branches.

### What You Need to Do
- Create divergent commits
- Perform three-way merge

### Deliverables
- Merge commit proof

### Interview Connect
- Why merge commits exist?

---

## Task 12 – Git Rebase

### Objective
Maintain clean history.

### What You Need to Do
- Rebase feature branch
- Resolve rebase conflicts if any

### Deliverables
- Rebased history proof

### Interview Connect
- Rebase vs merge?

---

## Task 13 – Git Stash

### Objective
Handle unfinished work.

### What You Need to Do
- Stash uncommitted changes
- List stashes
- Apply and drop stash

### Deliverables
- Stash list output

### Interview Connect
- When do you use git stash?

---

## Task 14 – Multiple Stashes

### Objective
Manage multiple work states.

### What You Need to Do
- Create multiple stashes
- Apply specific stash

### Deliverables
- Stash apply proof

### Interview Connect
- How do you handle context switching?

---

## Task 15 – Git Cherry Pick

### Objective
Apply specific fixes.

### What You Need to Do
- Create commit in one branch
- Cherry-pick into another branch

### Deliverables
- Cherry-pick commit proof

### Interview Connect
- Cherry-pick use cases?

---

## Task 16 – Git Tags

### Objective
Mark releases.

### What You Need to Do
- Create lightweight tag
- Create annotated tag
- Push tags to remote

### Deliverables
- Tag listing output

### Interview Connect
- Tags vs branches?

---

## Task 17 – Git Squash

### Objective
Clean commit history.

### What You Need to Do
- Squash multiple commits
- Create single clean commit

### Deliverables
- Squashed commit proof

### Interview Connect
- Why squash before PR?

---

## Task 18 – Merge Conflicts (Same Line)

### Objective
Understand conflict mechanics.

### What You Need to Do
- Create conflict on same file & same line
- Resolve conflict manually

### Deliverables
- Conflict markers + resolution proof

### Interview Connect
- How conflicts occur?

---

## Task 19 – Merge Conflicts (Different Scenarios)

### Objective
Handle complex conflicts.

### What You Need to Do
- File delete vs modify conflict
- Different line conflict

### Deliverables
- Conflict resolution proof

### Interview Connect
- Conflict resolution strategy?

---

## Task 20 – Git Revert

### Objective
Safely undo changes.

### What You Need to Do
- Revert a commit
- Verify new revert commit

### Deliverables
- Revert commit proof

### Interview Connect
- Why revert is safe in production?

---

## Task 21 – Git Reset (Soft)

### Objective
Move HEAD safely.

### What You Need to Do
- Perform soft reset
- Inspect staging state

### Deliverables
- Reset proof

### Interview Connect
- Soft vs mixed reset?

---

## Task 22 – Git Reset (Mixed & Hard)

### Objective
Understand destructive operations.

### What You Need to Do
- Perform mixed reset
- Perform hard reset
- Recover lost commit using reflog

### Deliverables
- Reset & recovery proof

### Interview Connect
- Why hard reset is dangerous?

---

## Task 23 – Git Errors & Recovery

### Objective
Handle real Git errors.

### What You Need to Do
Fix:
- Detached HEAD
- Non-fast-forward error
- Pull failure due to divergence

### Deliverables
- Error + fix outputs

### Interview Connect
- How do you recover from Git mistakes?

---

## Task 24 – Corporate Branching Strategy

### Objective
Follow enterprise workflows.

### What You Need to Do
- Create:
  - main branch
  - feature branch
  - release branch
  - hotfix branch
- Explain branch lifecycle

### Deliverables
- Branch structure proof

### Interview Connect
- How releases are managed in corporates?

---

## Task 25 – Git Ignore

### Objective
Ignore unwanted files.

### What You Need to Do
- Create `.gitignore`
- Ignore logs, build files, secrets
- Verify ignored files

### Deliverables
- git status proof

### Interview Connect
- Why `.gitignore` is important?

---

## Task 26 – Pull Request Workflow

### Objective
Collaborate using PRs.

### What You Need to Do
- Create PR from feature branch
- Review changes
- Merge PR

### Deliverables
- PR screenshot/proof

### Interview Connect
- Why PRs are mandatory?

---

## Task 27 – GitHub Organization & RBAC

### Objective
Understand access control.

### What You Need to Do
- Explain GitHub roles
- Explain repo permissions
- Explain RBAC use cases

### Deliverables
- Permission explanation

### Interview Connect
- How access is managed in teams?

---

## Task 28 – Git Flow Strategy

### Objective
Implement Git Flow.

### What You Need to Do
- Create branches as per Git Flow
- Explain flow from dev to release

### Deliverables
- Branch flow diagram or explanation

### Interview Connect
- Git Flow vs Trunk-based development?

---

## Task 29 – Documentation & Notes

### Objective
Document Git usage.

### What You Need to Do
- Add notes explaining:
  - Common Git commands
  - Error recovery steps
  - Branching strategy

### Deliverables
- Notes file

### Interview Connect
- How documentation helps DevOps teams?
---

## 🌍 Learn in Public (Mandatory)

After completing this module:

1. Push your work to your forked GitHub repo  
2. Post your learning on LinkedIn explaining:
   - What Git concepts became clear
   - What mistakes you made
   - How you recovered from them
3. Tag:
   - **Aditya Jaiswal**
   - **DevOps Shack**

Once you tag, your **task and repository will be reviewed**.

Use hashtags:

```
#Batch13 #Git #DevOps #VersionControl #LearnInPublic #devopsshack
```

---

## 🎯 Outcome of Module-3

After completing Module-3, you should be able to:
- Use Git confidently in team environments
- Handle branches, conflicts, and releases
- Recover safely from mistakes
- Follow corporate Git workflows
- Answer advanced Git interview questions with confidence

---
