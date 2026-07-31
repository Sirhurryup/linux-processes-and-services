# Storage Interview Language

The following statements summarize engineering decisions made throughout Labs 21–26.

---

## Storage Growth

"I used Logical Volume Manager to separate physical storage from logical storage, allowing capacity expansion without rebuilding traditional partitions."

---

## Online Expansion

"After extending the Logical Volume, I resized the filesystem so the operating system could utilize the newly allocated storage."

---

## Backup Validation

"I validated the backup artifact rather than relying solely on successful script execution."

---

## Automation

"I automated a repeatable backup process that consistently produces timestamped recovery artifacts."

---

## Troubleshooting

"The evidence identified the affected filesystem before I investigated storage consumers."

---

## Disk Full

"I treated the largest file as a suspect rather than assuming it was the root cause."

---

## File Identification

"I verified the file's contents before making any destructive changes."

---

## Recovery

"I confirmed the filesystem image existed, verified its type, restored access through a loop-mounted filesystem, and validated the recovered business data."

---

## Persistence

"After restoring service, I updated `/etc/fstab` and validated the configuration to ensure recovery persisted across future reboots."

---

## Verification

"I verify every engineering change using observable evidence rather than assuming success from command output."

---

## Consulting Perspective

"My objective was not simply to restore storage, but to restore business capability while minimizing operational risk and preventing future recurrence."
