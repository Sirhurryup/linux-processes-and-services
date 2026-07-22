# Lab 02 — Systemd Service Management

**Organization:** SirhurryUp Corporation  
**Environment:** Cumulai Linux Lab  
**Date:** July 22, 2026  
**Focus:** Managing Linux services with systemd

## Executive Summary

This lab explored how Linux transforms an ordinary application into a managed production service using **systemd**.

Rather than simply running a Python script, we learned how to register an application with the operating system so it could be started automatically during boot, monitored while running, restarted after failures, and managed through a consistent administrative interface.

Throughout the lab we investigated existing services, interpreted real unit files, built our own service definitions, applied production-ready configurations, and verified service behavior using `systemctl` and `journalctl`.

The most important lesson was that a running process is not the same as a managed service. Production engineering is not about keeping a single process alive—it is about ensuring the business capability remains available, even when individual processes fail.

## Business Problem

Organizations depend on applications that deliver business capabilities such as websites, APIs, authentication services, monitoring platforms, and background processing. Simply launching an application from the command line is not sufficient in a production environment because the process stops if the server reboots, the application crashes, or an administrator logs out.

Without a service manager, organizations face several operational risks:

- Applications do not start automatically after system reboots.
- Failed processes require manual intervention.
- Startup dependencies become inconsistent.
- Logging and monitoring are fragmented.
- Operational management differs from one application to another.

Linux addresses these challenges through **systemd**, a service manager responsible for starting, monitoring, restarting, and controlling applications throughout their lifecycle. Instead of managing individual processes, systemd manages the availability of the business capabilities those processes provide.

## Learning Objectives

By the end of this lab, I was able to:

- Explain the role of **systemd** as Linux's service manager.
- Distinguish between a **process** and a **service**.
- Identify and inspect running services using `systemctl`.
- Interpret the three primary sections of a systemd unit file.
- Create and configure a custom service.
- Reload systemd after modifying a unit file.
- Start, stop, enable, and disable services.
- Differentiate between a service's runtime state and its boot configuration.
- Configure automatic restart behavior for failed services.
- View and interpret service logs using `journalctl`.
- Apply production-ready service settings such as restart limits, environment variables, and working directories.
- Run a service using the principle of least privilege.
- Validate service behavior through evidence rather than assumptions.

## Why Does Systemd Exist?

Before learning individual commands, it is important to understand the business problem systemd was designed to solve.

An application running from the command line is simply a process. While that process may function correctly, it has no guarantee of starting after a system reboot, recovering from a crash, following startup dependencies, or producing centralized operational logs.

Production environments require more than a running process. They require predictable, repeatable, and manageable services that support critical business capabilities.

Systemd exists to solve these operational challenges by acting as Linux's service manager. It provides a standardized way to start, stop, monitor, restart, and organize services throughout their lifecycle.

Instead of protecting a single process, systemd protects the availability of the business capability that process provides.

## Core Concepts

Before building a service, it is important to understand the relationship between a process and a service.

### Process

A process is a running instance of a program. Every process has a unique Process ID (PID), consumes system resources, and exists only while it is actively running.

Examples of processes include:

- A Python script
- An SSH daemon
- A web server
- A database engine

Processes can start, stop, crash, or terminate unexpectedly.

---

### Service

A service is an application whose lifecycle is managed by the operating system.

Unlike a manually started process, a service can:

- Start automatically during system boot.
- Restart after failures.
- Follow startup dependencies.
- Produce centralized logs.
- Be managed through a consistent administrative interface.

Systemd is responsible for managing these services throughout their lifecycle.

---

### Systemd

Systemd is Linux's service manager.

Its responsibility is not to perform the application's work. Instead, it ensures the application is started, monitored, stopped, restarted, and made available when needed.

A helpful way to think about systemd is:

> **The application performs the work. Systemd manages the worker.**

## Mental Model: The Restaurant Manager

Imagine a busy restaurant preparing to open for the day.

The chef cannot begin cooking until the kitchen is ready.

The kitchen cannot operate until the lights are on.

The lights cannot turn on until the building has power.

The doors cannot open until the staff has arrived.

Every activity depends on something happening before it.

