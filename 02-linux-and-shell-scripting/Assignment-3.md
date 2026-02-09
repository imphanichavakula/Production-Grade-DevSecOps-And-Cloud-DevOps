# Assignment – Group 3

## Process Management Deep Dive (Job Control, Signals, Priority, systemd, Troubleshooting)

---

## Objective

By the end of this module, you will be able to:

* Explain **program vs process**
* Control jobs in the shell: **foreground/background**, **stop/resume**
* Use `jobs`, `bg`, `fg`, **`%jobid`**, `disown`, `nohup`
* Understand and use **signals** (`SIGTERM`, `SIGKILL`, `SIGSTOP`, `SIGCONT`, `SIGHUP`, `SIGINT`)
* Find PIDs and inspect processes (`ps`, `top`, `pgrep`, `/proc`)
* Manage CPU priority (`nice`, `renice`)
* Use service manager (`systemctl`) and logs (`journalctl`)
* Debug real issues: **high CPU**, **stuck process**, **zombie**, **permission denied kill**

---

## Prerequisites

* Ubuntu VM/EC2
* Use a terminal session (SSH)

Verify:

```bash
# Confirms current user
whoami

# Confirms sudo permissions
sudo -v
```

---

## Task 1: Program vs Process (Foundation)

### Goal

Understand what a process is and how to identify it.

```bash
# Start a process that runs for 600 seconds
sleep 600
```

👉 This creates a **process** (running instance of `sleep`).

Open a second terminal (or background it later) and run:

```bash
# Show running processes and filter for sleep
ps aux | grep sleep
```

👉 Output contains **PID** (process ID). PID is what we target for signals.

Stop the foreground process:

```text
Ctrl + C
```

👉 Sends **SIGINT** (interrupt) to terminate the foreground process.

---

## Task 2: Job Control Basics (Foreground vs Background)

### Goal

Understand **jobs** (shell-managed) vs **processes** (OS-managed).

```bash
# Run in background: terminal is free, process continues
sleep 300 &
```

👉 `&` means run in **background**.

```bash
# List current jobs of THIS shell only
jobs
```

👉 Shows something like:

* `[1]  Running  sleep 300 &`

**Important concept**

* `jobs` shows **shell jobs**
* `ps` shows **system processes**

---

## Task 3: Stop and Resume Jobs (Ctrl+Z, bg, fg, %jobid)

### Goal

Learn correct stop/resume syntax, including **job IDs**.

```bash
# Start a foreground job
sleep 1000
```

Now press:

```text
Ctrl + Z
```

👉 Sends **SIGTSTP** (terminal stop) and **stops** the job (does not kill it).

```bash
# List jobs to see job ID
jobs
```

👉 Example output:

* `[1]+  Stopped   sleep 1000`

### Resume a specific job in background

```bash
# Resume job 1 in background
bg %1
```

👉 `bg` needs a job reference: `%1` means job id 1.

### Bring a specific job to foreground

```bash
# Bring job 1 to foreground
fg %1
```

### If you have multiple jobs

```bash
# Start multiple background jobs
sleep 200 &
sleep 300 &
sleep 400 &

# Show them
jobs
```

Now control exact ones:

```bash
# Stop the job currently in foreground (if any): Ctrl+Z
# Resume job 2 in background
bg %2

# Bring job 3 to foreground
fg %3
```

---

## Task 4: Kill vs Stop (Big Interview Trap)

### Goal

Understand the difference between **pausing** and **terminating**.

* Stop (pause): process still exists in memory, state becomes stopped
* Kill (terminate): process exits, PID disappears

Create a job:

```bash
sleep 999 &
jobs
```

Kill by job id (shell job):

```bash
# Terminate job 1 (sends SIGTERM to that job)
kill %1
```

Verify:

```bash
jobs
```

---

## Task 5: Signals Deep Dive (kill -SIGNAL PID)

### Goal

Use correct signals, and know when to use which.

List signals:

```bash
kill -l
```

### Most important signals

* `SIGTERM (15)` = polite “please stop”
* `SIGKILL (9)` = force stop (cannot be ignored)
* `SIGSTOP` = hard pause (cannot be ignored)
* `SIGCONT` = resume a stopped process
* `SIGHUP` = hangup (often used when terminal closes)
* `SIGINT` = Ctrl+C interrupt
* `SIGTSTP` = Ctrl+Z stop (terminal stop)

Start a process:

```bash
sleep 1000 &
pgrep -a sleep
```

👉 `pgrep -a` shows PID + command.

Assume PID is `12345`.

Graceful stop:

```bash
# Ask the process to terminate gracefully
kill -TERM 12345
```

Force kill:

```bash
# Force kill if SIGTERM doesn’t work
kill -KILL 12345
# same as
kill -9 12345
```

Pause and resume:

```bash
# Pause the process
kill -STOP 12345

# Resume the process
kill -CONT 12345
```

---

## Task 6: Process Discovery (ps, pgrep, pidof)

### Goal

Find PIDs reliably.

```bash
# Full process list
ps aux | head
```

