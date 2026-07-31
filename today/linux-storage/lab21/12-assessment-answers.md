# Lab 21 Assessment Answers – LVM and Swap

## Answer Key

### Question 1

**Answer:** B

LVM provides flexible storage management by allowing Logical Volumes to grow without rebuilding traditional partitions.

---

### Question 2

**Answer:** B

A Physical Volume is a storage device prepared for management by LVM and serves as the foundation for Volume Groups.

---

### Question 3

**Answer:** B

A Volume Group combines one or more Physical Volumes into a shared storage pool from which Logical Volumes are allocated.

---

### Question 4

**Answer:** A

A Logical Volume is virtual storage carved from a Volume Group and presented to the operating system like a traditional disk partition.

---

### Question 5

**Answer:** B

`lvextend` increases the size of the Logical Volume, but `resize2fs` expands the ext4 filesystem so it can use the additional space.

---

### Question 6

**Answer:** B

Swap provides virtual memory by allowing Linux to temporarily move inactive memory pages from RAM to disk during memory pressure.

---

### Question 7

**Answer:** C

`swapon` activates configured swap space so the Linux kernel can begin using it.

---

### Question 8

**Answer:** B

Verification confirms that the filesystem is mounted correctly and that data can be successfully written and read.

---

### Question 9

**Answer:** B

One of LVM's greatest advantages is the ability to expand storage online while reducing or eliminating application downtime.

---

### Question 10

**Answer:** B

LVM provides flexible storage architecture, supports online capacity expansion, and integrates with swap to improve system resilience during memory pressure.

---

# Engineering Principles Earned

1. Separate physical storage from logical storage to improve flexibility.
2. Pool storage before allocating it to applications.
3. Expanding storage requires both Logical Volume growth and filesystem growth.
4. Verify storage functionality after every configuration change.
5. Swap extends system resilience but complements—rather than replaces—physical RAM.
