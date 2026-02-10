# Assignment-7: Git Ignore, Pull Request Workflow, and GitHub Organization Setup (RBAC)

## Objective

By the end of this assignment, students will:

* Understand why `.gitignore` is mandatory in real projects
* Implement a complete Pull Request workflow instead of direct merges
* Work inside a GitHub Organization with proper role-based access control
* Understand who can do what in a corporate Git setup

This assignment connects **Git usage** with **team collaboration and governance**.

---

## PART-1: Git Ignore (Repository Hygiene)

## What is .gitignore

`.gitignore` tells Git **which files should never be tracked**.

These files usually:

* are machine-specific
* are secrets or configs
* are build artifacts
* should never go to GitHub

If `.gitignore` is wrong:

* secrets leak
* repos become noisy
* PR reviews become painful

---

## Why .gitignore Is Important in DevOps

* CI/CD pipelines generate files that must not be committed
* Logs, builds, binaries should never go to Git
* Cloud credentials must never reach the repo

This is **non-negotiable** in production systems.

---

## Step-1: Create a .gitignore File

You must be on `main`.

```bash
git checkout main
git pull origin main
```

Create `.gitignore`:

```bash
touch .gitignore
```

Add the following entries:

```text
# Logs
*.log
logs/

# OS files
.DS_Store
Thumbs.db

# Environment files
.env
.env.*

# Build artifacts
dist/
build/
target/

# IDE files
.vscode/
.idea/

# Terraform
.terraform/
*.tfstate
*.tfstate.backup
```

Commit `.gitignore`:

```bash
git add .gitignore
git commit -m "chore: add standard gitignore"
git push origin main
```

---

## Step-2: Verify .gitignore Is Working

Create ignored files:

```bash
touch app.log
mkdir logs
touch .env
```

Check status:

```bash
git status
```

Expected:

* These files should NOT appear in git status

Explanation students must understand:

* `.gitignore` works only for **untracked files**
* Already tracked files must be removed explicitly

---

## PART-2: Pull Request Workflow (How Teams Actually Work)

## What is a Pull Request

A Pull Request (PR):

* is a request to merge code
* triggers reviews
* runs CI checks
* protects important branches

In real companies:

* direct merge to main / prod is blocked
* PR approval is mandatory

---

## Why PRs Are Mandatory in DevOps

* Prevents breaking production
* Enables code review
* Enforces CI/CD checks
* Provides audit trail

---

## Step-3: Feature Development Using PR Workflow

Feature work always starts from a base branch (`dev` or `qa`).

Assume base branch is `dev`.

```bash
git checkout dev
git pull origin dev
git checkout -b feature/pr-demo
```

Make a change:

```bash
echo "PR demo feature" >> app.txt
git add app.txt
git commit -m "feat: add pr demo feature"
git push -u origin feature/pr-demo
```

---

## Step-4: Create Pull Request on GitHub

On GitHub UI:

* Base branch: `dev`
* Compare branch: `feature/pr-demo`
* Title: `Add PR demo feature`
* Description: explain what was changed

Create the PR.

---

## Step-5: Review and Merge PR

Simulate review:

* Check file changes
* Add a comment (optional)

Merge strategy:

* Use **Squash and Merge** (recommended)

After merge:

* Delete feature branch

---

## Step-6: Verify Merge Result

On local:

```bash
git checkout dev
git pull origin dev
git log --oneline -5
```

Expected:

* One clean commit from the PR
* Feature branch changes present

---

## PART-3: GitHub Organization Setup and RBAC (Follow Video)

This section must be done **exactly as shown in the video**.

---

## What is a GitHub Organization

A GitHub Organization:

* owns repositories
* manages teams
* controls access
* enforces policies

In companies:

* personal repos are never used
* everything lives inside orgs

---

## Step-7: Create GitHub Organization

Follow video and create organization.

Naming example:

```text
devopsshack-org
```

Plan:

* Free plan is enough

---

## Step-8: Create Teams Inside Organization

Create the following teams:

```text
admins
developers
qa
release
sre
```

---

## Step-9: Assign RBAC Roles (Very Important)

Assign permissions as follows:

| Team       | Permission |
| ---------- | ---------- |
| admins     | Admin      |
| developers | Write      |
| qa         | Write      |
| release    | Maintain   |
| sre        | Maintain   |

Rules students must understand:

* Developers cannot merge to prod
* QA cannot force push
* Only release/SRE handle prod merges
* Admins control branch protection

---

## Step-10: Add Repository to Organization

Move or create repository inside the organization.

Example repo:

```text
application-git-sandbox
```

Assign team access:

* developers → Write
* qa → Write
* release → Maintain
* sre → Maintain

---

## Step-11: Enable Branch Protection Rules

Apply on:

* `main`
* `prod`

Rules to enable:

* Require pull request before merging
* Require at least 1 approval
* Restrict who can push
* Disable force push

---

## What Students Must Explain (README)

* Why .gitignore is mandatory
* What problems PR workflow solves
* Why direct commits to main/prod are blocked
* Difference between personal repo vs org repo
* How RBAC prevents production incidents

---

## Submission Requirements

Students must submit:

* Repo link inside organization
* Screenshot of `.gitignore`
* Screenshot of PR created and merged
* Screenshot of organization teams
* Screenshot of branch protection rules
* README explanations completed

---

## Success Criteria

* .gitignore correctly ignores files
* Feature merged only via PR
* Repo exists inside GitHub organization
* Teams and RBAC are configured correctly
* Student understands governance, not just Git commands


