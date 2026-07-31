# Lab 22 Assessment Answers – Archives & Backup

## Answer Key

### Question 1

**Answer:** B

`tar` bundles multiple files and directories into a single portable archive while preserving the directory structure, permissions, and ownership.

---

### Question 2

**Answer:** C

Inspecting an archive before extraction allows engineers to verify its contents before writing files onto a production system, reducing operational risk.

---

### Question 3

**Answer:** C

The `-C` option changes into the specified directory before archiving so the archive stores clean relative paths instead of unnecessary parent directories.

---

### Question 4

**Answer:** A

`gzip` provides the best balance of compression speed and file size, making it the preferred choice for routine Linux backups.

---

### Question 5

**Answer:** B

`xz` is chosen when achieving the smallest archive size is more important than the additional CPU time required to compress and decompress the data.

---

### Question 6

**Answer:** B

`rsync` compares the source and destination and transfers only changed files, reducing bandwidth, execution time, and storage operations.

---

### Question 7

**Answer:** B

Archive mode preserves permissions, ownership, timestamps, symbolic links, recursive directory structures, and other important file metadata.

---

### Question 8

**Answer:** A

A trailing slash copies the **contents** of the directory rather than creating another copy of the directory itself inside the destination.

---

### Question 9

**Answer:** A

`--delete` removes files from the destination that no longer exist in the source, creating an exact mirror of the source directory.

---

### Question 10

**Answer:** D

The success of a backup strategy is measured by the ability to restore accurate data within the required recovery objectives—not simply by creating an archive.

---

### Question 11

**Answer:** C

For frequently executed backups such as hourly log archives, `gzip` provides the best balance between execution speed and compression efficiency.

---

### Question 12

**Answer:** C

Always inspect an unfamiliar archive with `tar -tzf` before extracting it to verify its contents and reduce the risk of introducing unexpected files into the system.

---

# Overall Principles

1. A backup is only successful if it can be restored.
2. Archive creation and compression are separate engineering responsibilities.
3. Inspect archives before extraction.
4. Choose compression based on business requirements rather than maximum compression.
5. `rsync` synchronizes changes; `tar` creates point-in-time snapshots.
6. Validate the execution environment before assuming required utilities are installed.
7. Verify every backup with evidence rather than assumptions.
