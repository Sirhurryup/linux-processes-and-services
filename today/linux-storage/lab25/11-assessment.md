# Lab 25 Assessment – Build It From Requirements: Backup Script

## Instructions

Choose the BEST answer.

---

### Question 1

What is the primary business reason for automating backups?

A. To reduce typing.

B. To produce consistent, repeatable backups.

C. To compress files.

D. To reduce CPU utilization.

---

### Question 2

Why should backup archives include timestamps?

A. To improve compression.

B. To make the archive smaller.

C. To prevent overwriting previous backups and identify when each backup was created.

D. To improve execution speed.

---

### Question 3

What does the command `tar -czf` create?

A. A gzip-compressed tar archive.

B. A text file.

C. A filesystem.

D. A loop device.

---

### Question 4

What does an exit code of `0` indicate?

A. The previous command completed successfully.

B. The backup was compressed.

C. The archive is encrypted.

D. The filesystem was mounted.

---

### Question 5

Why inspect the archive after creating it?

A. To improve performance.

B. To rename the archive.

C. To delete duplicate files.

D. To verify the required files are actually inside the backup.

---

### Question 6

Which command lists the contents of a compressed archive without extracting it?

A. tar -xzf

B. tar -tzf

C. gzip

D. ls -R

---

### Question 7

Why make `backup.sh` executable?

A. To compress the script.

B. To create a tar archive.

C. So Linux can execute it directly.

D. To mount the backup.

---

### Question 8

Which provides the strongest evidence that the backup succeeded?

A. The script printed a success message.

B. The script exists.

C. The archive filename contains today's date.

D. The script returned exit code 0 **and** the archive contains every required file.

---

### Question 9

Why inventory the source directory before writing the script?

A. To determine how much RAM is available.

B. To identify every file and subdirectory that must be included in the backup.

C. To compress the data.

D. To create a loop device.

---

### Question 10

Which statement best summarizes this lab?

A. Automation eliminates the need for verification.

B. A successful script only needs to execute without errors.

C. Reliable automation requires building, verifying, and validating the backup artifact.

D. Backup scripts should always overwrite previous backups.
