# Lab 25 Assessment Answers – Build It From Requirements: Backup Script

## Answer Key

### Question 1

**Answer:** B

Automation creates consistent, repeatable backups that reduce human error and improve operational reliability.

---

### Question 2

**Answer:** C

Timestamps prevent newer backups from overwriting previous ones while also providing a historical record for recovery.

---

### Question 3

**Answer:** A

`tar -czf` creates a gzip-compressed tar archive.

- `c` = create
- `z` = gzip compression
- `f` = output filename

---

### Question 4

**Answer:** A

An exit code of `0` indicates the previous command completed successfully.

Remember that successful execution does **not** automatically mean the business requirement has been satisfied.

---

### Question 5

**Answer:** D

A backup is not considered complete until the archive has been inspected to verify it contains every required file.

Always validate the backup artifact.

---

### Question 6

**Answer:** B

`tar -tzf` lists the contents of a gzip-compressed archive without extracting it.

This is one of the safest ways to verify a backup.

---

### Question 7

**Answer:** C

The execute permission allows Linux to run the script directly.

Without execute permissions, the script exists but cannot be executed normally.

---

### Question 8

**Answer:** D

The strongest evidence combines:

- Exit code = 0
- Archive created successfully
- Archive contains every required file

Multiple pieces of evidence provide greater confidence than a single indicator.

---

### Question 9

**Answer:** B

Inventorying the source directory ensures every required file and subdirectory is included in the backup.

Missing even one required file results in an incomplete backup.

---

### Question 10

**Answer:** C

Reliable automation requires:

- Building the solution
- Verifying successful execution
- Validating that the backup artifact satisfies the business requirements

Automation without verification increases operational risk.

---

# Engineering Principles Earned

1. Automation improves consistency before speed.
2. Exit codes indicate command success—not necessarily business success.
3. Always validate backup artifacts before considering the job complete.
4. Inventory requirements before designing an automated solution.
5. Multiple forms of evidence produce higher confidence than a single successful command.
