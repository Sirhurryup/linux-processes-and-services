# Lab 24 Assessment Answers – No Space Left on Device

## Answer Key

### Question 1

**Answer:** C

`df -h` identifies which filesystem is full before investigating directories or files.

---

### Question 2

**Answer:** B

`df -h` reports filesystem utilization and available capacity.

---

### Question 3

**Answer:** B

`du` identifies the directories consuming storage within the affected filesystem.

---

### Question 4

**Answer:** C

Large files may contain critical business or application data and should always be verified before removal.

---

### Question 5

**Answer:** C

The `file` command identifies the actual file type by inspecting its contents.

---

### Question 6

**Answer:** B

`ls -lh` presents file sizes using KB, MB, and GB for easier interpretation.

---

### Question 7

**Answer:** A

Verification confirms the corrective action successfully restored the business outcome.

---

### Question 8

**Answer:** B

A root cause explains why the incident occurred rather than simply describing the symptom.

---

### Question 9

**Answer:** B

Professional investigations follow evidence by measuring, verifying, correcting, and validating.

---

### Question 10

**Answer:** C

The objective is restoring business capability while minimizing operational risk.

---

# Overall Principles

1. Follow evidence—not assumptions.
2. `df` identifies the affected filesystem; `du` identifies storage consumers.
3. Verify before deleting business data.
4. Validate every corrective action.
5. The engineer's responsibility is restoring business capability, not merely executing commands.
