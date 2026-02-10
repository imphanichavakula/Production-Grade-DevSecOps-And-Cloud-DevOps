# Assignment-5: Git Tags & Git Squash (Concept → Why → How → Scenarios)

## PART-0: Read This First (Context)

In real DevOps projects:

* **Tags** are used by:

  * release pipelines
  * rollback strategies
  * hotfix creation
* **Squash** is used by:

  * developers before PR merge
  * teams to keep Git history readable
  * CI/CD logs to stay clean

If you don’t understand **why** these exist, the commands will feel random.

---

# PART-1: GIT TAGS

## 1️⃣ What exactly is a Git Tag?

A **Git tag** is a **fixed label** attached to a **specific commit**.

Important points:

* A tag **does not move**
* A tag always points to **one exact commit**
* Even if new commits come later, the tag stays there

### Simple analogy

* Branch → moving pointer (keeps changing)
* Tag → permanent milestone

Example:

```
v1.0.0  → commit A
v1.1.0  → commit B
```

---

## 2️⃣ Why do we use Git Tags? (Very Important)

In DevOps & Cloud projects:

* CI/CD pipelines deploy **tags**, not random commits
* Rollback means: “Go back to tag v1.0.0”
* Hotfix means: “Start from production tag”

Without tags:

* You don’t know what exactly was deployed
* Rollbacks become guesswork

---

## Scenario-1: Create a Production Release Tag

### Step-1: Make sure you are on `main`

```bash
git checkout main
git pull origin main
git status
```

✅ Must be clean.

---

### Step-2: See which commit will be released

```bash
git log --oneline -5
```

📌 **Important**
You are tagging the **current HEAD of main**.

---

### Step-3: Create an annotated tag (recommended)

```bash
git tag -a v1.0.0 -m "Release v1.0.0 – first production release"
```

Why annotated?

* Stores message
* Stores author
* Acts like a real release record

---

### Step-4: Verify the tag

```bash
git tag
git show v1.0.0
```

You should see:

* tag name
* release message
* commit details

---

### Step-5: Push tag to remote

```bash
git push origin v1.0.0
```

✅ Now GitHub & CI/CD can see this release.

---

## Scenario-2: Lightweight Tag (Quick Marker)

### Why lightweight tags exist

* Temporary markers
* Quick references
* Not used for formal releases

### Steps

```bash
git checkout main
git tag v1.0.0-lite
git push origin v1.0.0-lite
```

## Scenario-3: Rollback or Hotfix Using a Tag (Real Production Flow)

### Why this scenario matters

In production:

* Code running in prod = **a tag**
* Hotfixes start from **that exact prod state**
* You never guess a commit hash in emergencies

---

### Step-1: Checkout a release tag

(You are moving to the exact production code)

```bash
git checkout v1.0.0
git status
```

You should see:

```
HEAD detached at v1.0.0
```

📌 **What this means**

* You are NOT on a branch
* You are looking at **exact release code**
* You should NOT commit here

---

### Step-2: Create a hotfix branch from the tag

(This is the safe and correct way)

```bash
git checkout -b hotfix/v1.0.1
```

Verify:

```bash
git branch
```

You should see:

```
* hotfix/v1.0.1
```

---

### Step-3: Apply the hotfix

```bash
echo "Critical production hotfix" >> hotfix.txt
git add hotfix.txt
git commit -m "fix: critical hotfix for v1.0.1"
```

---

### Step-4: Push hotfix branch

```bash
git push -u origin hotfix/v1.0.1
```

✅ **Outcome**

* Hotfix is based on production code
* `main` is untouched
* CI/CD can deploy this branch or merge it

---

## Scenario-4: Delete Tags (Cleanup & Safety)

### Why deletion matters

* Wrong tags confuse deployments
* Old markers should be removed

### Delete locally

```bash
git tag -d v1.0.0-lite
```

### Delete remotely

```bash
git push origin --delete v1.0.0-lite
```

