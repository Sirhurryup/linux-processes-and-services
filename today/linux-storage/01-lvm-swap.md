# Lab 21 – LVM & Swap

## Objective

Build a flexible storage architecture using LVM, expand storage without downtime, and configure swap space for memory overflow.

---

# Business Problem

Traditional disk partitions are fixed after creation. If one partition fills while another has free space, expanding capacity often requires downtime, repartitioning, or data migration.

LVM solves this problem by separating physical storage from logical storage.

---

# LVM Architecture

```text
Disk Image
     │
     ▼
Loopback Device
     │
     ▼
Physical Volume (PV)
     │
     ▼
Volume Group (VG)
     │
     ▼
Logical Volume (LV)
     │
     ▼
Filesystem
     │
     ▼
Mount Point
```

---

# Engineering Workflow

1. Create storage
2. Present storage as a block device
3. Initialize as a Physical Volume
4. Create a Volume Group
5. Create a Logical Volume
6. Create a filesystem
7. Mount and verify
8. Extend the VG
9. Extend the LV
10. Grow the filesystem

---

# Key Commands

```bash
pvcreate
vgcreate
lvcreate
mkfs.ext4
mount
vgextend
lvextend
resize2fs
mkswap
swapon
swapoff
```

---

# Evidence Collected

- Verified LVM tools were installed.
- Created two 100 MiB loopback disks.
- Initialized both as Physical Volumes.
- Created Volume Group `vg0`.
- Created Logical Volume `data`.
- Formatted with ext4.
- Mounted successfully.
- Verified file creation.
- Added a second Physical Volume.
- Expanded the Volume Group.
- Expanded the Logical Volume.
- Resized the filesystem online.
- Created, enabled, verified, and disabled swap.

---

# Engineering Principles

## Principle 1

LVM separates physical storage from application storage.

---

## Principle 2

Storage should be pooled before it is allocated.

---

## Principle 3

Logical Volumes can grow without changing how applications access storage.

---

## Principle 4

Growing an LVM volume and growing the filesystem are two separate operations.

---

## Principle 5

Swap extends virtual memory to improve system survivability under memory pressure, trading performance for stability.

---

# Consulting Perspective

A customer should never care how storage is expanded.

A well-designed storage architecture allows engineers to increase capacity underneath the application while preserving availability and minimizing operational risk.

---

# Commands to Remember

```bash
pvdisplay
vgdisplay
lvdisplay
vgs
df -h
swapon --show
cat /proc/swaps
cat /proc/sys/vm/swappiness
```

---

# Vocabulary

- Physical Volume (PV)
- Volume Group (VG)
- Logical Volume (LV)
- Physical Extent (PE)
- Filesystem
- Mount Point
- Swap
- Swappiness
- Loopback Device
