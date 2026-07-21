# Lab 01 – Performance & Resource Monitoring

**Repository:** Linux Processes and Services

**Organization:** SirhurryUp Corporation

**Author:** Dottington Fullwood

---

# Executive Summary

Performance issues are rarely solved by memorizing commands. Successful Linux administrators investigate system behavior, collect evidence, eliminate incorrect assumptions, identify the constrained resource, and verify recovery before concluding an incident.

This lab introduces the core monitoring utilities available on nearly every Linux system while developing a structured engineering workflow for diagnosing CPU, memory, and process-related performance issues.

Rather than treating each command independently, this lab demonstrates how they work together during a real operational investigation.

---

# Business Scenario

A user contacts the Operations Team reporting that the application server has become noticeably slower.

No alerts have fired.

No obvious errors have been reported.

The engineering objective is to determine:

- Is the server actually under resource pressure?
- Which resource is constrained?
- Which process is responsible?
- Is immediate intervention necessary?
- Has the system recovered after mitigation?

---

# Engineering Objectives

By completing this lab I learned to:

- Establish a performance baseline
- Measure system load
- Inspect kernel resource information
- Monitor CPU utilization
- Evaluate memory consumption
- Observe system trends over time
- Identify CPU-intensive processes
- Investigate parent-child process relationships
- Verify recovery after mitigation

---

# Investigation Workflow

Every investigation followed the same engineering methodology.

```
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
```

---

# Investigation Timeline

## Step 1 – Establish the Baseline

### Commands

```bash
uptime
nproc
```

### Purpose

Determine whether CPU scheduling pressure exists before making assumptions about performance.

### Evidence

- Load averages were essentially zero.
- System contained two processing units.
- CPU scheduling pressure was not present.

### Engineering Decision

The initial evidence did not support a CPU bottleneck.

Additional investigation was required.

---

## Step 2 – Gather Kernel Evidence

### Commands

```bash
cat /proc/loadavg

cat /proc/meminfo

cat /proc/cpuinfo

ls /proc/$$
```

### Purpose

Understand what the Linux kernel reports before relying on monitoring utilities.

### Findings

The `/proc` filesystem exposes live kernel telemetry including:

- CPU information
- Memory statistics
- Process metadata
- Scheduler information

Monitoring utilities such as `top`, `free`, and `vmstat` simply present information collected from the kernel.

### Engineering Decision

The kernel is the authoritative source of system state.

Monitoring tools visualize kernel data rather than generating their own metrics.

---

## Step 3 – Investigate Active Processes

### Commands

```bash
top

top -b -n 3
```

### Purpose

Identify active processes and determine current CPU utilization.

### Findings

The system was largely idle.

A temporary CPU-intensive workload was intentionally generated to observe scheduler behavior.

The `top` utility revealed:

- Running processes
- CPU utilization
- Memory utilization
- Process states
- Resource consumption

### Engineering Decision

High CPU utilization alone does not justify terminating a process.

The workload must first be understood.

---

## Step 4 – Evaluate Memory and System Trends

### Commands

```bash
free -h

free -h -s 2 -c 3

vmstat 2 4
```

### Purpose

Determine whether memory or I/O pressure contributed to the reported slowdown.

### Findings

Memory remained stable throughout observation.

Available memory remained healthy.

No swap activity occurred.

`vmstat` confirmed:

- Minimal run queue
- No blocked processes
- No disk wait
- CPU remained mostly idle

### Engineering Decision

Repeated observations provide stronger evidence than a single snapshot.

Trend analysis is more valuable than isolated measurements.

---

## Step 5 – Diagnose a CPU Bottleneck

### Commands

```bash
yes > /dev/null &

uptime

ps aux --sort=-%cpu

ps -p $(pgrep yes | head -1) \
-o pid,ppid,user,%cpu,%mem,etime,cmd
```

### Purpose

Simulate a CPU-intensive workload and investigate it using production-style techniques.

### Findings

The `yes` process immediately became the largest CPU consumer.

Investigation confirmed:

- Process ID
- Parent Process ID
- Owner
- CPU utilization
- Memory utilization
- Elapsed runtime
- Executed command

### Engineering Decision

Evidence confirmed that the process intentionally generated CPU load.

The hypothesis matched the collected evidence.

---

## Step 6 – Mitigation

### Command

```bash
kill %1
```

### Purpose

Terminate the known CPU-intensive workload.

### Engineering Decision

The process was terminated because it was intentionally created for testing.

In production, process ownership and business impact would be evaluated before termination.

---

## Step 7 – Verify Recovery

### Commands

```bash
uptime

top

htop
```

### Findings

System load returned toward baseline.

CPU utilization normalized.

Memory remained stable.

No additional resource constraints were observed.

### Engineering Decision

The incident was successfully resolved and recovery verified.

---

# Commands Used

| Command | Purpose |
|----------|----------|
| uptime | Measure scheduling pressure |
| nproc | Determine available processors |
| cat /proc/* | Inspect kernel telemetry |
| top | Interactive process monitoring |
| top -b | Batch monitoring |
| free | Evaluate memory availability |
| vmstat | Observe resource trends |
| ps | Investigate processes |
| pgrep | Locate process IDs |
| kill | Terminate test workload |
| htop | Interactive visualization |

---

# Lessons Learned

This lab reinforced several important engineering concepts.

- Performance symptoms require investigation rather than assumptions.
- Monitoring tools present kernel data.
- CPU, memory, and I/O must each be evaluated independently.
- Trend analysis provides stronger evidence than isolated measurements.
- High resource consumption does not automatically indicate a faulty process.
- Every mitigation should be followed by verification.

---

# Production Considerations

In a production environment I would also investigate:

- Application logs
- System logs
- Service health
- User impact
- Recent deployments
- Scheduled jobs
- Resource limits
- Cloud monitoring dashboards
- Historical performance metrics

Commands alone rarely provide sufficient evidence to explain a business outage.

---

# Final Reflection

This lab changed the way I approach Linux troubleshooting.

Instead of asking:

> "Which command should I run?"

I learned to ask:

> "What question am I trying to answer?"

Every command became a tool for collecting evidence rather than an exercise in memorization.

That shift from executing commands to conducting investigations represents the most valuable lesson from this lab.
