# 03-log-rotation.md

# Lab 23 – Log Rotation

## Objective

Prevent disk-full outages by automatically rotating, compressing, and removing old log files according to business-defined retention policies.

---

# Business Problem

Applications continuously generate logs. Without automated management, log files eventually consume all available disk space, preventing applications and databases from writing data and causing avoidable production outages.

---

# Core Architecture

```
Application
      │
      ▼
Writes app.log
      │
      ▼
logrotate Policy
      │
      ├─────────────┐
      ▼             ▼
Rename       Compress Older Logs
      │             │
      └──────┬──────┘
             ▼
Create New app.log
             │
             ▼
Application Continues Logging
```

---

# Global vs Service Configuration

| File | Responsibility |
|------|----------------|
| /etc/logrotate.conf | Global defaults for all services |
| /etc/logrotate.d/ | Service-specific rotation policies |

**Engineering Principle**

Global defaults establish organizational policy.

Individual services override only what their workload requires.

---

# Rotation Lifecycle

```
app.log
      │
      ▼
app.log.1
      │
      ▼
app.log.1.gz
      │
      ▼
Retention Policy
      │
      ▼
Deletion
```

---

# Directive Reference

| Directive | Purpose | Business Value |
|-----------|----------|----------------|
| daily | Rotate every day | Controls growth of busy applications |
| rotate 7 | Keep seven historical logs | One week of troubleshooting history |
| compress | Compress rotated logs | Reduce disk consumption |
| missingok | Ignore missing logs | Prevent automation failures |
| notifempty | Skip empty logs | Avoid wasting storage |
| create 0644 root root | Create a new writable log | Application immediately resumes logging |

---

# Engineering Distinctions

**rotate 12** means:

> Keep the most recent **12 rotated log files**.

It does **NOT** mean rotate the log twelve times.

---

# Engineering Workflow

1. Review global configuration.
2. Review service-specific policies.
3. Create application log.
4. Create custom rotation policy.
5. Validate policy using debug mode.
6. Force rotation.
7. Verify compressed history.
8. Verify new log creation.
9. Confirm application continues writing.
10. Validate retention policy.

---

# Commands to Understand

```bash
cat /etc/logrotate.conf
```

Reads the global logrotate configuration.

---

```bash
ls /etc/logrotate.d
```

Lists service-specific rotation policies.

---

```bash
cat /etc/logrotate.d/myapp
```

Displays a service's rotation rules.

---

```bash
sudo logrotate -d /etc/logrotate.conf
```

Debug mode.

Performs a dry run without modifying any logs.

---

```bash
sudo logrotate -f /etc/logrotate.d/myapp
```

Forces immediate rotation regardless of schedule.

Useful for safely testing new policies.

---

```bash
sudo logrotate -vf /etc/logrotate.d/myapp
```

Verbose + Force.

Shows exactly what logrotate is doing while forcing rotation.

---

```bash
sudo ls -lh /var/log/myapp
```

Verify rotated logs and compressed archives.

---

```bash
sudo wc -c /var/log/myapp/app.log
```

Confirm the newly created log begins empty.

---

# Evidence Collected

- Reviewed global configuration.
- Reviewed service-specific configuration.
- Created custom application log.
- Built a custom rotation policy.
- Validated policy with debug mode.
- Forced rotation successfully.
- Verified compressed historical log.
- Verified creation of a new writable log.
- Confirmed application continued logging after rotation.

---

# Engineering Principles

## Principle 1

Disk capacity is a finite business resource that must be managed proactively.

---

## Principle 2

Log rotation is preventive maintenance, not incident response.

---

## Principle 3

Always validate a new logrotate policy before deploying it into production.

---

## Principle 4

Retention policies are business decisions driven by operational, legal, auditing, and compliance requirements.

---

## Principle 5

A successful rotation preserves application availability while controlling storage growth.

---

# Consulting Perspective

Customers never notice successful log rotation.

They immediately notice when applications stop responding because disks became full.

Log rotation is an operational control that quietly protects business continuity every day.

---

# Vocabulary

Log Rotation

Retention

Compression

Archive

Rotation Policy

Global Configuration

Service Configuration

Debug Mode

Forced Rotation

Retention Window

Business Continuity