A restaurant manager coordinates these activities so they occur in the proper order and continues managing the restaurant throughout the day. If an employee leaves unexpectedly, the manager works to restore normal operations as quickly as possible.

Systemd plays a similar role within Linux.

Applications perform the actual work, but systemd manages when they start, the order in which they start, how they recover from failures, where they write their logs, and whether they automatically return after a reboot.

The application is the worker.

**Systemd is the manager responsible for keeping the business operating.**

## Anatomy of a Unit File

Every service managed by systemd is controlled by a **unit file**.

A unit file is a configuration file that tells systemd how a service should behave throughout its lifecycle.

Rather than viewing the file as a collection of directives, it is helpful to think of it as answering three engineering questions.

### 1. What is this service?

The **[Unit]** section provides general information about the service and its relationship to other services.

Typical directives include:

- `Description=`
- `After=`
- `Wants=`
- `Requires=`

These directives describe the service and establish startup ordering and dependencies.

---

### 2. How should this service run?

The **[Service]** section defines how systemd manages the application.

Common directives include:

- `ExecStart=`
- `Restart=`
- `RestartSec=`
- `User=`
- `Group=`
- `Environment=`
- `WorkingDirectory=`

These settings determine how the application starts, under which account it runs, how failures are handled, and what runtime environment is provided.

---

### 3. When should this service participate in the system lifecycle?

The **[Install]** section determines how the service integrates into system startup.

The most common directive is:

- `WantedBy=multi-user.target`

This tells systemd that the service should become part of the normal multi-user operating state when it is enabled.

---

Instead of memorizing directives, remember the three questions every unit file answers:

- **What is this service?**
- **How should it run?**
- **When should it start?**

## Step 1 — Investigating Running Services

### Objective

Before creating our own service, we first investigated how Linux manages services that already exist on the system.

### Command

```bash
systemctl list-units --type=service --state=running
```

### Why This Matters

Rather than immediately creating a new service, we first observed real production services already managed by systemd.

This approach mirrors good engineering practice. Before designing something new, understand how existing systems operate.

The command displayed active services such as SSH, logging services, scheduled task managers, and other background processes responsible for keeping the operating system functional.

### Evidence Observed

The output included columns similar to:

- LOAD
- ACTIVE
- SUB
- DESCRIPTION

Each column provides a different layer of operational evidence.

- **LOAD** confirms the unit file was successfully loaded.
- **ACTIVE** reports the service's overall state.
- **SUB** provides the detailed runtime condition.
- **DESCRIPTION** explains the service's purpose.

### Engineering Takeaway

This command answers one simple question:

> **What business capabilities is Linux actively managing right now?**

Before we can manage our own services, we must first understand the services Linux is already responsible for.

## Step 2 — Investigating an Existing Service

### Objective

After identifying the running services on the system, the next objective was to investigate one service in greater detail to understand how systemd reports operational health.

Rather than examining a custom application, we selected the SSH service because it is a critical component found on nearly every Linux server.

### Command

```bash
systemctl status ssh
```

### Why This Matters

When troubleshooting a production service, engineers rarely begin by reading configuration files.

The first step is to determine the current operational state of the service.

The `systemctl status` command provides a consolidated view of both configuration and runtime information, making it one of the most valuable commands for diagnosing service-related issues.

### Evidence Observed

The status output provided evidence including:

- Unit file location
- Load status
- Enablement status
- Active state
- Main Process ID (PID)
- CPU utilization
- Memory consumption
- Control Group (CGroup)
- Recent journal entries

Together, these details provide a snapshot of the service's operational health.

### Engineering Takeaway

One command answers multiple operational questions:

- Is the service installed?
- Is it currently running?
- Which process is providing the service?
- How many resources is it consuming?
- Has the service reported any recent issues?

For many production incidents, `systemctl status` is the first command an engineer should execute.

> **Principal Architect Insight**
>
> Engineering commands do not exist to produce terminal output.
> They exist to collect evidence that helps explain the current state of a system.


## Step 4 — Investigating Boot Configuration

### Objective

Determine whether a service is configured to start automatically when the operating system boots.

### Command

```bash
systemctl is-enabled ssh
```

### Why This Matters

A service being available today does not guarantee it will be available tomorrow.

