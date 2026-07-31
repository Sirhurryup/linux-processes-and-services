# Appendix – Storage Size Quick Reference

## Why This Matters

Storage size alone means very little.

Engineers evaluate storage **relative to the filesystem, workload, and business impact**.

---

# Storage Hierarchy

| Unit | Approximate Size | Think Of |
|------|-----------------:|----------|
| Byte (B) | 1 character | Individual letters and numbers |
| Kilobyte (KB) | ~1,024 Bytes | Configuration files, scripts, README files |
| Megabyte (MB) | ~1,024 KB | Images, PDFs, installers, application logs |
| Gigabyte (GB) | ~1,024 MB | Databases, VM disks, backups, Docker images |
| Terabyte (TB) | ~1,024 GB | Enterprise storage, SAN, NAS, cloud storage |

---

# Engineering Intuition

### KB

Usually too small to create storage problems.

Examples:

- README.md
- app.conf
- Shell scripts
- SSH configuration

---

### MB

Large enough to matter on small filesystems.

Examples:

- Images
- PDF documents
- Application installers
- Log files

Example:

```
50 MB log
```

On a:

```
53 MB filesystem
```

This is a critical incident.

---

### GB

Normal scale for production workloads.

Examples:

- Database backups
- Virtual machine disks
- Docker images
- Application data
- Large log directories

Example:

```
12 GB log
```

On a:

```
20 GB partition
```

This is a serious operational problem.

---

### TB

Enterprise scale.

Examples:

- Storage arrays
- SAN
- NAS
- Amazon S3
- Enterprise databases

---

# Engineering Rule

Never ask:

> "Is this file large?"

Instead ask:

> "Large compared to what?"

Always compare file size to:

- Filesystem capacity
- Remaining free space
- Expected application behavior

---

# Three Questions Every Engineer Should Ask

## 1. How large is the filesystem?

Example:

```
53 MB
```

or

```
2 TB
```

---

## 2. What percentage of the filesystem does this file consume?

Example:

```
50 MB file

53 MB filesystem

≈ 94% utilization
```

The percentage often matters more than the raw size.

---

## 3. Is this size expected?

Examples:

| File Type | Expected Size |
|-----------|---------------|
| README | KB |
| Configuration | KB |
| Log | MB to GB (depends on workload) |
| Database | GB to TB |
| Backup Archive | GB to TB |

Unexpected growth usually indicates an operational problem.

---

# Production Thinking

Instead of saying:

> "The log file is 50 MB."

Say:

> "The log file consumes approximately 94% of a 53 MB filesystem."

That statement immediately communicates business impact.

---

# Remember

Storage investigations follow this order:

```
Filesystem
      │
      ▼
Directory
      │
      ▼
Largest File
      │
      ▼
Verify
      │
      ▼
Correct
      │
      ▼
Validate
```

Never skip verification.

Evidence drives engineering decisions.

---

# Consulting Perspective

Customers do not care whether a file is measured in MB or GB.

They care whether storage capacity prevents the application from serving users.

Always communicate storage in terms of **business impact**, not just file size.
