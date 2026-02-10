# **Assignment-3: Git Merge, Git Rebase & Merge Conflicts (Deep Dive)**

---

## **Scenario**

Your team is working on a shared codebase.

Multiple engineers are modifying files **at the same time**.
As a DevOps engineer, your job is to **integrate code safely** without breaking production or CI/CD pipelines.

This assignment teaches you:

* How branches are merged
* How rebasing works internally
* Why merge conflicts happen
* How to resolve conflicts **calmly and correctly**


## **Objective (Why We Are Doing This)**

After this assignment, you will be able to:

* Understand **merge vs rebase** clearly
* Perform merges safely
* Rebase feature branches correctly
* Handle **real conflict scenarios**
* Explain conflict resolution in interviews and real projects

---

## **Prerequisites**

* Assignment-1 & Assignment-2 completed
* Repository contains:

  * `main` branch
  * At least one feature branch experience

---

# 🔹 PART-1: Git Merge (No Conflict First)

## **Task-1: Understand Git Merge (Concept)**

### What is Git Merge?

* Git merge **combines two branches**
* It creates a **merge commit** (unless fast-forward)

📌 **Important Rule**

> In real projects, **`main` is never rebased** — only merged into.

---

## **Task-2: Create a Feature Branch**

```bash
git checkout main
git pull origin main
git switch -c feature-readme
```

### Make a change:

Edit `README.md` and add:

```
## Feature Branch Merge Practice
```

Commit:

```bash
git add README.md
git commit -m "feat: add merge practice section"
```

---

## **Task-3: Merge Feature Branch into main (No Conflict)**

```bash
git checkout main
git merge feature-readme
```

### What happened?

* Git performed a **fast-forward merge**
* No conflict because:

  * No one else changed the same file

### Expected Outcome

✔ Feature merged cleanly
✔ No merge conflict

---

# 🔹 PART-2: Merge Conflicts (Multiple Scenarios)

Now we **intentionally create conflicts**.

---

## 🔥 Conflict Scenario-1: Same File, Different Lines (Easy)

### Step-1: Create Two Branches

```bash
git checkout main
git switch -c feature-a
```

Edit `notes.txt`:

```
Feature A implementation notes
```

Commit:

```bash
git commit -am "feat: add feature A notes"
```

Now second branch:

```bash
git checkout main
git switch -c feature-b
```

Edit `notes.txt` (different line):

```
Feature B logging improvements
```

Commit:

```bash
git commit -am "feat: add feature B notes"
```

---

### Step-2: Merge feature-a

```bash
git checkout main
git merge feature-a
```

✔ Merge succeeds

---

### Step-3: Merge feature-b

```bash
git merge feature-b
```

✔ Merge succeeds
❗ No conflict because changes are on **different lines**

---

## 🔥 Conflict Scenario-2: Same File, Same Line (Real Conflict)

### Step-1: Reset back to main

```bash
git checkout main
git switch -c conflict-feature-1
```

Edit `notes.txt`:

```
Deployment strategy: Rolling
```

Commit:

```bash
git commit -am "feat: update deployment strategy"
```

---

### Step-2: Second Branch

```bash
git checkout main
git switch -c conflict-feature-2
```

Edit **same line** in `notes.txt`:

```
Deployment strategy: Blue-Green
```

Commit:

```bash
git commit -am "feat: update deployment strategy differently"
```

---

### Step-3: Merge First Branch

```bash
git checkout main
git merge conflict-feature-1
```

✔ Merge success

---

### Step-4: Merge Second Branch (Conflict Happens)

```bash
git merge conflict-feature-2
```

❌ **Merge conflict occurs**

---

### Step-5: Resolve Conflict Manually

Open `notes.txt`:

```
<<<<<<< HEAD
Deployment strategy: Rolling
=======
Deployment strategy: Blue-Green
>>>>>>> conflict-feature-2
```

Choose correct content (example):

```
Deployment strategy: Blue-Green
```

Then:

```bash
git add notes.txt
git commit -m "merge: resolve deployment strategy conflict"
```

---

## 🔹 PART-3: Git Rebase (Clean History)

## **Task-4: Understand Git Rebase**

### What is Rebase?

* Rebase **replays commits** on top of another branch
* Creates **linear history**

⚠️ **Golden Rule**

> Never rebase shared branches like `main`

---

## **Task-5: Rebase a Feature Branch**

```bash
git checkout main
git switch -c feature-rebase-demo
```

Make a change:

```bash
echo "Rebase demo" >> notes.txt
git commit -am "feat: add rebase demo"
```

Update main:

```bash
git checkout main
echo "Main branch update" >> notes.txt
git commit -am "chore: update main branch"
```

Now rebase:

```bash
git checkout feature-rebase-demo
git rebase main
```

---

### If Conflict Occurs During Rebase

1. Fix conflict
2. Stage file:

```bash
git add notes.txt
```

3. Continue rebase:

```bash
git rebase --continue
```

---

## 🔹 PART-4: Visualize & Understand History

```bash
git log --oneline --graph --all
```

Explain:

* Merge commit vs rebased commits
* Why rebase history is cleaner

---

## **Additional Mandatory Concepts**

Students must explain:

* Why Git cannot auto-resolve some conflicts
* Difference between:

  * merge conflict
  * rebase conflict
* `git rebase --abort`
* `git merge --abort`
* Why DevOps teams prefer **merge for shared branches**

---

## **Success Criteria**

✔ Multiple conflict scenarios handled

✔ Manual conflict resolution done

✔ Merge vs rebase clearly understood

✔ No rebasing of main branch


