# Business Problem

## Engagement

**Client:** SirhurryUp Corporation
**Scenario:** Production Storage Outage Investigation

---

## Business Problem

At 3:00 AM, a production Linux server exhausted its available storage capacity, preventing business-critical applications from writing logs, temporary files, and customer data. The resulting outage delayed recovery because engineers lacked a structured methodology for identifying which filesystem was full, what was consuming the available space, and how to safely restore service.

SirhurryUp Corporation has been engaged to transform this technical incident into an engineering framework that enables future engineers to investigate storage incidents through evidence rather than assumption, reducing operational risk while improving service reliability.

---

## Business Impact

Failure to quickly diagnose storage incidents can result in:

- Customer-facing application outages
- Delayed transactions
- Loss of customer confidence
- Increased operational costs
- Extended recovery times
- Greater business risk

---

## Primary Stakeholders

| Stakeholder | Responsibility |
|------------|----------------|
| CEO | Protect customer trust and business continuity |
| Operations Manager | Restore production services quickly |
| Linux Engineer | Investigate storage through evidence |
| Application Team | Maintain application availability |
| Customers | Expect reliable access to their data |

---

## SirhurryUp Engineering Principle

> **Never Duplicate. Investigate. Validate. Elevate.**

Every storage incident begins with a claim.

Every engineering decision ends with evidence.

SirhurryUp Corporation transforms technical labs into engineering methodologies by connecting technology decisions to business outcomes.

# Business Capability

## Core Capability

Storage provides the organization with the repeatable ability to preserve, protect, and retrieve business information whenever it is needed.

Customers never purchase hard drives, partitions, or filesystems.

They purchase confidence.

They expect their financial records, medical information, application data, and business documents to remain accurate, available, and recoverable whenever business operations require them.

Storage is the business capability that fulfills this expectation.

---

## Business Outcome

When this capability is delivered consistently:

- Customers trust the organization with their information.
- Employees perform their jobs without interruption.
- Applications continue operating reliably.
- Business operations remain resilient.
- Organizational risk is reduced.

---

## SirhurryUp Corporation Perspective

Technology is never the capability.

Technology enables the capability.

Storage technologies such as disks, partitions, filesystems, and mount points exist to support one business objective:

> Deliver reliable preservation and retrieval of business information so the organization can operate with confidence.

---

## Engineering Principle

Business capabilities describe **what the organization must consistently deliver**.

Technology describes **how engineers make that delivery possible**.

---

## Storage Investigation Foundation

Before investigating a storage incident, the engineer must understand the environment in which the problem exists.

Mapping the storage landscape allows the engineer to work in a deliberate order, identify which storage resources support the affected business service, and collect evidence without creating additional operational risk.

Linux storage follows this path:

