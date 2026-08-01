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

# Vocabulary

### Network Interface

A physical or virtual network connection through which a computer sends and receives network traffic.

---

### IP Address

A logical address assigned to a network interface that uniquely identifies a device on a network.

---

### Loopback Interface (lo)

A virtual interface used by a computer to communicate with itself. It always uses the address `127.0.0.1` and never leaves the machine.

---

### Gateway (Default Gateway)

The router that forwards traffic from the local network to other networks when the destination is not local.

---

### Routing Table

A list of routes used by Linux to determine how packets reach their destination.

---

### Route

A path that tells the operating system where to send network traffic.

---

### DNS (Domain Name System)

A distributed service that translates human-readable names (such as `google.com`) into IP addresses.

---

### Resolver

The DNS server that performs hostname lookups on behalf of a client.

---

### Name Resolution

The process of converting a hostname into an IP address.

---

### A Record

A DNS record that maps a hostname to an IPv4 address.

---

### Packet

The basic unit of data transmitted across a network.

---

### ICMP (Internet Control Message Protocol)

A network diagnostic protocol used to determine whether another host can respond to an ICMP Echo Request.

ICMP does not transfer application data.

It is primarily used for:

- Reachability testing
- Network diagnostics
- Error reporting

The `ping` command uses ICMP.

Important:

A failed ICMP test does **not** prove that HTTP, HTTPS, SSH, or other application protocols are unavailable.

### Ping

A diagnostic utility that uses ICMP to determine whether another host responds to network echo requests.

---

### HTTP

The Hypertext Transfer Protocol used by web applications to exchange requests and responses.

Unlike ICMP, HTTP transfers application data between clients and servers.

The `curl` command is commonly used to test HTTP and HTTPS communication.

---

### HTTPS

The encrypted version of HTTP that protects communication using TLS (Transport Layer Security).

HTTPS is the standard protocol used by secure web applications.

---

### Curl

A command-line utility used to communicate with application-layer protocols such as HTTP and HTTPS.

---

### Socket

A communication endpoint used by an application to send or receive network traffic.

---

### Listening Socket

A socket that is actively waiting for incoming client connections.

---

### Port

A logical communication endpoint that identifies a specific application or service running on a host.

---

### SSH (Secure Shell)

A protocol used to securely administer remote Linux systems. By default, SSH listens on TCP port 22.

---

### Process

A running instance of a program.

In networking, a process owns one or more sockets used for communication.

---

### Process Ownership

The relationship between a network socket and the application responsible for that socket.

The `ss -p` option displays the owning process.



