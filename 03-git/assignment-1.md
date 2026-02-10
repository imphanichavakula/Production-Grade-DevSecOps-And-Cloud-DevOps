# **Assignment-1: Git Fundamentals & Repository Setup**

## **Scenario**

You have just joined a company as a **Junior DevOps Engineer**.

On Day-1, your manager asks you to:

* Set up Git on your system
* Create your own GitHub repository
* Understand how Git actually works **internally**
* Push your first code following proper Git practices

This task ensures you are **ready to work with real repositories** used in CI/CD pipelines.

---

## **Objective (Why We Are Doing This)**

By completing this assignment, you should be able to:

* Understand **what Git is and why it exists**
* Understand **how Git works internally** (Blobs, Trees, Commits, Snapshots)
* Confidently work with:

  * Untracked files
  * Staged files
  * Commits
  * Local → Remote workflow
* Push code to GitHub using **industry-standard commit messages**
* Understand **upstream and downstream** flow in Git

This is **non-negotiable** knowledge for any DevOps engineer.

---

## **Prerequisites**

* A laptop/VM with **Linux / macOS / Windows**
* GitHub account (free)
* Basic terminal usage

---

## **Task-1: Install Git**

### **Steps**

1. Install Git based on your OS:

   * Linux
   * macOS
   * Windows
2. Verify installation using:

   ```bash
   git --version
   ```
3. Configure Git identity:

   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your-email@example.com"
   ```

### **Expected Outcome**

* Git is installed and accessible from terminal
* Git identity is configured correctly

---

## **Task-2: Understand Git Architecture (Mandatory)**

Before touching any commands, understand and **document in your own words**:

### **Explain the following concepts:**

* What problem Git solves
* Centralized vs Distributed Version Control
* Git working areas:

  * Working Directory
  * Staging Area (Index)
  * Local Repository
  * Remote Repository

### **Git Internals (Very Important)**

Explain with diagrams or text:

* **Blob** – what it stores
* **Tree** – how directory structure is maintained
* **Commit object**
* **Snapshots vs diffs**
* Why Git is fast and reliable

> 📌 **DevOps Insight**:
> Explain why understanding Git internals helps during **merge conflicts, rollbacks, and CI/CD debugging**.

### **Expected Outcome**

* Student can explain Git internals without memorizing commands
* Clear conceptual understanding (this will be used later)

---

## **Task-3: Create a Remote Repository (GitHub)**

1. Log in to GitHub
2. Create a **new repository**
3. Repository naming rules:

   * Use lowercase
   * Use hyphen (`-`)
   * Example:

     ```
     git-learning-devops
     ```
4. Add:

   * Repository description
   * Make it **public**
   * Do NOT initialize with README (we’ll do it locally)

### **Expected Outcome**

* A clean GitHub repository is created and ready to receive code

---

## **Task-4: Initialize Local Repository & First Commit**

1. Create a local project directory:

   ```bash
   mkdir git-learning-devops
   cd git-learning-devops
   ```
2. Initialize Git:

   ```bash
   git init
   ```
3. Create files:

   * `README.md`
   * `notes.txt`
4. Add meaningful content inside both files

### **Understand File States**

Check file status:

```bash
git status
```

Identify and explain:

* Untracked files
* Staged files
* Modified files

### **Stage & Commit**

```bash
git add .
git commit -m "Initial commit: add project structure and notes"
```

### **Expected Outcome**

* Files move from:

  ```
  Untracked → Staged → Committed
  ```
* Commit message follows best practices

---

## **Task-5: Connect Local Repo to Remote (Upstream / Downstream)**

1. Add remote origin:

   ```bash
   git remote add origin <repo-url>
   ```
2. Verify:

   ```bash
   git remote -v
   ```
3. Push code:

   ```bash
   git push -u origin main
   ```

### **Explain in Your Own Words**

* What is **upstream**?
* What is **downstream**?
* What does `-u` flag do?
* Difference between `git push` and `git push -u`

### **Expected Outcome**

* Code is visible on GitHub
* Branch tracking is set correctly

---

## **Task-6: Commit Hygiene (Very Important for DevOps)**

1. Make a small change in `notes.txt`
2. Commit using a **clear message**:

   * ❌ `update file`
   * ✅ `docs: add notes on git architecture`
3. Push changes to GitHub

### **Expected Outcome**

* Clean commit history
* Meaningful commit messages

---

## **Additional Mandatory Concepts (Added by Instructor)**

Students **must also understand and explain**:

* Difference between:

  * Git vs GitHub
  * Clone vs Pull
  * Commit vs Push
* Why `.git` folder is critical
* What happens if `.git` folder is deleted
* Why DevOps pipelines fail due to bad Git practices

---

## **Submission Requirements**

Students must submit:

1. GitHub repository link
2. Screenshot of:

   * `git status`
   * Commit history
3. A short explanation (in README.md):

   * What they learned
   * One real-world use case of Git in DevOps

---

## **Success Criteria**

✔ Git installed and configured

✔ Repository created properly

✔ Clear understanding of Git internals

✔ Code pushed successfully

✔ Proper commit practices followed

---