Production systems experience planned maintenance, unexpected outages, and routine reboots. If a critical service is not configured to start automatically, the business capability it provides may remain unavailable until an administrator manually intervenes.

For this reason, engineers must verify both a service's current operational state and its behavior after a system restart.

### Evidence Observed

The command returned:

```text
enabled
```

This confirmed that the SSH service was configured to start automatically whenever the operating system entered its normal multi-user operating state.

### Engineering Takeaway

A service has two independent states that every engineer should verify:

- **Runtime State** — Is the service running right now?
- **Boot Configuration** — Will the service start automatically after a reboot?

Both are required to evaluate the operational readiness of a production service.

> **Principal Architect Insight**
>
> Availability is measured over time, not at a single moment. A service that only works until the next reboot is not operationally reliable.

## Step 5 — Investigating the Service Definition

### Objective

Examine the unit file to understand how systemd is instructed to start, manage, and stop a service.

### Command

```bash
cat /lib/systemd/system/ssh.service
```

### Why This Matters

Systemd does not inherently know how to manage an application. Instead, it follows instructions stored in a unit file that define the service's behavior throughout its lifecycle.

By examining the unit file, engineers can determine how a service starts, what dependencies it has, how it should recover from failures, and when it should participate in the system's startup process.

### Evidence Observed

The SSH unit file contained several key directives, including:

- `[Unit]` — Describes the service and its dependencies.
- `[Service]` — Defines how the service is started and managed.
- `[Install]` — Specifies how the service integrates into the system's boot process.

Additional directives such as `Description`, `After`, `ExecStart`, and `WantedBy` further defined the operational behavior of the service.

### Engineering Takeaway

A unit file serves as the operational blueprint for a service. Rather than manually starting applications each time the system boots, engineers define the desired behavior once, allowing systemd to consistently manage the service according to those instructions.

> **Principal Architect Insight**
>
> Reliable systems are built on explicit instructions, not assumptions. Before troubleshooting how a service behaves, first understand the configuration that defines its behavior.

## Step 6 — Investigating Service Dependencies

### Engineering Question

**What must happen before this service can start successfully?**

### Objective

Identify the dependencies and startup relationships that systemd uses to determine when a service should be started.

### Command

```bash
systemctl show ssh --property=After,Wants,Requires
```

### Why This Matters

Modern applications rarely operate in isolation. A web server may depend on networking, a database, or a logging service before it can function correctly. Starting services in the wrong order can cause startup failures, delayed availability, or inconsistent system behavior.

Systemd prevents many of these issues by defining service relationships through dependencies.

### Evidence Observed

The command displayed the dependency relationships for the SSH service, including directives such as:

- **After=** — Services or system targets that should be available before SSH starts.
- **Wants=** — Preferred dependencies that systemd attempts to start but are not strictly required.
- **Requires=** — Mandatory dependencies that must be available for the service to function correctly.

These relationships help systemd determine the proper startup sequence during system initialization.

### Engineering Takeaway

Individual services are only one part of a larger operating system ecosystem. Understanding service dependencies allows engineers to explain why a service may fail to start even when its own configuration appears correct.

Investigating dependencies is often a critical step when troubleshooting startup failures.

> **Principal Architect Insight**
>
> Production incidents are frequently caused by failed dependencies rather than failed applications. Always investigate what a service depends on before assuming the service itself is at fault.

## Step 7 — Creating a Custom Systemd Service

### Engineering Question

**How can engineers transform an ordinary application into a managed production service?**

### Objective

Create a custom systemd service that allows Linux to manage a Python application using the same lifecycle controls applied to native system services.

### Command

```bash
sudo nano /etc/systemd/system/myapp.service
```

### Why This Matters

Applications launched directly from the command line terminate when the session ends, provide limited operational visibility, and require manual intervention after system restarts.

By registering an application with systemd, engineers gain centralized lifecycle management, automated startup, logging integration, and consistent operational behavior.

This transformation is one of the fundamental differences between a development environment and a production environment.

### Evidence Observed

A new unit file named `myapp.service` was created inside:

```text
/etc/systemd/system/
```

This location allows administrators to define custom services without modifying operating system files supplied by the Linux distribution.

