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


