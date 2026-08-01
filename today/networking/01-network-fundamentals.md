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

### ICMP (Internet Control Message Protocol)

ICMP is a network diagnostic protocol used to test reachability and report network conditions.

Unlike HTTP, SSH, or FTP, ICMP is **not** used to transfer application data.

Instead, it answers questions such as:

- Is the destination reachable?
- Can the host respond?
- Did the packet arrive successfully?

The `ping` command uses ICMP Echo Requests and Echo Replies.

**Engineering Question**

> Can the destination respond to an ICMP Echo Request?

**Important Distinction**

A failed ICMP test does **not** prove that web applications are unavailable.

Many production environments intentionally block ICMP while allowing HTTPS, SSH, and other application traffic.

## ICMP vs HTTP

These protocols answer different engineering questions.

| Protocol | Question It Answers |
|----------|---------------------|
| ICMP | Can the host respond to a network echo request? |
| HTTP/HTTPS | Can I communicate with the web application? |

A server may ignore ICMP requests while fully supporting HTTPS connections.

Therefore:

- A failed `ping` does not prove the website is unavailable.
- A successful `curl` demonstrates successful application-layer communication.

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

Different protocols validate different capabilities.

Examples:

- `ping` validates ICMP communication.
- `curl` validates HTTP/HTTPS communication.
- `dig` validates DNS name resolution.
- `ss` validates whether services are accepting incoming connections.

Always choose the diagnostic tool that matches the business capability being tested.

---

# Consulting Perspective

The business reports that "the application cannot reach the external API."

A network engineer translates that into technical questions instead of making assumptions.

Evidence is collected one layer at a time until the failure domain is isolated.

# Repository Notes

## Investigation Framework

Every networking investigation begins with seven questions:

1. Who is experiencing the problem?
2. What business capability are they trying to accomplish?
3. What does the reported symptom actually mean?
4. Where should the traffic go?
5. How should the traffic get there?
6. What evidence supports the failure?
7. Which networking component is preventing communication?

---

## Four Failure Domains

Every networking problem eventually traces back to one or more of these areas:

- Addressing
- Routing
- Name Resolution
- Connectivity

Every diagnostic command exists to eliminate one of these possibilities.

---

## Evidence Over Assumptions

A failed test proves only the capability being tested.

Examples:

- Failed `ping` proves ICMP communication was unsuccessful.
- Successful `dig` proves DNS resolution succeeded.
- Successful `curl` proves application-layer communication succeeded.

Avoid drawing conclusions beyond the evidence.
