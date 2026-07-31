# Lab 22 – Archives & Backup

## Objective

Protect application data by creating archives, compressing them efficiently, restoring them safely, and understanding incremental synchronization.

---

# Business Problem

Production systems require reliable backup and recovery. A backup is only valuable if engineers can verify, restore, and trust the data during an outage.

---

# Core Architecture

```
Application Data
        │
        ▼
Archive (tar)
        │
        ▼
Compression
(gzip | bzip2 | xz)
        │
        ▼
Archive File
(.tar.gz, .tar.bz2, .tar.xz)
        │
        ▼
Verification
(tar -tzf)
        │
        ▼
Restore
(tar -xzf)
```

---

# tar vs rsync

| tar | rsync |
|------|--------|
| Creates point-in-time snapshots | Synchronizes directories |
| Bundles files into one archive | Copies only changed files |
| Excellent for backups | Excellent for continuous synchronization |
| Restore from archive | Keep backup continuously updated |

---

# Compression Comparison

| Compressor | Speed | Compression | Typical Use |
|------------|-------|-------------|-------------|
| gzip | Fastest | Moderate | Log rotation, routine backups |
| bzip2 | Moderate | Better | Archives where additional compression is worthwhile |
| xz | Slowest | Best | Long-term archives, package distribution |

---

# Engineering Workflow

1. Create application data.
2. Verify source files.
3. Create archive.
4. Verify archive exists.
5. Inspect archive before extraction.
6. Restore into a separate location.
7. Verify restored files.
8. Compare compression strategies.
9. Synchronize using rsync.
10. Mirror using rsync --delete.

---

## Command Options (Flags) to Understand

### tar -czf

Creates a gzip-compressed archive.

- **c** = Create a new archive.
- **z** = Compress the archive using **gzip**.
- **f** = Write the archive to the specified filename.

Example:

```bash
tar -czf backup.tar.gz myproject/
```

Creates a gzip-compressed archive named `backup.tar.gz`.

---

### tar -cjf

Creates a bzip2-compressed archive.

- **c** = Create a new archive.
- **j** = Compress using **bzip2**.
- **f** = Write the archive to the specified filename.

Example:

```bash
tar -cjf backup.tar.bz2 myproject/
```

Produces a `.tar.bz2` archive with a higher compression ratio than gzip, but generally requires more processing time.

---

### tar -cJf

Creates an xz-compressed archive.

- **c** = Create a new archive.
- **J** = Compress using **XZ**.
- **f** = Write the archive to the specified filename.

Example:

```bash
tar -cJf backup.tar.xz myproject/
```

Produces the smallest archive in many cases but is typically the slowest compression method. (The XZ utility was not installed in this lab.)

---

### tar -tzf

Lists the contents of a gzip-compressed archive without extracting it.

- **t** = Display the table of contents.
- **z** = Read a gzip-compressed archive.
- **f** = Read the specified archive file.

Example:

```bash
tar -tzf backup.tar.gz
```

Useful for verifying a backup before performing a restore.

---

### tar -xzf

Extracts a gzip-compressed archive.

- **x** = Extract files.
- **z** = Read a gzip-compressed archive.
- **f** = Read the specified archive file.

Example:

```bash
tar -xzf backup.tar.gz
```

Restores the archived files into the current directory.

---

### tar -C

Temporarily changes to another directory before performing the requested operation.

Example:

```bash
tar -czf backup.tar.gz -C /tmp myproject
```

Instead of storing:

```
/tmp/myproject/
```

the archive stores:

```
myproject/
```

This creates cleaner, more portable archives.

---

# Engineering Memory Tip

The **first letter** tells **what** you want to do:

- **c** = Create
- **t** = Table of contents (List)
- **x** = Extract

The **middle letter(s)** describe **how** the archive is compressed:

- **z** = Gzip (`.gz`)
- **j** = Bzip2 (`.bz2`)
- **J** = XZ (`.xz`)

The **last letter** tells Linux that the **next argument is the filename**:

- **f** = File

Think of it as:

```
Action
↓

Compression
↓

Filename
```

Examples:

```text
tar -czf
Create → Gzip → File

tar -cjf
Create → Bzip2 → File

tar -cJf
Create → XZ → File

tar -tzf
List → Gzip → File

tar -xzf
Extract → Gzip → File
```

# Evidence Collected

- Created a realistic application directory.
- Archived the application successfully.
- Verified archive creation.
- Listed archive contents without extraction.
- Restored to a new directory.
- Verified directory integrity after restoration.
- Compared gzip and bzip2 archive sizes.
- Identified missing xz dependency in the lab environment.
- Identified missing rsync package in the lab environment.

---

# Engineering Principles

## Principle 1

Archives should always be verified before extraction.

---

## Principle 2

Bundling files and compressing files are separate engineering responsibilities.

---

## Principle 3

Choose compression based on business requirements rather than maximum compression.

---

## Principle 4

Incremental synchronization is more efficient than repeatedly copying unchanged data.

---

## Principle 5

Always validate the execution environment before assuming required utilities are available.

---

# Consulting Perspective

A backup strategy is measured by successful restoration, not successful archive creation.

Customers care about recovery time and data integrity—not the compression algorithm used behind the scenes.

---

# Commands to Remember

```bash
find

tar -czf
tar -tzf
tar -xzf

ls -lh

rsync -av
rsync -av --delete
```

---

# Vocabulary

### Archive

A single file that bundles one or more files and directories together for storage, backup, or transfer.

---

### Tar (Tape Archive)

A Linux utility used to create, list, extract, and manage archive files while preserving directory structure and file permissions.

---

### Compression

The process of reducing a file's size to save storage space and decrease transfer time.

---

### Gzip

A compression utility that reduces file size using the DEFLATE compression algorithm. Commonly used with `tar` to create `.tar.gz` archives.

---

### Bzip2

A compression utility that generally achieves higher compression ratios than gzip but requires more processing time.

---

### XZ

A high-compression utility that typically produces the smallest archives but requires the greatest amount of CPU time. (In this lab, the required `xz` utility was not installed.)

---

### Recursive Backup

A backup that includes every file and subdirectory beneath the specified source directory.

---

### Restore

The process of extracting archived files back into a usable directory structure.

---

### Working Directory

The current directory from which a command executes. Commands like `tar -C` temporarily change the working directory before processing files.

---

### Relative Path

A file or directory location interpreted from the current working directory rather than from the root (`/`) of the filesystem.

---

### Absolute Path

A complete file path beginning at the root directory (`/`) that uniquely identifies a resource regardless of the current working directory.

---

### Artifact

The final deliverable produced by an engineering process. In this lab, the backup archive (`.tar.gz`) is the artifact.

---

### Verification

The process of confirming that an archive was created correctly by inspecting its contents before relying on it for recovery.

---

### Backup

A protected copy of data created to support recovery after accidental deletion, corruption, hardware failure, or disaster.

---

### Restore Validation

The process of confirming that recovered files are complete, readable, and match the original data after extraction.
