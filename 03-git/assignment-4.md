# Assignment-4: Git Undo & Recovery (Sandwich Model, Step-by-Step)

**Topics:** Revert vs Reset → Checkout → Stash/Pop → Cherry-pick

## Repo Setup (Do this once)

### Step 0.1: Confirm repo + clean state

**You must be on `main`**

```bash
git checkout main
git pull origin main
git status
```

✅ You should see: `working tree clean`

### Step 0.2: Create a practice file (if not already)

```bash
echo "Undo practice file" > undo.txt
git add undo.txt
git commit -m "chore: add undo practice file"
git push origin main
```

---

# PART-1: REVERT vs RESET (Clear rules + scenarios)

## Rule (must remember)

* If commit is **already pushed** → use **revert**
* If commit is **only local** → use **reset**

---

## Scenario-1: Revert a bad commit that is already pushed (SAFE for teams)

### Step 1.1: Create a “bad commit” on a separate branch

**You must be on `main`**

```bash
git checkout main
git pull origin main
git checkout -b feature/bad-change
```

### Step 1.2: Make a bad change + commit

```bash
echo "BAD CHANGE" >> undo.txt
git add undo.txt
git commit -m "feat: introduce bad change for practice"
```

### Step 1.3: Push the branch (simulate real work pushed)

```bash
git push -u origin feature/bad-change
```

### Step 1.4: Merge the bad change into `main` (simulate mistake reaching main)

**Switch to main**

```bash
git checkout main
git merge feature/bad-change
git push origin main
```

### Step 1.5: Get the bad commit hash (THIS is what you asked clarity for)

**You must be on `main`**

```bash
git log --oneline -10
```

✅ Copy the commit hash of:
`feat: introduce bad change for practice`

Example (your hash will differ):
`a1b2c3d feat: introduce bad change for practice`

### Step 1.6: Revert it (SAFE)

**Still on `main`**

```bash
git revert a1b2c3d
```

If editor opens, save and close.

### Step 1.7: Push revert commit

```bash
git push origin main
```

✅ Expected Outcome:

* A **new commit** appears: `Revert "feat: introduce bad change for practice"`
* `undo.txt` no longer contains `BAD CHANGE`

### Verification:

```bash
tail -n 5 undo.txt
git log --oneline -5
```

---

## Scenario-2: Reset a commit that is NOT pushed (LOCAL cleanup)

### Step 2.1: Create a local-only commit

**You must be on `main`**

```bash
git checkout main
git pull origin main
```

Make a local change:

```bash
echo "LOCAL ONLY CHANGE" >> undo.txt
git add undo.txt
git commit -m "wip: local only change (do not push)"
```

✅ Confirm commit exists locally:

```bash
git log --oneline -3
```

### Step 2.2: Use reset (choose one mode)

#### Option A: Keep changes staged (soft)

```bash
git reset --soft HEAD~1
```

✅ Result: commit removed, changes remain staged.

#### Option B: Keep changes unstaged (mixed – default)

```bash
git reset HEAD~1
```

✅ Result: commit removed, changes remain in working directory.

#### Option C: Discard changes completely (hard – dangerous)

```bash
git reset --hard HEAD~1
```

✅ Result: commit removed AND changes deleted.

### Verification:

```bash
git log --oneline -3
git status
```

---

# PART-2: CHECKOUT SAFETY (branch switching without losing work)

## Scenario-3: Checkout blocked due to local changes

### Step 3.1: Create a branch to switch into

**From main**

```bash
git checkout main
git checkout -b feature/switch-test
```

Return to main:

```bash
git checkout main
```

### Step 3.2: Make an uncommitted change

```bash
echo "uncommitted work" >> undo.txt
```

Try switching:

```bash
git checkout feature/switch-test
```

✅ You may see an error about overwriting changes.

### Fix Options (do ONE at a time, verify each)

#### Fix A: Commit before switching

```bash
git add undo.txt
git commit -m "wip: save before switching"
git checkout feature/switch-test
```

#### Fix B: Stash before switching (most common)

Go back to main (if needed) and create change again, then:

