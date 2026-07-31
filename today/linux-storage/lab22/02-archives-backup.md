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

# Key Commands

```bash
tar -czf
tar -tzf
tar -xzf

tar -cjf
tar -cJf

rsync -av
rsync -av --delete
```

---

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

Archive

Compression

Restore

Snapshot

Incremental Backup

Synchronization

Mirror

Retention

Dry Run

Trailing Slash