```text
Physical Disk → Partition → Filesystem → Mount Point → Business Data

---

## Section 1 – Inspecting the Storage Landscape

### Objective

Before investigating storage utilization, the engineer must identify the storage resources available to the operating system.

### Command

```bash
lsblk
```

### Evidence Collected

| Device | Type | Size | Observation |
|---------|------|------|-------------|
| nvme0n1 | Physical Disk | 30 GB | Primary storage device attached to the system |
| nvme0n1p1 | Partition | 30 GB | Mounted and actively used by the operating system |
| nvme0n1p127 | Partition | 1 MB | Reserved partition |
| nvme0n1p128 | Partition | 10 MB | Reserved partition |

### Engineering Observation

The investigation begins by identifying the storage devices available to Linux.

At this stage we are **not** determining whether storage is full.

We are establishing the storage landscape so future evidence can be interpreted within the correct context.

### SirhurryUp Principle

> Never investigate evidence without first understanding the environment that produced it.


### Command

```bash
lsblk -f
```

### Evidence Collected

| Device | Filesystem | Available | Used | Observation |
|---------|------------|-----------|------|-------------|
| nvme0n1 | Not Displayed | — | — | Physical storage device |
| nvme0n1p1 | Not Displayed | 26.8 GB | 10% | Active partition with available storage information |
| nvme0n1p127 | Not Displayed | — | — | Reserved partition |
| nvme0n1p128 | Not Displayed | — | — | Reserved partition |

### Engineering Observation

The `-f` option requests additional filesystem information such as filesystem type, UUID, available capacity, utilization, and mount points.

In this lab environment, not every field is populated because the operating system is running inside a managed container. Even though some metadata is unavailable, the command still provides useful evidence regarding available storage and utilization.

An engineer records the evidence presented by the system rather than assuming information that is not available.

### SirhurryUp Engineering Principle

> Engineering decisions are based on observable evidence, not expected evidence.


### Command

```bash
cat /etc/fstab
```

### Evidence Collected

```text
# UNCONFIGURED FSTAB FOR BASE SYSTEM
```

### Engineering Observation

The `/etc/fstab` file is normally used to define which filesystems Linux should mount automatically during system startup.

In this training environment, the file is intentionally unconfigured because the underlying infrastructure manages the storage configuration. This differs from a traditional production server where engineers maintain persistent mount definitions.

Rather than assuming an error, the engineer evaluates the evidence within the context of the operating environment.

### Why This Matters

Understanding environmental differences prevents engineers from making incorrect assumptions during investigations. Cloud-managed systems, containers, and training environments often abstract infrastructure components that would normally be visible on traditional Linux servers.

### SirhurryUp Engineering Principle

> Evidence must always be interpreted within the context of the operating environment.

### Command

```bash
df -h
```

### Evidence Collected

| Filesystem | Size | Used | Available | Utilization | Mount Point |
|------------|-----:|-----:|----------:|------------:|-------------|
| overlay | 30 GB | 3.2 GB | 27 GB | 11% | / |
| /dev/nvme0n1p1 | 30 GB | 3.2 GB | 27 GB | 11% | /etc/hosts |

### Engineering Observation

The `df -h` command reports storage utilization for mounted filesystems.

The root filesystem is currently using **11%** of its available capacity, leaving approximately **27 GB** of free space.

Based on the collected evidence, there is no indication of storage exhaustion or an immediate operational risk.

This establishes the baseline health of the storage capability before investigating individual directories or files.

### Business Interpretation

The organization's storage capability is operating within acceptable capacity.

Applications retain sufficient free space to continue writing logs, temporary files, and business data without interruption.

No immediate customer impact is expected based on current utilization.

### SirhurryUp Engineering Principle

> Measure the health of the business capability before searching for the cause of a problem.

### Command

```bash
df -h /var/log
```

### Evidence Collected

| Directory | Filesystem | Available | Utilization |
|-----------|------------|----------:|------------:|
| /var/log | overlay | 27 GB | 11% |

### Engineering Observation

The `/var/log` directory resides on the same filesystem as the root (`/`) filesystem.

Current utilization remains at **11%**, indicating that log storage is not contributing to an immediate storage-related incident.

No additional investigation into log growth is warranted at this stage because the collected evidence shows adequate available capacity.

### Business Capability Assessment (BCA)

The organization's logging capability remains healthy because sufficient storage capacity exists to continue recording operational and security events.

Maintaining available storage for log files ensures engineers can investigate future incidents while allowing business applications to continue operating without interruption.

### SirhurryUp Engineering Principle

> Investigate the specific business capability only after verifying the health of the underlying capability that supports it.

### Command

```bash
du -sh /var/log
```

### Evidence Collected

```text
du: cannot read directory '/var/log/private': Permission denied
8.9M    /var/log
```

### Engineering Observation

The `du` command measures the disk space consumed by a directory and its contents.

The `/var/log` directory currently consumes approximately **8.9 MB** of storage.

During the investigation, Linux denied access to the `/var/log/private` directory because the current user does not have sufficient permissions to read its contents. The command still completed successfully and reported the size of the accessible portions of the directory.

### Business Capability Assessment (BCA)

The logging capability is consuming a very small portion of the available storage capacity.

No evidence suggests that log growth is placing the storage capability at operational risk. The permission restriction also demonstrates that Linux protects sensitive log data through access controls, reducing the risk of unauthorized disclosure.

### SirhurryUp Engineering Principle

> Successful investigations acknowledge both the evidence collected and the evidence that could not be collected because of security boundaries.
### Command

```bash
sudo du -sh /var/log
```

### Evidence Collected

```text
8.9M    /var/log
```

### Engineering Observation

Running the command with elevated privileges removed the permission restriction encountered during the previous investigation.

The reported size remained **8.9 MB**, confirming that the protected directory did not significantly affect the overall storage consumption of `/var/log`.

Comparing results before and after privilege escalation increases confidence that the collected evidence accurately represents the directory's storage usage.

### Business Capability Assessment (BCA)

The logging capability continues to consume minimal storage resources and presents no measurable risk to the organization's storage capability.

Elevated privileges allowed the engineer to validate the completeness of the investigation without revealing any additional operational concerns.

### SirhurryUp Engineering Principle

> Escalate privileges only when necessary to validate evidence, not as the default starting point.

## Storage Consumption Investigation and Cleanup

### Engineering Question

Which directories and files are consuming storage, and can unnecessary consumption be removed without affecting system operations?

### Commands Used

```bash
sudo du -sh /* 2>/dev/null | sort -rh | head -10
sudo find / -type f -size +100M 2>/dev/null
dd if=/dev/zero of=/tmp/bigfile.dat bs=1M count=50 2>/dev/null
ls -lh /tmp/bigfile.dat
sudo find /tmp -type f -size +10M
sudo rm -rf /tmp/bigfile.dat
sudo apt clean
df -h /
```

### Evidence Collected

The largest top-level directories were:

| Directory | Storage Used |
|---|---:|
| `/usr` | 407 MB |
| `/var` | 28 MB |
| `/etc` | 2 MB |

A 50 MB test file was created in `/tmp`, successfully detected, and removed.

Two simulated runaway files were later created:

| File | Size |
|---|---:|
| `/tmp/runaway-log-2.dat` | 40 MB |
| `/tmp/runaway-log-1.dat` | 30 MB |

Both files were identified through directory ranking and file-size searches before being removed.

### Engineering Observation

The investigation followed a narrowing process:

1. Rank top-level directories.
2. Search for unusually large files.
3. Confirm individual file sizes.
4. Remove only verified test data.
5. Recheck filesystem utilization.

The root filesystem remained at approximately **11% utilization** after cleanup. The deleted files were too small relative to the 30 GB filesystem for the rounded percentage to visibly change.

### Business Capability Assessment (BCA)

The storage capability remains healthy and has sufficient capacity to support application data, logs, and temporary operations.

The investigation also demonstrated that engineers can locate and remove abnormal storage consumption without deleting unverified business data.

### SirhurryUp Engineering Principle

> Rank broadly, investigate narrowly, validate precisely, and delete only verified data.
