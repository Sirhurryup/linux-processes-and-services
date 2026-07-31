# Lab 21 Assessment – LVM & Swap

## Instructions

Select the best answer. Do not use notes.

---

### Question 1

What business problem does LVM primarily solve?

A. Faster CPU scheduling

B. Flexible storage expansion without disruptive repartitioning

C. Increased RAM

D. Improved networking

---

### Question 2

What is the correct architectural order?

A. VG → PV → LV

B. PV → LV → VG

C. PV → VG → LV

D. LV → VG → PV

---

### Question 3

Why are loopback devices used in this lab?

A. They improve disk performance.

B. They simulate block devices without requiring additional physical disks.

C. They automatically create Volume Groups.

D. They replace Logical Volumes.

---

### Question 4

What does `pvcreate` actually do?

A. Creates a new disk.

B. Formats a filesystem.

C. Writes LVM metadata to an existing block device so LVM can manage it.

D. Mounts storage.

---

### Question 5

What is the primary responsibility of a Volume Group?

A. Store files.

B. Pool one or more Physical Volumes into flexible storage.

C. Mount filesystems.

D. Manage swap.

---

### Question 6

Why couldn't users store files immediately after `lvcreate`?

A. The LV was read-only.

B. The filesystem had not yet been created.

C. The VG was inactive.

D. The mount point was missing.

---

### Question 7

Why did we create two 100 MB disks instead of one 200 MB disk?

A. Linux limits image files to 100 MB.

B. To demonstrate adding storage later and expanding the Volume Group without rebuilding the application.

C. It improves performance.

D. LVM requires two disks.

---

### Question 8

After adding the second Physical Volume, what changed first?

A. Filesystem size

B. Mount point

C. Volume Group capacity

D. Application data

---

### Question 9

Why wasn't `lvextend` the final step?

A. The filesystem must also be expanded to recognize the additional space.

B. The kernel requires a reboot.

C. The Volume Group must be recreated.

D. The mount point changes.

---

### Question 10

What business advantage does online LVM expansion provide?

A. Faster CPUs

B. Additional RAM

C. Increased storage capacity without interrupting applications

D. Smaller filesystems

---

### Question 11

Why is swap considered a safety mechanism?

A. It replaces RAM.

B. It provides overflow virtual memory, helping the system remain operational when RAM is exhausted.

C. It speeds up storage.

D. It backs up application data.

---

### Question 12

Why might a database server lower the swappiness value?

A. To increase disk activity.

B. To encourage swapping.

C. To keep frequently accessed database pages in RAM.

D. To disable virtual memory.




```
dd

↓

losetup

↓

pvdisplay

↓

vgdisplay

↓

lvdisplay

↓

df -h

↓

swapon --show
```
