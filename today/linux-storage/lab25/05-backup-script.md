# Lab 25 – Build It From Requirements: Backup Script

## Objective

Automate the backup of an application data directory by creating a reusable Bash script that produces a valid, timestamped gzip-compressed tar archive.

---

# Business Problem

Manual backups are inconsistent and error-prone. The goal is to automate the backup process so Operations can reliably archive application data without depending on manual execution.

---

# Engineering Workflow

Requirements
↓
Inventory Source Data
↓
Design Script
↓
Create Timestamped Archive
↓
Verify Exit Code
↓
Validate Archive Contents
↓
Grade

---

# Commands to Understand

## ls -R /srv/data

Recursively inventories the complete directory tree before building the backup.

---

## tar -czf

Creates a gzip-compressed tar archive.

- c = create
- z = gzip compression
- f = output filename

---

## chmod +x backup.sh

Makes the script executable.

---

## echo $?

Displays the exit status of the previously executed command.

0 = Success

Non-zero = Failure

---

## tar -tzf archive.tar.gz

Lists the contents of a compressed archive without extracting it.

Useful for validating backups.

---

# Engineering Distinctions

A successful script execution does not guarantee a successful backup.

Always verify:

- Exit status
- Archive creation
- Archive contents

---

# Evidence Collected

✓ Inventory verified

✓ Backup script created

✓ Script executable

✓ Exit code returned 0

✓ Timestamped archive created

✓ Archive contained:

- index.html
- config.yml
- data/records.csv

✓ Lab passed

---

# Engineering Principles

- Automation improves consistency before speed.
- Verify artifacts, not just command execution.
- Build reusable operational processes.
- Timestamp backups to prevent accidental overwrites.

---

# Consulting Perspective

Reliable backups reduce operational risk by producing repeatable recovery artifacts that can be validated before they are needed.

---

# Vocabulary

### Backup Script

A reusable program that automates the creation of backup archives.

### Exit Code

The numeric status returned by a process indicating success or failure.

### Archive

A single file containing one or more files for backup or distribution.

### Timestamp

Date and time embedded in a filename to uniquely identify backup versions.

### Artifact

The final backup file produced by the script.
