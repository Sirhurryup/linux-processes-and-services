# Lab 26 Assessment – Recover a Detached Volume

## Instructions

Choose the BEST answer.

---

### Question 1

What was the primary business problem in this lab?

A. The filesystem image was corrupted.

B. The application was reading from an unmounted directory.

C. The ext4 filesystem needed formatting.

D. The loop device failed.

---

### Question 2

Why was the `file` command used?

A. To measure disk usage.

B. To determine the actual contents of `datavol.img`.

C. To mount the filesystem.

D. To update `/etc/fstab`.

---

### Question 3

Why was `mountpoint` executed before mounting?

A. To determine whether the destination was already mounted.

B. To compress the image.

C. To repair the filesystem.

D. To identify the filesystem UUID.

---

### Question 4

What does the `-o loop` option accomplish?

A. Compresses the filesystem.

B. Creates a backup.

C. Treats a regular file as a virtual block device for mounting.

D. Repairs ext4 metadata.

---

### Question 5

Why verify `important.txt` after mounting?

A. To test file permissions.

B. To prove the required business data was successfully recovered.

C. To create the file.

D. To compress the recovered data.

---

### Question 6

What is the purpose of `/etc/fstab`?

A. Stores backup archives.

B. Configures automatic filesystem mounts during boot.

C. Creates loop devices.

D. Tracks log rotation.

---

### Question 7

Why run `mount -a` after updating `/etc/fstab`?

A. To validate the configuration before rebooting.

B. To resize the filesystem.

C. To compress the filesystem image.

D. To rebuild the ext4 journal.

---

### Question 8

Which statement best describes a loop device?

A. A dedicated hardware disk.

B. A network storage protocol.

C. A virtual block device backed by a regular file.

D. A compressed archive.

---

### Question 9

Which action prevented the incident from occurring again after reboot?

A. Reading `important.txt`.

B. Using `mountpoint`.

C. Adding the mount entry to `/etc/fstab`.

D. Running `file`.

---

### Question 10

Which statement best summarizes this lab?

A. Recovery ends after mounting the filesystem.

B. Successful recovery restores service, verifies data, and persists the configuration.

C. Mounting automatically updates `/etc/fstab`.

D. Loop devices permanently replace physical disks.
