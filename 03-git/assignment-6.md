# Assignment-6 Strategy-1 (More Detailed): Environment-Based Branching (dev → qa → ppd → prod → dr)

## Objective

Implement a real enterprise branching model where each long-lived branch represents an environment. You will practice: creating environment branches, creating feature branches from dev, promoting code dev→qa→ppd→prod→dr, creating a QA bugfix from qa and promoting it, creating a production hotfix from prod and syncing it back to all branches.

## Why This Strategy Exists (DevOps View)

* Each branch maps to an environment with different stability requirements.
* CI/CD pipelines can trigger deployments based on branch name:

  * push to dev → deploy to dev namespace
  * push to qa → deploy to QA namespace
  * push to ppd → deploy to pre-prod
  * push to prod → deploy to production
  * push to dr → deploy to DR site/cluster
* Promotions are controlled so production only receives tested code.

## Branch Definitions (Must Understand)

* dev: active integration branch for ongoing development
* qa: testing branch for QA validation
* ppd: pre-prod validation branch (UAT, performance testing, security validation)
* prod: production release branch (should be protected)
* dr: disaster recovery branch (mirror of production)

## Rules (Must Follow)

1. Never commit directly to dev/qa/ppd/prod/dr except via merges
2. Features are created only from dev
3. Bugfix branches are created from qa (if issue exists in QA only)
4. Hotfix branches are created from prod (if production is broken)
5. Promotions must be sequential: dev→qa→ppd→prod→dr
6. After a production hotfix, sync prod back to ppd, qa, dev and dr

## Files Used for Practice (To Make It Clear)

You will use these files to simulate real application changes:

* app.txt (main application changes)
* release-notes.txt (release notes per environment)
* env.txt (environment markers)

Do not rename these files.

## Pre-Check (Every Time Before You Start Any Step)

Run:

```bash
git status
git branch
```

Expected:

* Working tree clean before merges
* You know exactly which branch you are on

If not clean, stop and clean using stash or commit to a feature/bugfix/hotfix branch.

## Step-1 Create Environment Branches on Remote

Start from main baseline:

```bash
git checkout main
git pull origin main
git status
```

Expected:

* on main
* clean working tree

Create and push dev:

```bash
git checkout -b dev
echo "dev branch created" > env.txt
git add env.txt
git commit -m "chore: add env marker for dev"
git push -u origin dev
```

Create and push qa:

```bash
git checkout main
git checkout -b qa
echo "qa branch created" > env.txt
git add env.txt
git commit -m "chore: add env marker for qa"
git push -u origin qa
```

Create and push ppd:

```bash
git checkout main
git checkout -b ppd
echo "ppd branch created" > env.txt
git add env.txt
git commit -m "chore: add env marker for ppd"
git push -u origin ppd
```

Create and push prod:

```bash
git checkout main
git checkout -b prod
echo "prod branch created" > env.txt
git add env.txt
git commit -m "chore: add env marker for prod"
git push -u origin prod
```

Create and push dr:

```bash
git checkout main
git checkout -b dr
echo "dr branch created" > env.txt
git add env.txt
git commit -m "chore: add env marker for dr"
git push -u origin dr
```

Verification:

```bash
git branch -a
```

Expected:

* You see local branches: dev, qa, ppd, prod, dr
* You see remote-tracking: origin/dev, origin/qa, origin/ppd, origin/prod, origin/dr

Important explanation:

* These initial commits make each branch unique so students can visually track promotions.

## Step-2 Feature Development from dev (Simulate Developer Workflow)

Checkout dev and update:

```bash
git checkout dev
git pull origin dev
```

Create feature branch from dev:

```bash
git checkout -b feature/user-login
```

Simulate feature changes:

```bash
echo "feature: user login implemented" >> app.txt
echo "added login endpoint" >> app.txt
git add app.txt
git commit -m "feat: implement user login"
```

Add a release note for feature:

