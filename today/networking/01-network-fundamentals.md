# Lab 01 – Network Fundamentals

## Objective

Understand the core networking components that allow a Linux server to communicate with other systems and learn the primary diagnostic tools used to investigate connectivity problems.

---

# Business Problem

A developer reports that an application cannot communicate with an external API.

Before making configuration changes, engineers must determine whether the failure involves:

- Addressing
- Routing
- Name Resolution
- Connectivity

The objective is to collect evidence before identifying the root cause.

---

# Core Architecture

```
Application
      │
      ▼
Linux Network Stack
      │
      ▼
Network Interface
      │
      ▼
Default Gateway
      │
      ▼
DNS Resolver
      │
      ▼
Destination Server
```

---

# Engineering Workflow

Business Problem

↓

Identify Interfaces

↓

Verify IP Address

↓

Verify Routing

↓

Verify Name Resolution

↓

Verify Connectivity

↓

Verify Listening Services

↓

Determine Root Cause

---

# Commands to Understand

### ip addr

Displays network interfaces and assigned IP addresses.

Engineering Question

> Who am I on the network?

---

### ip route

Displays the routing table.

Engineering Question

> How does Linux decide where to send packets?

---

### ping

Tests ICMP connectivity.

Flags

- **c** = Number of echo requests to send

Engineering Question

> Can the destination respond to ICMP?

---

### curl

Transfers data using application-layer protocols such as HTTP and HTTPS.

Flags

- **-I** = Retrieve headers only

Engineering Question

> Can I successfully communicate with the application?

---

### ss -tuln

Displays listening network sockets.

Flags

- **t** = TCP
- **u** = UDP
- **l** = Listening sockets
- **n** = Numeric addresses and ports

Engineering Question

> What services are accepting incoming connections?

---

### dig

Queries DNS.

Engineering Question

> Can this server resolve hostnames into IP addresses?

---

# Evidence Collected

- Multiple interfaces identified (`lo`, `eth0`, `eth1`)
- Production interface identified as `eth1`
- Default gateway: `10.0.1.1`
- DNS successfully resolved `google.com`
- ICMP requests failed
- HTTPS requests succeeded (`HTTP/2 200`)
- SSH service listening on port 22

---

# Engineering Distinctions

- An interface is not an IP address.
- A route is not a destination.
- DNS resolves names—it does not provide connectivity.
- ICMP is different from HTTP.
- Listening services do not guarantee successful connectivity.

---

# Engineering Principles

- Every networking problem traces back to:
  - Addressing
  - Routing
  - Name Resolution
  - Connectivity

- Collect evidence before making changes.

- One failed test proves one failed capability.

- Different protocols validate different capabilities.

---

# Consulting Perspective

The business reports that "the application cannot reach the external API."

A network engineer translates that into technical questions instead of making assumptions.

Evidence is collected one layer at a time until the failure domain is isolated.
