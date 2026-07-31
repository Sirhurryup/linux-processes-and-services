# Lab 22 Assessment – Archives & Backup

## Instructions

Choose the BEST answer without using your notes.

---

### Question 1

What business problem does `tar` primarily solve?

A. Encrypts files for secure transfer.

B. Bundles multiple files and directories into a single portable archive while preserving their structure.

C. Synchronizes two servers.

D. Compresses files without creating an archive.

---

### Question 2

Why should an engineer inspect an archive before extracting it on a production server?

A. To reduce compression time.

B. To determine which compression algorithm was used.

C. To verify the archive's contents before allowing files to be written onto the system.

D. To increase extraction speed.

---

### Question 3

What is the purpose of the `-C /tmp` option in the command below?

```bash
tar -czf backup.tar.gz -C /tmp myproject
```

A. Compress the archive.

B. Create the archive in `/tmp`.

C. Change into `/tmp` before archiving so cleaner relative paths are stored.

D. Copy the archive to `/tmp`.

---

### Question 4

Which compression method is generally the best choice for routine Linux backups?

A. gzip

B. bzip2

C. xz

D. zip

---

### Question 5

When would an engineer intentionally choose **xz** instead of **gzip**?

A. When compression speed is the highest priority.

B. When maximum compression is more important than processing time.

C. When synchronizing two directories.

D. When extracting archives.

---

### Question 6

What is the primary advantage of `rsync` over repeatedly creating full archives?

A. It automatically encrypts backups.

B. It transfers only changed files, reducing bandwidth and execution time.

C. It creates smaller archives than gzip.

D. It replaces tar.

---

### Question 7

What does the `-a` (archive mode) option preserve?

A. Only directory names.

B. Permissions, ownership, timestamps, symbolic links, and recursive copying.

C. Compression ratios.

D. Network bandwidth.

---

### Question 8

Why is the trailing slash important in this command?

```bash
rsync -av /tmp/myproject/ /tmp/mybackup/
```

A. It copies only the **contents** of `myproject`.

B. It enables compression.

C. It deletes extra files.

D. It forces archive mode.

---

### Question 9

When is the `--delete` option appropriate?

A. When the destination should become an exact mirror of the source.

B. When archives should be compressed faster.

C. When restoring deleted files.

D. When creating incremental backups.

---

### Question 10

What is the best measure of a successful backup strategy?

A. The archive file is large.

B. Compression completed successfully.

C. Backups run every night.

D. Data can be successfully restored with integrity and acceptable recovery time.

---

### Question 11

Your backup server stores hourly application logs.

Which compressor best balances speed and operational efficiency?

A. xz

B. bzip2

C. gzip

D. No compression

---

### Question 12

A production server receives an unfamiliar archive named:

```text
deploy-20260731.tar.gz
```

What should you do FIRST?

A. Extract it into `/`.

B. Delete the archive.

C. List its contents with `tar -tzf`.

D. Rename the archive.
