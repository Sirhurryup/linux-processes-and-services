# Lab 21 Assessment Answers – LVM & Swap

## Answer Key

### Question 1

**Answer:** B

LVM solves the business problem of inflexible storage by allowing capacity to expand without disruptive repartitioning or data migration.

---

### Question 2

**Answer:** C

```
Physical Volume
        ↓
Volume Group
        ↓
Logical Volume
```

Each layer depends on the one before it.

---

### Question 3

**Answer:** B

Loopback devices simulate block devices, allowing engineers to safely practice storage administration without additional physical disks.

---

### Question 4

**Answer:** C

`pvcreate` writes LVM metadata to an existing block device so it can be managed by LVM.

---

### Question 5

**Answer:** B

A Volume Group pools storage from one or more Physical Volumes into a flexible storage reservoir.

---

### Question 6

**Answer:** B

A Logical Volume is only block storage. A filesystem must be created before files can be stored.

---

### Question 7

**Answer:** B

Two disks demonstrate one of LVM's greatest strengths: adding storage later and expanding capacity without rebuilding the application.

---

### Question 8

**Answer:** C

After adding the second Physical Volume, the Volume Group's available storage increased first.

---

### Question 9

**Answer:** A

`lvextend` enlarges the Logical Volume. `resize2fs` expands the filesystem so Linux can use the additional space.

---

### Question 10

**Answer:** C

LVM allows storage capacity to grow while applications remain online.

---

### Question 11

**Answer:** B

Swap provides overflow virtual memory, allowing the operating system to remain functional when RAM is exhausted.

---

### Question 12

**Answer:** C

Database servers typically reduce swappiness to keep frequently accessed database pages in RAM rather than moving them to slower disk storage.

---

# Overall Principles

1. Separate physical storage from logical storage.
2. Pool storage before allocating it.
3. Verify every storage layer before building the next.
4. Growing a Logical Volume and growing a filesystem are separate operations.
5. Swap trades performance for system stability during memory pressure.
