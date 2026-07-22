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