```bash
echo "feature/user-login: added login feature" >> release-notes.txt
git add release-notes.txt
git commit -m "docs: add release notes for login feature"
```

Verification (check commits are on feature branch only):

```bash
git log --oneline -5
git checkout dev
git log --oneline -5
```

Expected:

* feature branch has 2 commits
* dev does not have these commits yet

Push feature branch:

```bash
git checkout feature/user-login
git push -u origin feature/user-login
```

Merge feature into dev:

```bash
git checkout dev
git merge feature/user-login
git push origin dev
```

Verification:

```bash
git checkout dev
git log --oneline -5
```

Expected:

* dev includes the feature commits

## Step-3 Promotion dev → qa (Simulate CI/CD Promotion)

Checkout qa:

```bash
git checkout qa
git pull origin qa
```

Before merging, compare differences (students must do this):

```bash
git diff qa..dev
```

Expected:

* You can see what will be promoted

Merge dev into qa:

```bash
git merge dev
git push origin qa
```

Verify:

```bash
git log --oneline -5
```

Expected:

* qa contains feature commits

## Step-4 Promotion qa → ppd (Pre-Prod Validation Stage)

Checkout ppd:

```bash
git checkout ppd
git pull origin ppd
```

Diff check:

```bash
git diff ppd..qa
```

Merge:

```bash
git merge qa
git push origin ppd
```

Verify:

```bash
git log --oneline -5
```

## Step-5 Promotion ppd → prod (Release to Production)

Checkout prod:

```bash
git checkout prod
git pull origin prod
```

Diff check:

```bash
git diff prod..ppd
```

Merge:

```bash
git merge ppd
git push origin prod
```

Verify:

```bash
git log --oneline -5
```

## Step-6 Sync prod → dr (DR Mirror)

Checkout dr:

```bash
git checkout dr
git pull origin dr
```

Diff:

```bash
git diff dr..prod
```

Merge:

```bash
git merge prod
git push origin dr
```

Verify:

```bash
git log --oneline -5
```

## Step-7 QA Bug Fix Flow (Issue Found In QA Only)

Scenario:

* QA finds a bug in the login feature during QA testing
* The bug is not in prod yet (if it is in prod, it becomes hotfix, not bugfix)

Checkout qa:

```bash
git checkout qa
git pull origin qa
```

Create bugfix branch from qa:

```bash
git checkout -b bugfix/qa-login-validation
```

Apply fix:

```bash
echo "fix: add login validation" >> app.txt
git add app.txt
git commit -m "fix: add validation for login in qa"
```

Add QA note:

```bash
echo "bugfix: login validation added for QA" >> release-notes.txt
git add release-notes.txt
git commit -m "docs: update release notes for QA bugfix"
```

Push bugfix branch:

```bash
git push -u origin bugfix/qa-login-validation
```

Merge bugfix into qa:

```bash
git checkout qa
git merge bugfix/qa-login-validation
git push origin qa
```

Promote QA fix forward to ppd and prod:

```bash
git checkout ppd
git pull origin ppd
git merge qa
git push origin ppd

git checkout prod
git pull origin prod
git merge ppd
git push origin prod
```

Verify fix is in prod:

```bash
git checkout prod
git log --oneline -8
grep -n "validation" app.txt
```

Expected:

* login validation line exists

## Step-8 Production Hotfix Flow (Incident in Prod)

Scenario:

* Production incident occurs after release
* Must fix immediately
* Start from prod branch only

Checkout prod:

```bash
git checkout prod
git pull origin prod
```

Create hotfix branch from prod:

```bash
git checkout -b hotfix/prod-login-outage
```

Apply hotfix:

```bash
echo "hotfix: handle login outage" >> app.txt
git add app.txt
git commit -m "hotfix: mitigate login outage in prod"
```

Tag the fix (optional but recommended):

```bash
git tag -a v1.0.1 -m "Production hotfix v1.0.1"
```

Push hotfix branch and tag:

```bash
git push -u origin hotfix/prod-login-outage
git push origin v1.0.1
```

