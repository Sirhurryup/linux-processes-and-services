# Lab 02 – Sockets, Packets, and Firewall Rules

# Objective

Understand how to troubleshoot network communication by verifying:

1. The service is listening.
2. Packets are reaching the host.
3. The firewall allows the traffic.

This three-layer methodology forms the foundation of network troubleshooting.

---

# Business Problem

Users report that an application is unreachable.

Before restarting services or changing firewall rules, determine where communication is failing by collecting evidence.

Successful communication requires three conditions:

- The application must be listening.
- Packets must reach the host.
- Security controls must allow the traffic.

---

# Core Architecture

```
            Client
               │
               ▼
      Listening Socket
            (ss)
               │
               ▼
        Network Packets
         (tcpdump)
               │
               ▼
     Kernel Firewall Rules
         (iptables)
               │
               ▼
        Application Process
```

---

# Engineering Workflow

Business Problem

↓

Verify Listening Service (`ss`)

↓

Verify Packet Arrival (`tcpdump`)

↓

Verify Firewall Rules (`iptables`)

↓

Determine Root Cause

---

# Commands to Understand

## ss -tlnp

Displays listening TCP sockets and the process that owns them.

Flags

- t = TCP
- l = Listening sockets
- n = Numeric ports and addresses
- p = Owning process

Engineering Question

> Is the service listening and which process owns the socket?

---

## ss -tnp state established

Displays active TCP sessions.

Engineering Question

> Is anyone connected right now?

Important

LISTEN and ESTABLISHED are different TCP states.

---

## tcpdump -i lo -n -c 6

Captures packets on the loopback interface.

Flags

- i = Interface
- lo = Loopback interface
- n = Numeric addresses
- c = Number of packets

Engineering Question

> Are packets actually arriving at the interface?

---

## tcpdump -w capture.pcap

Writes captured packets to a file.

Engineering Question

> Can I preserve traffic for later analysis?

---

## tcpdump -r capture.pcap

Reads packets from a saved packet capture.

Engineering Question

> What happened during the captured communication?

---

## iptables -L -n -v

Lists firewall rules.

Flags

- L = List rules
- n = Numeric addresses
- v = Verbose

Engineering Question

> Is the firewall permitting or denying traffic?

---

## iptables -A

Append a rule to the end of a chain.

---

## iptables -I

Insert a rule at a specific position.

---

## iptables -D

Delete a firewall rule.

---

# Engineering Distinctions

- A process is not a socket.
- A socket is not a network packet.
- A listening service is not an active connection.
- Capturing packets proves traffic reached the interface.
- Firewall rules are evaluated from top to bottom.
- First matching firewall rule wins.
- A DROP rule placed before an ACCEPT rule prevents later rules from being evaluated.

---

# Evidence Collected

- SSH listening on port 22.
- Python HTTP server listening on port 8080.
- No ESTABLISHED connections before client traffic.
- HTTP connection completed too quickly to remain ESTABLISHED.
- tcpdump captured the TCP handshake and HTTP request.
- Packet capture saved successfully as a `.pcap` file.
- iptables controls whether packets are accepted or dropped.

---

# Engineering Principles

- Troubleshoot communication from the inside out.
- Verify the application before investigating the network.
- Packet captures only collect traffic while the capture is running.
- Start the packet capture before reproducing the problem.
- Different tools answer different engineering questions.
- Firewall rules are processed sequentially.
- The first matching rule determines the outcome.

---

# Consulting Perspective

When a service is unreachable:

1. Verify the service is listening.
2. Verify packets reach the server.
3. Verify firewall policy.

Collect evidence before making configuration changes.

---

# Vocabulary

### Socket

A communication endpoint used by an application to send or receive network traffic.

---

### Listening Socket

A socket waiting for incoming client connections.

---

### ESTABLISHED

A TCP state indicating an active communication session between a client and server.

---

### Packet

The basic unit of data transmitted across a network.

---

### Packet Capture (PCAP)

A recorded collection of network packets used for troubleshooting and forensic analysis.

---

### tcpdump

A command-line packet analyzer used to capture and inspect network traffic.

---

### Loopback Interface (lo)

A virtual interface used for communication within the local system.

---

### Firewall

A security mechanism that permits or denies network traffic according to defined rules.

---

### iptables

The Linux userspace utility used to configure the kernel Netfilter firewall.

---

### Chain

An ordered list of firewall rules evaluated sequentially.

Examples:

- INPUT
- OUTPUT
- FORWARD

---

### Rule Order

The sequence in which firewall rules are evaluated.

The first matching rule determines the action taken.

---

### ACCEPT

Allows matching traffic to continue.

---

### DROP

Silently discards matching traffic without notifying the sender.

---

# Interview Language

"When troubleshooting a connectivity issue, I follow a structured workflow. I first verify that the service is listening using `ss`, then confirm that packets reach the host with `tcpdump`, and finally inspect firewall policy using `iptables`. This evidence-driven approach isolates the failure before making changes."
