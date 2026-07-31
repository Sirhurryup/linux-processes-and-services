# 12-assessment-answers.md

# Lab 23 Assessment Answers – Log Rotation

## Answer Key

### Question 1

**Answer:** B

Log rotation automatically manages log growth to prevent disks from filling and causing production outages.

---

### Question 2

**Answer:** B

`/etc/logrotate.conf` defines the global defaults that apply unless overridden by service-specific configurations.

---

### Question 3

**Answer:** B

Files in `/etc/logrotate.d/` contain rotation policies for individual services.

---

### Question 4

**Answer:** B

`rotate 7` means retain the seven most recent rotated log files—not rotate seven times.

---

### Question 5

**Answer:** B

The `create` directive immediately creates a new empty log so the application can continue logging after rotation.

---

### Question 6

**Answer:** B

Compression reduces the storage consumed by historical log files.

---

### Question 7

**Answer:** B

Debug mode validates what logrotate would do without modifying any files.

---

### Question 8

**Answer:** A

`-f` forces rotation immediately, regardless of the configured schedule.

---

### Question 9

**Answer:** B

Retention policies are determined by business, legal, auditing, and operational requirements.

---

### Question 10

**Answer:** C

A new active log should always exist so the application can continue writing immediately.

---

### Question 11

**Answer:** B

Without the `create` directive (or an equivalent application action), the application may not have a writable log after rotation.

---

### Question 12

**Answer:** C

The true measure of success is uninterrupted application logging while historical logs are retained according to business policy.

---

# Overall Principles

1. Log rotation prevents predictable disk-full outages.
2. Global defaults establish policy; service configurations customize behavior.
3. Always validate a rotation policy before deploying it.
4. Retention is driven by business and compliance requirements.
5. Successful rotation preserves both historical evidence and application availability.
6. Automation should prevent incidents before customers experience them.