### Engineering Takeaway

A Python script is simply an application.

A systemd unit file transforms that application into a managed service capable of participating in the operating system's lifecycle.

This distinction is what allows businesses to operate applications reliably after maintenance windows, system reboots, and unexpected failures.

> **Principal Architect Insight**
>
> Production readiness begins when applications stop depending on human operators and start depending on well-defined operational controls.


## Step 8 — Registering the New Service

### Engineering Question

**How does systemd recognize that a new service has been added to the operating system?**

### Objective

Reload the systemd manager configuration so it discovers and registers the newly created unit file.

### Command

```bash
sudo systemctl daemon-reload
```

### Why This Matters

Systemd reads its unit files into memory. Creating or modifying a unit file on disk does not automatically update the running systemd manager.

Reloading the daemon instructs systemd to rescan its unit file directories and incorporate any new or modified service definitions without requiring a system reboot.

### Evidence Observed

The `daemon-reload` command completed successfully without producing output.

Although no confirmation message was displayed, systemd refreshed its internal configuration and became aware of the newly created `myapp.service` unit file.

### Engineering Takeaway

Not every successful engineering operation produces visible output. Engineers must understand what a command accomplishes behind the scenes and know how to verify its effects using subsequent commands.

> **Principal Architect Insight**
>
> Silent commands still produce measurable outcomes. Effective engineers verify system state rather than relying on terminal messages.

## Step 9 — Verifying Service Registration

### Engineering Question

**Did systemd successfully register the new service?**

### Objective

Confirm that systemd recognizes the newly created unit file before attempting to start or enable the service.

### Command

```bash
systemctl status myapp
```

### Why This Matters

Attempting to start or enable a service that systemd has not registered results in unnecessary troubleshooting and confusion.

Verifying registration immediately after reloading the daemon confirms that the operating system recognizes the service definition before additional configuration is performed.

### Evidence Observed

The `systemctl status` command displayed the newly registered service.

The output confirmed:

- The service name (`myapp.service`)
- The location of the unit file
- Its current load state
- Its current active state (inactive prior to startup)

This demonstrated that systemd successfully discovered the service definition during the daemon reload.

### Engineering Takeaway

Registration and execution are separate phases.

A service can be successfully registered with systemd even if it has never been started.

Understanding this distinction helps engineers isolate configuration problems from runtime problems during troubleshooting.

> **Principal Architect Insight**
>
> Verify every major change before introducing the next one. Layering multiple changes without verification makes troubleshooting significantly more difficult.

## Step 10 — Starting the Service

### Engineering Question

**Can systemd successfully launch and manage the new service?**

### Objective

Start the custom service and verify that systemd can execute the application using the instructions defined in the unit file.

### Command

```bash
sudo systemctl start myapp
```

### Why This Matters

Creating and registering a unit file does not automatically execute the application. Starting the service is the first operational test of the configuration and validates whether systemd can successfully interpret the unit file and launch the application.

This step also reveals common configuration issues, such as incorrect file paths, invalid permissions, missing dependencies, or syntax errors within the unit file.

### Evidence Observed

The `start` command completed without displaying output.

A successful execution indicated that systemd accepted the request to launch the service. The actual operational state was verified in the following step using service status information.

### Engineering Takeaway

Starting a service is not the same as proving it is healthy.

A successful start request simply means the operating system attempted to launch the service. Engineers must always perform a separate verification to confirm the service entered the expected operational state.

> **Principal Architect Insight**
>
> Never confuse an accepted command with a successful outcome. Reliable engineering requires verifying results, not assuming them.

## Step 11 — Verifying Service Execution

### Engineering Question

**Is the service operating successfully after being started?**

### Objective

Verify that the custom service is running correctly and collect operational evidence about its current state.

### Command

```bash
systemctl status myapp
```

### Why This Matters

Starting a service only submits a request to systemd. Engineers must verify whether the service actually entered the expected operational state.

Service status provides valuable operational evidence, including the current state, process identifier (PID), execution history, resource usage, and recent log messages. This information is often the first place engineers investigate when troubleshooting production services.

### Evidence Observed

The service status confirmed:

