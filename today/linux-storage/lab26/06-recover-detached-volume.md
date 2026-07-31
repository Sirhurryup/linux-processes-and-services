# Lab 26 – Recover a Detached Volume

## Objective

Recover an unmounted ext4 filesystem image, restore application access by mounting it to the correct location, verify the recovered data, and persist the mount so it survives future reboots.

---

# Business Problem

A production application depends on data stored under `/mnt/recovered`. Overnight, the storage volume became detached, leaving the application pointing to an empty directory.

The objective is not simply to mount a filesystem—it is to restore business capability while ensuring the recovery survives future system restarts.

---

# Engineering Workflow

Recovery Incident
↓
Verify Recovery Source
↓
Identify Filesystem
↓
Confirm Destination is Unmounted
↓
Mount Using Loop Device
↓
Verify Business Data
↓
Persist Recovery in /etc/fstab
↓
Validate Configuration
↓
Grade

---

# Commands to Understand

## ls -lh /srv/datavol.img

Verifies the recovery image exists and provides its size.

---

## file /srv/datavol.img

Identifies the contents of the image.

In this lab it confirmed the file contains an ext4 filesystem.

---

## mountpoint /mnt/recovered

Determines whether something is currently mounted at the recovery location.

---

## mount -o loop

Mounts a filesystem stored inside a regular file.

`-o loop` tells Linux to treat the image as a virtual block device.

---

## cat /mnt/recovered/important.txt

Verifies the recovered business data.

---

## /etc/fstab

Defines filesystems that should automatically mount during system startup.

---

## mount -a

Validates `/etc/fstab` by attempting to mount every configured filesystem without requiring a reboot.

---

# Engineering Distinctions

A directory existing is not proof that its filesystem is mounted.

An empty mount directory does **not** automatically indicate lost data.

Always verify the mount before concluding data is missing.

---

# Evidence Collected

✓ Recovery image verified

✓ ext4 filesystem identified

✓ Destination confirmed unmounted

✓ Mounted successfully using loop device

✓ important.txt recovered

✓ File contained:

```
RECOVERED-DATA-OK
```

✓ /etc/fstab updated

✓ mount -a completed successfully

✓ Lab passed

---

# Engineering Principles

- Recovery begins with evidence.
- Verify the source before making changes.
- Validate recovered business data before closing the incident.
- Recovery restores service.
- Persistence prevents repeat incidents.

---

# Consulting Perspective

Successful recovery is measured by restoring business capability and preventing recurrence. Mounting the filesystem solved today's outage; updating `/etc/fstab` prevented tomorrow's.

---

# Vocabulary

### Loop Device

A virtual block device that allows Linux to mount a filesystem stored inside a regular file.

---

### Filesystem Image

A file containing an entire filesystem.

---

### Mount Point

The directory where a filesystem becomes accessible.

---

### Recovery

The process of restoring access to existing data.

---

### Persistence

Configuration that survives system reboot.

---

### /etc/fstab

The Linux configuration file that defines filesystems automatically mounted during boot.