```bash
# Find PID(s) of nginx (if installed)
pgrep nginx
```

```bash
# Show PID + full command line
pgrep -a sshd
```

```bash
# Another way to get PID by name
pidof sshd
```

---

## Task 7: Process Inspection (state, PPID, threads)

### Goal

Read process state and parent-child relationships.

```bash
# Show PID, PPID, state, and command for sleep processes
ps -o pid,ppid,state,tty,cmd -C sleep
```

States:

* `R` running
* `S` sleeping
* `T` stopped
* `Z` zombie

Process tree:

```bash
# Show process tree (parent-child)
pstree -p | head -n 30
```

---

## Task 8: Foreground/Background Pitfalls (Terminal Close, SIGHUP)

### Goal

Understand what happens when SSH session closes.

### `nohup` (survive terminal close)

```bash
# Run command immune to hangup and continue after logout
nohup sleep 500 > nohup.out 2>&1 &
```

👉 Output goes to `nohup.out`.

### `disown` (detach job from shell)

```bash
sleep 600 &
jobs

# Detach job 1 so it’s not tied to this shell
disown %1
```

Verify it won’t show in jobs anymore:

```bash
jobs
```

---

## Task 9: CPU and Memory Troubleshooting (production style)

### Goal

Create load, observe, then fix safely.

### CPU load

```bash
# Create CPU-heavy process
yes > /dev/null &
```

Observe:

```bash
top
```

👉 Find `yes` using high CPU.

Stop it:

```bash
# Kill by name
pkill yes
```

### Memory view (quick)

```bash
free -m
```

---

## Task 10: Priority Control (nice, renice)

### Goal

Understand why some processes get more CPU time than others.

Start process with low priority:

```bash
# Start with nice value 10 (lower priority than default)
nice -n 10 sleep 1000 &
```

Check priority:

```bash
# Show PID and nice value
ps -o pid,ni,cmd -C sleep
```

Change priority of a running process:

```bash
# Renice a process to lower priority (example PID 12345)
sudo renice +15 -p 12345
```

👉 Higher nice number = **lower CPU priority**.

---

## Task 11: Services vs Processes (systemd reality)

### Goal

Understand “service ≠ process” and how systemd manages processes.

```bash
# Check SSH service status
systemctl status ssh
```

```bash
# Restart ssh service (service manager spawns processes)
sudo systemctl restart ssh
```

View logs:

```bash
# View logs for ssh service
journalctl -u ssh --no-pager | tail -n 50
```

---

## Task 12: When kill doesn’t work (permissions + root)

### Goal

Understand why you sometimes can’t kill a process.

Create a process as root:

```bash
# Start process as root
sudo sleep 999 &
```

Try killing it as ubuntu:

```bash
# Find PID
pgrep -a sleep

# Try kill (may fail if not owned)
kill <PID>
```

Fix:

```bash
# Kill as root
sudo kill <PID>
```

---

## Task 13: Zombie Process (concept + identification)

You’re **100% right** 👍
That `awk` pipeline is **too complex** for teaching at this stage and distracts from the *concept* of zombies.

Let’s replace it with **simple, readable, beginner-friendly commands** and put it in **proper assignment format**.

Below is a **clean rewrite of the Zombie Process task**, using **easy commands only**.

---

## Task X: Identifying Zombie Processes (Simple & Clear)

### Goal

Identify zombie processes using **simple Linux commands** and understand what they mean.

---

### What is a Zombie Process? (1-line, student friendly)

> A **zombie process** is a process that has finished execution but whose **parent has not cleaned it up yet**.

---

### Step 1: View Process State Using `ps`

```bash
# Show PID, PPID, state, and command for all processes
ps -eo pid,ppid,state,cmd | head
```

👉 This shows:

* `PID`  → process ID
* `PPID` → parent process ID
* `STATE` → current process state

---

### Step 2: Look for Zombie State (`Z`)

```bash
# Show only processes with state Z (Zombie)
ps -eo pid,ppid,state,cmd | grep ' Z '
```

👉 Explanation:

* `ps -eo` → custom output format
* `state` column shows process state
* `Z` means **Zombie**
* `grep ' Z '` keeps it readable (no awk)

---

### Step 3: If No Zombies Are Found

```bash
# No output means there are no zombie processes (this is normal)
```

👉 Most healthy systems **do not have zombies**
👉 That is **expected and good**

---

### Step 4: Understand the Output (Example)

Example output:

```text
12345  6789  Z  [some-process]
```

What this means:

* `12345` → zombie process PID
* `6789` → parent PID
* `Z` → zombie state
* Command is wrapped in `[]` (common for zombies)

---

### Important Rule 

❌ **You cannot kill a zombie process**

Why?

* Zombie is already dead
* It only exists as a record

✅ Fix is always:

* Restart or fix the **parent process**
* Or restart the service/system if needed

---

### Validation

* Student ran `ps -eo pid,ppid,state,cmd`
* Student understands:

  * What `Z` means
  * Why zombies cannot be killed
  * That zombies are cleaned by parent processes