```bash
git checkout main
echo "uncommitted work again" >> undo.txt
git stash push -m "temp work before switching branch"
git checkout feature/switch-test
```

#### Fix C: Discard change (only if useless)

```bash
git checkout main
git restore undo.txt
git checkout feature/switch-test
```

---

## Scenario-4: Detached HEAD and recovery (VERY IMPORTANT)

### Step 4.1: Copy an old commit hash

**You must be on main**

```bash
git checkout main
git log --oneline -10
```

Copy any commit hash like `xxxxxxx`.

### Step 4.2: Checkout commit hash (this causes detached HEAD)

```bash
git checkout xxxxxxx
git status
```

✅ You should see: `HEAD detached`

### Step 4.3: Recover safely by creating a branch

```bash
git checkout -b debug/detached-recovery
```

✅ Verification:

```bash
git branch
git status
```

---

# PART-3: STASH & POP (context switch like real production)

## Scenario-5: Mid-feature → urgent hotfix → return to work

### Step 5.1: Start feature work

```bash
git checkout main
git checkout -b feature/long-work
echo "feature work line 1" >> undo.txt
echo "feature work line 2" >> undo.txt
```

Do NOT commit yet.

### Step 5.2: Stash it

```bash
git stash push -m "wip: feature long work paused"
git status
```

✅ Clean now.

### Step 5.3: Create hotfix branch from main

```bash
git checkout main
git checkout -b hotfix/urgent-fix
echo "HOTFIX applied" >> undo.txt
git add undo.txt
git commit -m "fix: urgent hotfix"
```

Push hotfix:

```bash
git push -u origin hotfix/urgent-fix
```

### Step 5.4: Return to feature branch and pop stash

```bash
git checkout feature/long-work
git stash list
git stash pop
```

✅ Verification:

```bash
git status
tail -n 10 undo.txt
```

---

# PART-4: CHERRY-PICK (take one commit without merging a whole branch)

## Scenario-6: Cherry-pick a hotfix commit into main

### Step 6.1: Get the hotfix commit hash

**You must be on hotfix branch**

```bash
git checkout hotfix/urgent-fix
git log --oneline -5
```

Copy the commit hash of: `fix: urgent hotfix`

Example: `h7i8j9k fix: urgent hotfix`

### Step 6.2: Switch to main and cherry-pick

**Now go to main**

```bash
git checkout main
git pull origin main
git cherry-pick h7i8j9k
```

### Step 6.3: Push main

```bash
git push origin main
```

✅ Expected Outcome:

* main now contains the hotfix commit, without merging the whole hotfix branch history.

---

## Scenario-7: Cherry-pick conflict + resolution (must experience once)

### Step 7.1: Create conflict situation

On `main`:

```bash
git checkout main
echo "LINE: value=main" > config.txt
git add config.txt
git commit -m "chore: main config baseline"
git push origin main
```

On feature branch:

```bash
git checkout -b feature/config-change
echo "LINE: value=feature" > config.txt
git add config.txt
git commit -m "feat: update config differently"
```

Copy commit hash:

```bash
git log --oneline -3
```

Now go back to main and change same line:

```bash
git checkout main
echo "LINE: value=main-updated" > config.txt
git add config.txt
git commit -m "chore: update config on main"
```

Now cherry-pick feature commit hash:

```bash
git cherry-pick <feature-commit-hash>
```

✅ Conflict happens.

### Step 7.2: Resolve conflict

Open `config.txt`, decide final value, save.

Then:

```bash
git add config.txt
git cherry-pick --continue
```

If you want to cancel:

```bash
git cherry-pick --abort
```

---

# Submission (What students must provide)

1. Repo link
2. Proof (screenshots or terminal output) for each:

* revert done on pushed commit
* reset done on local commit (soft/mixed/hard)
* blocked checkout + fix (commit or stash)
* detached HEAD + recovery branch
* stash + pop flow
* cherry-pick success
* cherry-pick conflict + continue/abort

3. README section:
   **“How I decide between reset, revert, stash, checkout, cherry-pick”**


