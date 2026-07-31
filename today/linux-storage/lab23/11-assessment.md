# 11-assessment.md

# Lab 23 Assessment – Log Rotation

## Instructions

Choose the BEST answer without using your notes.

---

### Question 1

What business problem does log rotation primarily solve?

A. Encrypts application logs.

B. Prevents disks from filling by automatically managing log growth.

C. Speeds up CPU performance.

D. Increases network bandwidth.

---

### Question 2

What is the primary purpose of `/etc/logrotate.conf`?

A. Store application logs.

B. Define global log rotation defaults.

C. Compress log files.

D. Create log directories.

---

### Question 3

Why are files stored inside `/etc/logrotate.d/`?

A. To replace the global configuration.

B. To define service-specific rotation policies.

C. To store archived logs.

D. To configure cron jobs.

---

### Question 4

What does the directive `rotate 7` mean?

A. Rotate the log seven times each day.

B. Keep seven historical rotated log files.

C. Compress seven log files.

D. Rotate every seven hours.

---

### Question 5

Why is the `create` directive important?

A. It compresses the previous log.

B. It creates a new empty log so the application can continue writing.

C. It deletes old log files.

D. It forces rotation.

---

### Question 6

What does `compress` accomplish?

A. Encrypts the log.

B. Reduces storage consumption of rotated logs.

C. Deletes duplicate logs.

D. Improves CPU utilization.

---

### Question 7

Why should engineers run `logrotate -d` before deploying a new policy?

A. It rotates logs immediately.

B. It validates the configuration without making changes.

C. It deletes old logs.

D. It restarts logrotate.

---

### Question 8

What is the purpose of `logrotate -f`?

A. Force an immediate rotation regardless of schedule.

B. Repair corrupted logs.

C. Compress all logs.

D. Disable rotation.

---

### Question 9

Why is retention considered a business decision?

A. Linux requires every business to keep seven logs.

B. It is driven by operational, auditing, legal, and compliance requirements.

C. It improves processor speed.

D. It determines compression level.

---

### Question 10

After a successful rotation, what should always exist?

A. Only the compressed archive.

B. Only the deleted log.

C. A new active log ready for the application.

D. An empty log directory.

---

### Question 11

A production application suddenly stops writing logs after rotation.

Which configuration directive would you investigate first?

A. compress

B. create

C. rotate

D. daily

---

### Question 12

What is the best measure of a successful log rotation policy?

A. The configuration file has no syntax errors.

B. Logs are compressed.

C. The application continues operating while historical logs are retained according to policy.

D. The disk usage decreases immediately.