Merge hotfix into prod:

```bash
git checkout prod
git merge hotfix/prod-login-outage
git push origin prod
```

Sync prod back to other branches:

```bash
git checkout ppd
git pull origin ppd
git merge prod
git push origin ppd

git checkout qa
git pull origin qa
git merge prod
git push origin qa

git checkout dev
git pull origin dev
git merge prod
git push origin dev

git checkout dr
git pull origin dr
git merge prod
git push origin dr
```

Verify hotfix present everywhere:

```bash
for b in dev qa ppd prod dr; do
  git checkout $b
  echo "BRANCH=$b"
  git log --oneline -3
done
```

Expected:

* hotfix commit appears in each branch history

## What Students Must Explain (Write in README)

* Why environment branches exist and what each represents
* Why features come from dev, bugfix from qa, hotfix from prod
* Why promotions are sequential and why diff checks are done before merge
* Why production hotfix must be synced back to dev/qa/ppd (to prevent reintroducing the bug)
* How this maps to CI/CD pipelines

## Submission Requirements

* GitHub repo link
* Evidence (screenshots or terminal output) showing:

  1. Environment branches created and pushed
  2. Feature branch from dev created and merged to dev
  3. Promotions dev→qa→ppd→prod→dr completed
  4. Bugfix branch from qa created, merged to qa, promoted to prod
  5. Hotfix branch from prod created, merged to prod, synced back to all branches
* README explanations completed

## Success Criteria

* Correct branches exist and follow naming
* Correct branch source for feature/bugfix/hotfix
* Promotions follow the exact sequence
* Student demonstrates verification at each step


# Assignment-6 Strategy-2: QA–PROD Branching with Release Candidates (RC)

## Objective

Implement a **release-candidate–based branching strategy** used by SaaS and product companies where:

* only **two long-living branches** exist (`qa` and `prod`)
* releases are stabilized through **multiple RC branches**
* QA cycles repeat until a stable RC is promoted to production

---

## Why This Strategy Exists

This strategy is used when:

* teams release frequently
* QA feedback loops are fast
* production must stay extremely stable
* teams want to avoid too many permanent branches

Key idea:

* Development happens on `qa`
* Production is protected on `prod`
* Release candidates act as **temporary stabilization branches**

---

## Branch Names Used (Strict)

Permanent branches
`qa`
`prod`

Temporary branches
`feature/*`
`bugfix/*`
`rc-1` `rc-2` `rc-3`
`hotfix/*`

---

## Strategy-2 Algorithm (High Level)

1. All development and integration happens on `qa`
2. Features are created from `qa`
3. When release is planned, create `rc-1` from `qa`
4. QA tests `rc-1`
5. If bugs are found, fixes are done in `qa`
6. Create next RC (`rc-2`) from updated `qa`
7. Repeat until RC is stable
8. Final stable RC is merged into `prod`
9. Production hotfixes start from `prod` and sync back to `qa`

---

## Pre-Check (Mandatory Before Starting)

```bash
git status
git branch
```

Expected:

* You know exactly which branch you are on
* Working tree is clean before merges

---

## Step-1 Create Base Branches

Start from `main`:

```bash
git checkout main
git pull origin main
```

Create `qa`:

```bash
git checkout -b qa
git push -u origin qa
```

Create `prod`:

```bash
git checkout main
git checkout -b prod
git push -u origin prod
```

Verify:

```bash
git branch -a
```

Expected:

* `qa` and `prod` exist locally and remotely

---

## Step-2 Feature Development Flow (From QA)

All features start from `qa`.

```bash
git checkout qa
git pull origin qa
git checkout -b feature/payment-api
```

Implement feature:

```bash
echo "payment api implemented" >> app.txt
git add app.txt
git commit -m "feat: add payment api"
```

Push feature branch:

```bash
git push -u origin feature/payment-api
```

Merge feature into `qa`:

```bash
git checkout qa
git merge feature/payment-api
git push origin qa
```

