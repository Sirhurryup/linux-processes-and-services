# Lab 21 Assessment – LVM and Swap

## Instructions

Choose the BEST answer.

---

### Question 1

Why is LVM preferred over traditional disk partitions?

A. It encrypts storage automatically.

B. It allows storage to be expanded more flexibly.

C. It eliminates filesystems.

D. It replaces Linux permissions.

---

### Question 2

What is the purpose of a Physical Volume (PV)?

A. It stores application logs.

B. It provides storage that LVM can manage.

C. It mounts filesystems.

D. It compresses disks.

---

### Question 3

What is the role of a Volume Group (VG)?

A. It formats filesystems.

B. It pools one or more Physical Volumes into shared storage.

C. It mounts Logical Volumes.

D. It enables swap.

---

### Question 4

What does a Logical Volume (LV) represent?

A. Virtual storage allocated from a Volume Group.

B. A physical disk.

C. A network share.

D. A backup archive.

---

### Question 5

Why is `resize2fs` required after `lvextend`?

A. To mount the filesystem.

B. To expand the filesystem so it can use the newly allocated storage.

C. To enable swap.

D. To create a Volume Group.

---

### Question 6

What is the purpose of swap space?

A. Permanent file storage.

B. Additional virtual memory when RAM becomes constrained.

C. Backup storage.

D. Archive storage.

---

### Question 7

Which command enables swap?

A. swapoff

B. mkswap

C. swapon

D. vgcreate

---

### Question 8

Why verify the mounted filesystem after creation?

A. To improve performance.

B. To confirm storage is accessible and writable.

C. To compress the filesystem.

D. To create a Volume Group.

---

### Question 9

Which statement best describes online storage expansion?

A. Storage can only be expanded after rebooting.

B. LVM allows storage growth while minimizing application downtime.

C. Filesystems automatically resize themselves.

D. Online expansion only applies to swap.

---

### Question 10

Which statement best summarizes this lab?

A. LVM eliminates the need for filesystems.

B. LVM provides flexible storage management while supporting online growth and virtual memory through swap.

C. Swap permanently replaces RAM.

D. Physical Volumes cannot be expanded.
