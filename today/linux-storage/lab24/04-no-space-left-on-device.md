# Lab 24 – No Space Left on Device

## Objective

Diagnose and recover from a disk-full incident by following an evidence-based investigation process instead of making assumptions.

---

# Business Problem

A production application reports:

"No space left on device."

When a filesystem reaches 100% utilization, applications, databases, and operating system services may fail because they can no longer write data.

The engineer's responsibility is to restore business capability while minimizing operational risk.

---

# Core Architecture

```
Alert
    │
    ▼
Filesystem Full
    │
    ▼
df -h
Identify Filesystem
    │
    ▼
du -sh
Identify Largest Directory
    │
    ▼
Investigate Largest File
    │
    ▼
Verify File Purpose
    │
    ▼
Corrective Action
    │
    ▼
Validate Recovery
```

---

# Engineering Investigation Workflow

1. Receive storage alert.
2. Identify the affected filesystem.
3. Locate the largest directory.
4. Locate the largest file.
5. Verify the file before taking action.
6. Recover storage.
7. Verify business recovery.

---

# Engineering Distinctions

## df vs du

| Command | Question Answered |
|----------|-------------------|
| df -h | Which filesystem is full? |
| du -sh | Which directory or file is consuming space? |

Never use `du` before identifying the affected filesystem.

---

## File Names vs File Contents

A filename suggests intent.

A file's contents determine what it actually is.

Example:

```
huge.log
```

looked like a text log.

Evidence showed:

```
file huge.log

data
```

The extension alone is never proof.

---

## Investigation Evidence

Filesystem identified:

```
/dev/loop2
```

Mounted at:

```
/mnt/data
```

Initial utilization:

```
100%
```

Largest file discovered:

```
huge.log
50 MB
```

File classification:

```
data
```

Storage after recovery:

```
1%
```

---

# Commands to Understand

```bash
df -h
```

Displays filesystem usage.

Answers:

> Which filesystem is full?

---

```bash
du -sh <directory>
```

Measures directory size.

Answers:

> Which directory is consuming storage?

---

```bash
ls -lh
```

Displays file sizes in human-readable format.

Useful for quickly identifying large files.

---

```bash
file <filename>
```

Identifies the actual file type based on its contents rather than its extension.

---

```bash
tail
```

Displays the end of a text file.

Useful for inspecting recent log activity.

---

```bash
rm
```

Removes a file.

Only perform after verifying the file can be safely removed.

---

# Engineering Principles

## Principle 1

Evidence always drives the investigation.

---

## Principle 2

The largest file is a suspect—not automatically the culprit.

---

## Principle 3

Never delete data until you understand its business purpose.

---

## Principle 4

Verification occurs before and after every corrective action.

---

## Principle 5

Success is measured by restoring business capability—not by successfully deleting a file.

---

# Consulting Perspective

Customers do not care that disk utilization reached 100%.

They care that their application continues operating.

The engineer's responsibility is to restore service while protecting business data.

---

# Vocabulary

### Filesystem

A mounted storage area that organizes and manages files.

---

### Mount Point

The directory where a filesystem becomes accessible.

---

### Disk Utilization

The percentage of storage currently in use.

---

### Human Readable

Displaying storage sizes using KB, MB, GB, and TB instead of raw bytes.

---

### Investigation

A structured process of gathering evidence before taking corrective action.

---

### Corrective Action

A change made to resolve the identified root cause.

---

### Validation

Confirming that the corrective action restored the desired business outcome.

---

### Root Cause

The underlying reason an incident occurred.

---

### Business Recovery

Restoring the customer's ability to use the application successfully.
