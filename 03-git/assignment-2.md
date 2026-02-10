# **Assignment-2: Git Branch Management & Git Diff**

## **Scenario**

You are working as a **DevOps Engineer** in a product-based company.

The `main` branch is **production-ready** and must never break.
Developers and DevOps engineers are expected to:

* Create feature branches
* Compare changes across branches
* Push branches to remote
* Clean up unused branches
* Rename branches when standards change

Your manager wants to ensure you **fully understand Git branching**, because all CI/CD pipelines depend on correct branch workflows.

---

## **Objective (Why We Are Doing This)**

By completing this assignment, you will:

* Understand **why branching is critical** in DevOps
* Create and manage branches **locally and remotely**
* Push and track branches correctly
* Compare changes using `git diff`
* Safely delete and rename branches
* Avoid common mistakes that break CI/CD pipelines

---

## **Prerequisites**

* Assignment-1 completed
* Existing GitHub repository from Assignment-1
* `main` branch already pushed to remote

---

## **Task-1: Understand Branching Conceptually (Mandatory)**

Before running commands, explain in your own words:

* What is a Git branch?
* Why do we **never work directly on `main`** in real projects?
* What happens internally when a branch is created?
* How branches relate to **commits and pointers**

📌 **DevOps Insight**
Explain how Git branches map to:

* Feature development
* Bug fixes
* Hotfixes
* Release pipelines

### **Expected Outcome**

* Clear conceptual understanding of branching
* Ability to explain branch usage in interviews

---

## **Task-2: Create a Branch Locally**

1. Check current branch:

   ```bash
   git branch
   ```
2. Create a new branch:

   ```bash
   git branch feature-readme-update
   ```
3. Switch to the branch:

   ```bash
   git checkout feature-readme-update
   ```

OR (modern approach):

```bash
git switch -c feature-readme-update
```

### **Expected Outcome**

* New branch created locally
* You are working on the feature branch

---

## **Task-3: Make Changes on Feature Branch**

1. Modify `README.md`
2. Add a new section:

   ```
   ## Git Branching Practice
   ```
3. Commit changes:

   ```bash
   git add README.md
   git commit -m "feat: add git branching section to README"
   ```

### **Expected Outcome**

* Commit exists **only** on feature branch
* `main` branch remains unchanged

---

## **Task-4: Push Local Branch to Remote Repository**

1. Push branch to GitHub:

   ```bash
   git push -u origin feature-readme-update
   ```
2. Verify branch exists on GitHub UI

### **Explain**

* Why `-u` is used
* What happens if you run `git push` without `-u`
* Difference between local and remote branches

### **Expected Outcome**

* Branch visible on GitHub
* Upstream tracking enabled

---

## **Task-5: List & Inspect Branches**

Run and understand:

```bash
git branch
git branch -r
git branch -a
```

Explain:

* What each command shows
* Difference between:

  * Local branches
  * Remote-tracking branches

### **Expected Outcome**

* Ability to identify where a branch exists

---

## **Task-6: Git Diff – Comparing Changes (Very Important)**

### **Scenario-1: Compare Working Tree Changes**

```bash
git diff
```

### **Scenario-2: Compare Staged vs Last Commit**

```bash
git diff --staged
```

### **Scenario-3: Compare Two Branches**

```bash
git diff main feature-readme-update
```

Explain:

* What exactly is being compared
* Why `git diff` is critical during code reviews and PRs

📌 **DevOps Insight**
Explain how `git diff` helps:

* Debug broken deployments
* Validate pipeline-triggering changes

### **Expected Outcome**

* Student can confidently compare branches and changes

---

## **Task-7: Rename a Branch**

### **Rename Locally**

```bash
git branch -m feature-readme-update feature-docs-update
```

### **Update Remote Branch Name**

```bash
git push origin -u feature-docs-update
git push origin --delete feature-readme-update
```

Explain:

* Why branch renaming is required in real projects
* Risks if old branches are not deleted

### **Expected Outcome**

* Branch renamed locally and remotely
* No stale branches left behind

---

## **Task-8: Delete Branches (Local & Remote)**

### **Delete Local Branch**

```bash
git branch -d feature-docs-update
```

If force delete is required:

```bash
git branch -D feature-docs-update
```

### **Delete Remote Branch**

```bash
git push origin --delete feature-docs-update
```

Explain:

* Difference between `-d` and `-D`
* When force deletion is dangerous

### **Expected Outcome**

* Clean branch list
* No unused branches

---

## **Additional Mandatory Concepts (Instructor Added)**

Students must also explain:

* Difference between:

  * `git branch`
  * `git checkout`
  * `git switch`
* What happens if you delete a branch with unmerged commits
* Why stale branches are dangerous in CI/CD
* How branch naming impacts automation pipelines

---

## **Submission Requirements**

Students must submit:

1. GitHub repository link
2. Screenshot showing:

   * Local branches
   * Remote branches
3. Commands used for:

   * Branch creation
   * Rename
   * Delete
4. Short explanation in `README.md`:

   * Why branching is mandatory in DevOps workflows

---

## **Success Criteria**

✔ Branches created locally and remotely

✔ Feature work isolated from `main`

✔ Git diff used correctly

✔ Branch renamed and deleted safely

✔ Clear understanding of real-world branching


