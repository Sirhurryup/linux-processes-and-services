lab-03-package-management/

# Lab 03 – Linux Package Management

# Objective

Learn how Linux installs, verifies, updates, and removes software using Ubuntu's package management system while understanding the difference between system packages and language-specific packages.

---

# Business Problem

A development team requires a new Ubuntu server configured with:

- Nginx for web serving
- Flask for a Python API

The engineer must install the required software, verify it functions correctly, and remove unnecessary software to reduce security risk and operational overhead.

---

# Core Architecture

                 Ubuntu Repository
                         │
                         ▼
                   apt Package Manager
                         │
      ┌──────────────────┴──────────────────┐
      ▼                                     ▼
 System Packages                     Python Packages
 (nginx, curl, git)              (Flask, requests, boto3)
      │                                     │
      ▼                                     ▼
 Installed Software                  Python Runtime

---

# Engineering Workflow

Business Requirement

↓

Refresh Package Catalog

↓

Search Available Software

↓

Install Required Package

↓

Verify Installation

↓

Verify Service

↓

Verify Business Function

↓

Remove Unneeded Software

↓

Remove Orphaned Dependencies

---

# Commands to Understand

## sudo apt update

Refreshes the package catalog from Ubuntu repositories.

Engineering Question

> Does this server have the latest package information?

---

## apt search

Searches package names and descriptions.

Engineering Question

> Is the software available?

---

## apt show

Displays detailed package information.

Engineering Question

> What does this package provide?

---

## apt install

Downloads, verifies, installs, and resolves dependencies.

Flags

- y = Automatically answer "Yes" to prompts.

Engineering Question

> How do I install trusted software?

---

## apt list --installed

Displays installed system packages.

Engineering Question

> What software exists on this server?

---

## apt remove

Removes an installed package while preserving configuration files.

Engineering Question

> How do I uninstall software while preserving configuration?

---

## apt purge

Removes both the package and its configuration files.

Engineering Question

> How do I completely remove software?

---

## apt autoremove

Removes orphaned dependencies.

Engineering Question

> Which packages are no longer required?

---

## pip3 install

Installs Python packages.

Engineering Question

> How do I install Python libraries?

---

## pip3 list

Displays installed Python packages.

Engineering Question

> Which Python libraries are installed?

---

## pip3 show

Displays detailed package information.

Engineering Question

> What version and dependencies does this Python package have?

---

## systemctl start

Starts a Linux service.

Engineering Question

> Is the application running?

---

## systemctl status

Displays the status of a Linux service.

Engineering Question

> Is the service healthy?

---

## curl localhost

Tests the web application.

Engineering Question

> Can the application serve client requests?

---

# Engineering Distinctions

- Installing software is different from starting a service.
- Installing software is different from verifying business functionality.
- apt manages operating system packages.
- pip manages Python packages.
- Removing software does not necessarily remove configuration.
- Verifying installation is different from verifying usability.

---

# Evidence Collected

- Package catalog successfully refreshed.
- nginx located within Ubuntu repositories.
- nginx installed successfully.
- nginx service running.
- HTTP response successfully returned.
- Flask installed through pip3.
- Flask successfully imported into Python.
- nginx removed.
- Orphaned dependencies removed.

---

# Engineering Principles

- Always refresh the package catalog before installing software.
- Install software only from trusted repositories.
- Verify installation before declaring success.
- Verify business functionality—not just installation.
- Remove unnecessary software to reduce attack surface.
- Clean orphaned dependencies after removing packages.
- Use the appropriate package manager for the technology.

---

# Consulting Perspective

Installing software is only one phase of server provisioning.

A consultant verifies:

1. Installation
2. Service availability
3. Business functionality
4. Security posture
5. System cleanliness

Successful installation alone does not satisfy the business requirement.

---

# Vocabulary

### Package

A collection of software files managed as a single installable unit.

---

### Package Manager

Software that installs, updates, verifies, and removes packages.

---

### Repository

A trusted server that stores verified software packages.

---

### Dependency

A software component required by another package.

---

### Package Catalog

A local index of available software maintained by the package manager.

---

### apt

Ubuntu's package manager for operating system software.

---

### pip

Python's package manager for Python libraries.

---

### System Package

Software installed for the operating system.

Examples:

- nginx
- git
- curl

---

### Python Package

A library installed for Python development.

Examples:

- Flask
- boto3
- requests

---

### Service

A background application managed by systemd.

---

### Configuration File

A file that stores application settings.

---

### Orphaned Dependency

A package no longer required because the software depending on it has been removed.

---

### Attack Surface

The collection of software and services that could potentially be exploited by an attacker.

---

# Interview Language

"When provisioning Linux servers, I follow a repeatable workflow: update the package catalog, install software from trusted repositories, verify the service is operational, validate the business functionality, and remove unnecessary software along with orphaned dependencies to maintain a secure and lean system."