Verify:

```bash
git log --oneline -5
```

Expected:

* Feature commit exists on `qa`
* `prod` is unchanged

---

## Step-3 Create Release Candidate 1 (RC-1)

Release is planned.

Create RC-1 from `qa`:

```bash
git checkout qa
git pull origin qa
git checkout -b rc-1
git push -u origin rc-1
```

Verification:

```bash
git log --oneline -5
```

Expected:

* `rc-1` snapshot matches `qa` at release time

---

## Step-4 QA Testing on RC-1 (Bug Found)

Scenario:

* QA tests `rc-1`
* Bug is found
* **Do NOT fix directly on rc-1**

Why:

* RC branches are disposable
* Fixes must go back to `qa`

---

## Step-5 Bug Fix Flow (From QA)

Checkout `qa`:

```bash
git checkout qa
git pull origin qa
git checkout -b bugfix/payment-timeout
```

Apply fix:

```bash
echo "fix payment timeout issue" >> app.txt
git add app.txt
git commit -m "fix: resolve payment timeout issue"
```

Push bugfix branch:

```bash
git push -u origin bugfix/payment-timeout
```

Merge bugfix into `qa`:

```bash
git checkout qa
git merge bugfix/payment-timeout
git push origin qa
```

Verify:

```bash
git log --oneline -5
```

Expected:

* Bug fix exists on `qa`
* `rc-1` is now outdated and ignored

---

## Step-6 Create Release Candidate 2 (RC-2)

Create new RC from updated `qa`:

```bash
git checkout qa
git pull origin qa
git checkout -b rc-2
git push -u origin rc-2
```

QA tests `rc-2`.

If bug still exists → repeat bugfix process and create `rc-3`.

---

## Step-7 Final Stable Release Candidate

Assume `rc-3` is stable.

Verify RC branch:

```bash
git checkout rc-3
git log --oneline -5
```

Expected:

* All required fixes present
* No unstable changes

---

## Step-8 Promote Stable RC to Production

Checkout `prod`:

```bash
git checkout prod
git pull origin prod
```

Merge final RC:

```bash
git merge rc-3
git push origin prod
```

Verification:

```bash
git log --oneline -5
```

Expected:

* Production now contains the stable release

---

## Step-9 Production Hotfix Flow

Scenario:

* Issue found in production after release

Hotfix must start from `prod`.

```bash
git checkout prod
git pull origin prod
git checkout -b hotfix/prod-payment-outage
```

Apply fix:

```bash
echo "critical prod hotfix" >> app.txt
git add app.txt
git commit -m "hotfix: resolve prod payment outage"
```

Push hotfix branch:

```bash
git push -u origin hotfix/prod-payment-outage
```

Merge hotfix into `prod`:

```bash
git checkout prod
git merge hotfix/prod-payment-outage
git push origin prod
```

---

## Step-10 Sync Production Hotfix Back to QA

This prevents the bug from reappearing in next release.

```bash
git checkout qa
git pull origin qa
git merge prod
git push origin qa
```

Verification:

```bash
git log --oneline -5
```

Expected:

* Hotfix commit exists on both `prod` and `qa`

---

## What Students Must Explain (README)

* Why only `qa` and `prod` are permanent
* Why RC branches are temporary
* Why fixes are never done directly on RC branches
* Difference between:

  * feature branch
  * bugfix branch
  * release candidate branch
  * hotfix branch
* When Strategy-2 is preferred over Strategy-1

---

## Submission Requirements

* GitHub repo link
* Proof of:

  * `qa` and `prod` branches
  * feature branch from `qa`
  * rc-1, rc-2, rc-3 creation
  * bugfix from `qa`
  * rc promotion to `prod`
  * prod hotfix and sync back to `qa`
* README explanations completed

---

## Success Criteria

* Correct branch sources used
* RC flow followed exactly
* No fixes directly on RC branches
* Production hotfix synced back to QA
* Student can clearly explain the strategy verbally