- The unit file was successfully loaded.
- The service entered the **active (running)** state.
- A Process ID (PID) was assigned.
- The service was executing under systemd management.
- Recent journal entries reflected the service startup process.

These observations confirmed that systemd successfully launched and was actively managing the application.

### Engineering Takeaway

Successful service management is demonstrated through evidence, not assumptions. A running service should always be validated by examining its operational state rather than relying solely on the absence of error messages.

> **Principal Architect Insight**
>
> Every successful deployment deserves a verification step. Trust evidence more than expectations.

## Step 12 — Enabling Automatic Startup

### Engineering Question

**Will the service start automatically after the operating system reboots?**

### Objective

Configure the custom service to start automatically during the system boot process.

### Command

```bash
sudo systemctl enable myapp
```

### Why This Matters

Production systems must recover from planned maintenance, security updates, power interruptions, and unexpected failures with minimal manual intervention.

Enabling a service ensures that systemd includes it in the operating system's startup sequence, allowing the application to become available automatically after each reboot.

This reduces operational risk and improves service availability.

### Evidence Observed

The command created the appropriate symbolic link between the service's unit file and the system's startup target.

This confirmed that `myapp.service` was now registered to participate in the normal boot sequence.

### Engineering Takeaway

A running service provides availability today.

An enabled service provides availability tomorrow.

Reliable systems require both.

> **Principal Architect Insight**
>
> Operational maturity is measured by how well a system recovers without human intervention. Automation is a cornerstone of reliability.

## Step 13 — Investigating Service Logs

### Engineering Question

**What evidence explains the service's behavior over time?**

### Objective

Examine the service journal to investigate startup events, operational messages, warnings, and errors generated by the service.

### Command

```bash
journalctl -u myapp
```

### Why This Matters

A service's current status provides a snapshot of its present condition, but it does not explain how the service reached that state.

The system journal records the service's operational history, allowing engineers to reconstruct events, identify failures, validate successful startups, and investigate unexpected behavior.

Logs provide the historical evidence required for effective troubleshooting.

### Evidence Observed

The journal displayed entries generated by `myapp.service`, including:

- Service startup events
- Application output
- Error or warning messages (if present)
- Service stop and restart events
- Timestamps documenting the sequence of operations

These entries established a chronological record of the service's lifecycle.

### Engineering Takeaway

Status answers **what is happening now**.

Logs answer **what happened over time**.

Both perspectives are required to understand system behavior and accurately diagnose production issues.

> **Principal Architect Insight**
>
> Good engineers remember what they changed. Great engineers can prove what the system experienced.

# Engineering Lessons Learned

Throughout this lab, service management evolved from a collection of Linux commands into a structured engineering workflow.

Key lessons included:

- A running process is not the same as a managed service.
- Systemd provides lifecycle management rather than application functionality.
- Runtime state and boot configuration answer different operational questions.
- Unit files define how services should behave throughout their lifecycle.
- Dependencies influence service availability and startup sequencing.
- Registering a service is different from starting a service.
- Every configuration change should be verified before introducing the next.
- Operational evidence is gathered through status information and system logs.
- Reliable systems depend on automation rather than manual intervention.
- Engineering decisions should always be supported by observable evidence.

# Engineering Principles Earned

1. Every technology exists to solve a business problem.

2. Commands gather evidence; engineers interpret evidence.

3. Configuration expresses intent. Verification confirms behavior.

4. Automation improves operational reliability.

5. Services should recover without requiring human intervention.

6. Separate runtime health from startup configuration.

7. Logs preserve the operational history of a system.

8. Investigate before modifying a production system.

9. Validate every significant engineering change.

10. Engineering judgment is built through disciplined observation and interpretation.

# Reflection

## What changed in the way I think today?

Before this lab, I viewed systemd primarily as a collection of commands used to start and stop Linux services.

After completing this lab, I understand that systemd is an operational framework responsible for managing the lifecycle of business-critical applications. I now recognize the importance of distinguishing between configuration and verification, runtime state and startup behavior, and assumptions and evidence.

Rather than memorizing commands, I learned to approach service management by asking engineering questions, collecting evidence, and interpreting the results before making decisions.

This shift in thinking will influence how I investigate Linux systems, design cloud architectures, and troubleshoot production environments in the future.

