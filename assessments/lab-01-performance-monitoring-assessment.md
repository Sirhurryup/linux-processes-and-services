# Lab 01 Assessment
## Performance & Resource Monitoring

**Repository:** Linux Processes and Services

**Organization:** SirhurryUp Corporation

---

# Instructions

Choose the BEST answer.

Focus on engineering reasoning rather than command memorization.

---

## Question 1

A user reports that an application feels slow.

What is your FIRST engineering objective?

A. Restart the application

B. Kill the highest CPU process

C. Establish the current health of the system

D. Clear system memory

---

## Question 2

Which command provides a quick overview of CPU scheduling pressure?

A. ps

B. free

C. uptime

D. kill

---

## Question 3

Why is the Linux `/proc` filesystem important?

A. It stores backups

B. It contains application source code

C. It exposes live kernel information

D. It manages user passwords

---

## Question 4

Memory shows only 60 MB free but over 500 MB available.

What is the BEST conclusion?

A. Linux is out of memory.

B. The kernel has efficiently used memory for caching.

C. The server must immediately reboot.

D. A memory leak definitely exists.

---

## Question 5

Which utility is most useful for identifying the highest CPU-consuming process?

A. vmstat

B. ps

C. free

D. kill

---

## Question 6

Why is `vmstat` valuable?

A. It creates processes.

B. It shows trends across multiple samples.

C. It formats disks.

D. It edits configuration files.

---

## Question 7

A process consumes 95% CPU.

What should happen NEXT?

A. Kill it immediately.

B. Investigate ownership, purpose, and business impact.

C. Reboot the server.

D. Clear cached memory.

---

## Question 8

Which statement best describes `htop`?

A. It replaces the Linux kernel.

B. It provides a more interactive visualization of process activity.

C. It permanently fixes CPU problems.

D. It installs monitoring software.

---

## Question 9

Which sequence represents sound engineering practice?

A. Kill → Observe → Investigate

B. Investigate → Verify → Assume

C. Observe → Gather Evidence → Confirm → Mitigate → Verify

D. Restart → Monitor → Explain

---

## Question 10

Why is verifying recovery important?

A. It proves the mitigation actually solved the problem.

B. It reduces RAM usage.

C. It removes zombie processes.

D. It restarts services automatically.

---

## Question 11

A server has high CPU utilization but almost no memory usage.

Which resource should become the primary investigation?

A. CPU

B. Memory

C. Swap

D. DNS

---

## Question 12

A server has high memory usage but CPU remains mostly idle.

Which investigation should receive greater attention?

A. CPU scheduling

B. Memory consumption

C. Network routing

D. SSH authentication

---

## Question 13

Why is a process's Parent PID valuable?

A. It identifies who created the process.

B. It increases CPU performance.

C. It encrypts system memory.

D. It reduces process priority.

---

## Question 14

Which statement best reflects professional troubleshooting?

A. Every performance issue has one cause.

B. High CPU always indicates a bad application.

C. Evidence should eliminate assumptions before mitigation.

D. Restarting services is the safest first response.

---

## Question 15

What was the most important lesson from this lab?

A. Memorize more Linux commands.

B. Investigate first, act second.

C. Always use htop instead of top.

D. Kill resource-intensive processes immediately.

---

## Scenario 16

A production database consistently appears as the highest CPU process.

What should you do FIRST?

A. Kill the database.

B. Investigate whether the workload is expected.

C. Delete cached memory.

D. Restart Linux.

---

## Scenario 17

You collect evidence from `uptime`, `top`, `free`, and `vmstat`.

The evidence conflicts.

What should you do?

A. Trust the first command you ran.

B. Continue gathering evidence before drawing conclusions.

C. Restart the server.

D. Ignore the conflicting information.

---

## Scenario 18

Why are repeated observations often better than a single measurement?

A. Linux commands become faster.

B. Trends reveal whether conditions are improving or deteriorating.

C. They reduce memory usage.

D. They eliminate CPU utilization.

---

## Scenario 19

An application is consuming significant CPU, but customers report excellent performance.

What is the BEST engineering response?

A. Continue investigating before intervening.

B. Kill the application.

C. Reboot the server.

D. Increase swap space.

---

## Scenario 20

Complete the investigation workflow.

User Report

↓

Baseline

↓

Kernel Evidence

↓

Resource Constraint

↓

_________

↓

Theory

↓

Business Impact

↓

Mitigation

↓

Verification

A. Restart

B. Identify Responsible Process

C. Install htop

D. Clear Memory
