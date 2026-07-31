# Lab 21 – LVM and Swap

## Objective

Build flexible storage using Logical Volume Manager (LVM), expand storage online without downtime, and configure swap space to provide virtual memory when physical RAM becomes constrained.

---

# Business Problem

Production systems rarely stop growing. Storage requirements change as applications evolve, databases expand, and customer demand increases.

Traditional partitions are rigid and often require downtime to resize.

Logical Volume Manager (LVM) solves this problem by introducing a flexible storage layer that allows administrators to expand storage without rebuilding disks or interrupting business operations.

---

# Engineering Workflow
```
Create Physical Volume
↓
Create Volume Group
↓
Create Logical Volume
↓
Create Filesystem
↓
Mount Filesystem
↓
Verify Storage
↓
Expand Storage Online
↓
Resize Filesystem
↓
Configure Swap
↓
Validate
```
---

# Commands to Understand

## pvcreate

Initializes a disk or block device as a Physical Volume (PV) that LVM can manage.

---

## vgcreate

Creates a Volume Group (VG) by pooling one or more Physical Volumes into a single storage resource.

---

## lvcreate

Creates a Logical Volume (LV) from available storage inside a Volume Group.

---

## mkfs.ext4

Creates an ext4 filesystem on the Logical Volume.

---

## mount

Attaches the filesystem to the Linux directory tree.

---

## lvextend

Expands the size of an existing Logical Volume without recreating it.

---

## resize2fs

Expands the ext4 filesystem so it recognizes the newly allocated storage.

Growing the Logical Volume alone does not automatically grow the filesystem.

---

## mkswap

Formats a Logical Volume as Linux swap space.

---

## swapon

Enables swap space for use by the Linux kernel.

---

## swapoff

Disables swap space.

---

# Engineering Distinctions

LVM separates physical storage from logical storage.

Traditional Storage

Disk → Partition → Filesystem

LVM Storage

Disk → Physical Volume → Volume Group → Logical Volume → Filesystem

Logical Volumes can be expanded independently of the underlying physical disks.

---

# Evidence Collected

✓ Physical Volume created

✓ Volume Group created

✓ Logical Volume created

✓ ext4 filesystem created

✓ Filesystem mounted successfully

✓ Test file written successfully

✓ Second Physical Volume added

✓ Volume Group expanded

✓ Logical Volume expanded online

✓ Filesystem resized online

✓ Swap Logical Volume created

✓ Swap enabled

✓ Swappiness verified

✓ Swap disabled successfully

✓ Lab passed

---

# Engineering Principles

- Separate physical storage from logical storage.
- Storage should grow without disrupting business operations.
- Expanding a Logical Volume is only half the task; the filesystem must also be expanded.
- Verify storage after every change.
- Swap provides resilience during memory pressure but is slower than RAM.

---

# Consulting Perspective

Modern infrastructure should be designed for growth. LVM enables organizations to expand storage dynamically without interrupting application availability, reducing operational risk and maintenance windows.

---

# Vocabulary

### Physical Volume (PV)

A storage device initialized for management by LVM.

---

### Volume Group (VG)

A storage pool created by combining one or more Physical Volumes.

---

### Logical Volume (LV)

A virtual storage device allocated from a Volume Group.

---

### Filesystem

The structure Linux uses to organize and access files.

---

### Physical Extent (PE)

The fixed-size allocation unit used internally by a Volume Group.

---

### Swap Space

Disk storage temporarily used as virtual memory when physical RAM becomes constrained.
