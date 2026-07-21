# Linux Processes and Services

**Organization:** SirhurryUp Corporation

## Overview

Modern Linux administrators do far more than memorize commands. They investigate problems, gather evidence, validate assumptions, and make decisions that protect both the operating system and the business it supports.

This repository documents my journey learning Linux process management, system services, and operational troubleshooting from the perspective of a Cloud Engineer and Solutions Architect.

Rather than focusing on command memorization, each lab emphasizes engineering thinking, structured investigations, and evidence-based decision making.

---

# Repository Goals

This repository demonstrates my ability to:

- Monitor Linux system performance
- Investigate CPU, memory, and I/O bottlenecks
- Analyze running processes
- Understand process relationships
- Manage Linux services with systemd
- Troubleshoot production-style incidents
- Apply engineering principles to real-world operational problems

Every lab is treated as an engineering engagement rather than a command exercise.

---

# Engineering Methodology

Each investigation follows the same workflow.

Incident Report
        │
        ▼
Establish Baseline
        │
        ▼
Gather Kernel Evidence
        │
        ▼
Identify Resource Constraint
        │
        ▼
Identify Responsible Process
        │
        ▼
Observe Trends
        │
        ▼
Confirm Theory
        │
        ▼
Assess Business Impact
        │
        ▼
Mitigate
        │
        ▼
Verify Recovery
        │
        ▼
Document Lessons Learned

This investigative workflow helps reduce assumptions, encourages evidence-based troubleshooting, and improves operational decision making.

---

# Repository Structure

```

```text
linux-processes-and-services/
│
├── README.md
│
├── labs/
│   ├── 01-performance-resource-monitoring.md
│   ├── 02-systemd-services.md
│   ├── 03-load-is-climbing.md
│   └── 04-resilient-systemd-service.md
│
├── mental-models/
│   └── linux-performance-investigation-framework.md
│
├── principles/
│   └── principles-earned.md
│
├── assessments/
│   └── lab-01-performance-monitoring-assessment.md
│
└── medium/
    └── article-outline.md
```

```markdown
---

# Skills Demonstrated

## Linux Monitoring

- uptime
- top
- htop
- free
- vmstat
- ps

## Process Investigation

- CPU utilization
- Memory utilization
- Parent and child processes
- Process ownership
- Process lifetime
- Process identification

## Operational Thinking

- Performance monitoring
- Incident investigation
- Root cause analysis
- Verification of recovery
- Business impact assessment

---

# Engineering Principles

Throughout this repository I focus on one central belief:

> Great administrators do not memorize commands. They build repeatable investigative processes that consistently lead to the correct engineering decision.

Every command answers a question.

Every question produces evidence.

Every piece of evidence supports an engineering decision.

---

# About SirhurryUp Corporation

SirhurryUp Corporation approaches cloud engineering, Linux administration, and systems architecture from a consulting perspective.

Every project answers four questions:

1. What business problem exists?
2. What technical evidence supports the diagnosis?
3. What engineering decision should be made?
4. How do we verify success?

Technology is only valuable when it improves outcomes for the people and organizations depending on it.

---

# Continuous Improvement

This repository will continue expanding with additional Linux investigations, service management scenarios, production troubleshooting exercises, and engineering retrospectives as my cloud engineering journey progresses.


