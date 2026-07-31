# Lab 26 Assessment Answers – Recover a Detached Volume

## Answer Key

### Question 1

**Answer:** B

The application was pointing to an empty directory because the required filesystem was no longer mounted.

---

### Question 2

**Answer:** B

The `file` command identified that `datavol.img` contained an ext4 filesystem image, confirming it was suitable for mounting.

---

### Question 3

**Answer:** A

`mountpoint` verified that the destination was not already in use before performing the recovery.

---

### Question 4

**Answer:** C

The `-o loop` option allows Linux to treat a regular file as a virtual block device, making it possible to mount the contained filesystem.

---

### Question 5

**Answer:** B

Reading `important.txt` verified that the business-critical data had been successfully recovered after mounting.

---

### Question 6

**Answer:** B

`/etc/fstab` defines which filesystems Linux should automatically mount during system startup.

---

### Question 7

**Answer:** A

Running `mount -a` validates the `/etc/fstab` configuration immediately, allowing errors to be discovered before the next reboot.

---

### Question 8

**Answer:** C

A loop device is a virtual block device backed by a regular file, allowing filesystem images to be mounted like physical disks.

---

### Question 9

**Answer:** C

Adding the recovery entry to `/etc/fstab` ensured the filesystem would automatically mount after future reboots, preventing recurrence.

---

### Question 10

**Answer:** B

A complete recovery includes restoring access, verifying the recovered business data, and making the recovery persistent.

---

# Engineering Principles Earned

1. Recovery begins with evidence, not assumptions.
2. Verify both the recovery source and destination before making changes.
3. Successful recovery restores business capability, not just mounted filesystems.
4. Validate recovered data before closing an incident.
5. Persistence prevents repeat incidents by ensuring recovery survives reboot.
