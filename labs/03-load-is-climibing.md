# Lab 03 – Investigating and Terminating a Runaway Process

## Business Problem

A production Linux server is experiencing excessive disk activity because an unknown background process continuously writes to a log file. The objective is to identify the offending process, terminate only that workload, and verify that normal system behavior is restored without rebooting the server.

---

# Why This Matters

Production systems often host dozens or even hundreds of processes simultaneously. Rebooting an entire server to solve one misbehaving process introduces unnecessary downtime and risk.

Engineers are expected to isolate the problem, collect evidence, remediate only the affected workload, and verify that the system has returned to a healthy state.

The goal is not simply to stop a process—it is to restore business operations with the least possible disruption.

---

# Core Concepts

* Process
* Process ID (PID)
* Parent Process
* Child Process
* Signals
* SIGTERM
* Process Enumeration
* Process Verification
* Root Cause Analysis
* Evidence-Based Troubleshooting

---

# Mental Model

```
Business reports a problem
            │
            ▼
Observe the symptom
            │
            ▼
Collect evidence
            │
            ▼
Identify the responsible process
            │
            ▼
Understand why it exists
            │
            ▼
Terminate only the offending workload
            │
            ▼
Verify the issue is resolved
            │
            ▼
Confirm system stability
```

---

# Engineering Investigation

## Engineering Question 1

### What evidence suggests the system is still unhealthy?

### Objective

Determine whether the reported log file is actively growing.

### Command

```bash
wc -l /var/log/runaway/out.log
```

### Why This Matters

Before making changes, establish a measurable baseline. Engineers need evidence that a problem exists before attempting remediation.

### Evidence Observed

```
181 /var/log/runaway/out.log
```

### Engineering Takeaway

A single measurement establishes the current size of the log file but does not prove continuous growth.

---

## Engineering Question 2

### Is the log actively growing?

### Command

```bash
tail -f /var/log/runaway/out.log
```

### Why This Matters

Unlike `cat`, which displays historical data, `tail -f` observes activity in real time.

### Evidence Observed

Repeated timestamped entries appeared approximately every second.

### Engineering Takeaway

The log file was actively receiving new entries, confirming the incident was ongoing.

---

## Engineering Question 3

### Which process is responsible?

### Command

```bash
ps aux --sort=-%cpu | head
```

### Why This Matters

Enumerating running processes allows engineers to correlate system activity with executing workloads.

### Evidence Observed

```
37 runuser -u student -- /bin/bash /opt/stress/hog.sh
39 /bin/bash /opt/stress/hog.sh
```

### Engineering Takeaway

Two related processes were discovered:

* PID 37 launched the workload.
* PID 39 executed the script.

---

## Engineering Question 4

### Which command launched the workload?

### Command

```bash
pgrep -af hog.sh
```

### Why This Matters

Searching by command line identifies the exact workload rather than relying only on a PID.

### Evidence Observed

```
37 runuser -u student -- /bin/bash /opt/stress/hog.sh
39 /bin/bash /opt/stress/hog.sh
```

### Engineering Takeaway

The investigation confirmed that `hog.sh` was the application generating the log activity.

---

## Engineering Question 5

### Why does this process never stop?

### Command

```bash
cat /opt/stress/hog.sh
```

### Why This Matters

Understanding root cause is more valuable than simply identifying the process.

### Evidence Observed

```bash
while true; do
    echo "$(date) processing batch" >> /var/log/runaway/out.log
    sleep 1
done
```

### Engineering Takeaway

The script contains an infinite loop (`while true`) that continuously appends log entries every second. Because it sleeps between writes, it produces minimal CPU usage while continuously generating disk activity.

---

## Engineering Question 6

### How do we stop only the offending workload?

### Command

```bash
pkill -f hog.sh
```

### Why This Matters

Targeted remediation minimizes disruption to the rest of the operating system.

### Evidence Observed

```
pkill: killing pid 37 failed: Operation not permitted
```

### Engineering Takeaway

The command attempted to signal both the root-owned launcher and the student-owned script. Permission was denied for the root-owned process, but the actual runaway script terminated successfully.

---

## Engineering Question 7

### Did the remediation work?

### Commands

```bash
wc -l /var/log/runaway/out.log
sleep 2
wc -l /var/log/runaway/out.log
```

```bash
pgrep -af hog.sh
```

### Why This Matters

Successful remediation must be verified with evidence rather than assumption.

### Evidence Observed

```
849
849
```

```
(no output)
```

### Engineering Takeaway

The log file stopped growing, and no running process matched `hog.sh`. The incident was successfully resolved.

---

# Engineering Lessons Learned

* Evidence should always precede remediation.
* A process causing operational issues is not always the highest CPU consumer.
* Parent and child processes serve different roles during execution.
* Infinite loops can create operational issues without consuming significant CPU.
* Verification is the final step of every engineering change.

---

# Engineering Principles Earned

1. Observe before acting.
2. Establish measurable baselines.
3. Follow evidence instead of assumptions.
4. Identify root cause before remediation.
5. Remediate with the smallest possible scope.
6. Verify every operational change.
7. Separate symptoms from causes.
8. Commands gather evidence; engineers interpret it.
9. Process management is fundamental to Linux operations.
10. Reliable systems are maintained through disciplined investigation.

---

# Reflection

Today reinforced that Linux troubleshooting is not about memorizing commands—it is about asking increasingly better engineering questions.

Every command collected another piece of evidence until the system itself revealed the root cause. The solution was not found by guessing; it was discovered through observation, interpretation, targeted action, and verification.

The most important lesson is that engineers do not prove they are correct by fixing a problem. They prove they are correct by collecting evidence before and after the change.