---

# PART-2: GIT SQUASH

## 1️⃣ What exactly is Git Squash?

**Git squash** means:

> Combining multiple commits into **one clean commit**

Nothing else.

Example before squash:

```
wip: fix typo
wip: fix typo again
wip: update spacing
```

After squash:

```
feat: add validation logic
```

---

## 2️⃣ Why do teams squash commits?

In real teams:

* Developers commit frequently
* PR reviewers want **clean history**
* CI/CD logs should be readable

Squash helps:

* clean PR history
* easier rollbacks
* easier debugging

---

## 3️⃣ When should you NOT squash?

❌ Never squash:

* `main` after it is shared
* commits already pulled by others
* production history

Squash is for:

* **feature branches**
* **before merge**

---

## Scenario-5: Create a Messy Feature Branch (On Purpose)

### Step-1: Create branch

```bash
git checkout main
git pull origin main
git checkout -b feature/squash-demo
```

---

### Step-2: Create multiple commits

```bash
echo "step 1" >> squash.txt
git add squash.txt
git commit -m "wip: step 1"

echo "step 2" >> squash.txt
git add squash.txt
git commit -m "wip: step 2"

echo "step 3" >> squash.txt
git add squash.txt
git commit -m "wip: step 3"
```

---

### Step-3: Verify messy history

```bash
git log --oneline -5
```

You should see **3 wip commits**.

---

## Scenario-6: Squash Using Interactive Rebase (Most Important)

### Why rebase is used here

Interactive rebase lets you:

* choose which commits stay
* combine commits
* rewrite messages

---

### Step-1: Start interactive rebase

(You are on `feature/squash-demo`)

```bash
git rebase -i HEAD~3
```

---

### Step-2: Edit rebase todo file

You will see:

```
pick a1b2c3 wip: step 1
pick d4e5f6 wip: step 2
pick g7h8i9 wip: step 3
```

Change to:

```
pick a1b2c3 wip: step 1
squash d4e5f6 wip: step 2
squash g7h8i9 wip: step 3
```

Save and close.

---

### Step-3: Write final commit message

Replace everything with:

```
feat: add squash demo feature
```

Save and close editor.

---

### Step-4: Verify squash worked

```bash
git log --oneline -5
```

You should now see **ONE commit**.

---

## Scenario-7: Push Squashed Branch Safely

### Case-1: Branch was never pushed

```bash
git push -u origin feature/squash-demo
```

---

### Case-2: Branch was already pushed (Advanced)

```bash
git push --force-with-lease
```

📌 **Why not `--force`?**

* `--force-with-lease` protects others’ work

---

## Scenario-8: Squash Merge (Simpler Alternative)

### What squash merge does

* Takes all changes from a branch
* Creates **one commit on main**
* Does NOT preserve branch commit history

---

### Step-1: Create a branch with multiple commits

```bash
git checkout main
git checkout -b feature/squash-merge

echo "A" >> merge.txt
git add merge.txt
git commit -m "feat: add A"

echo "B" >> merge.txt
git add merge.txt
git commit -m "feat: add B"
```

---

### Step-2: Squash merge into main

```bash
git checkout main
git merge --squash feature/squash-merge
git commit -m "feat: add merge feature (squashed)"
```

---

### Step-3: Verify

```bash
git log --oneline -5
```

Only **one commit** is added to `main`.

---

# FINAL SUMMARY (Must Be Written by Student)

Students must explain:

### Git Tags

* What a tag is
* Why tags are used for releases
* Difference between annotated and lightweight tags
* How hotfixes start from tags

### Git Squash

* What squash does
* Why teams squash commits
* Difference between rebase-squash and squash-merge
* Why squash should never be done on shared branches

---

## Success Criteria

✔ Student understands **why tags exist**

✔ Student can create, push, delete tags

✔ Student can start hotfix from a tag

✔ Student can squash commits cleanly

✔ Student can explain when squash is safe


