# Lab 04 – Host Firewall Security (UFW)

# Objective

Design, implement, verify, and manage a production-ready host firewall using Ubuntu's Uncomplicated Firewall (UFW) while understanding how firewall policy protects business services.

---

# Business Problem

Every server connected to a network is continuously scanned by automated bots and malicious actors searching for exposed services.

Without a firewall, every exposed service becomes a potential attack vector.

The engineer's responsibility is to expose only the services required by the business while denying everything else.

---

# Core Architecture

```
                     Internet
                         │
         ┌───────────────┴───────────────┐
         │                               │
   Legitimate Users               Automated Scanners
         │                               │
         └───────────────┬───────────────┘
                         │
                 Ubuntu Firewall (UFW)
                         │
        ┌────────────────┼─────────────────┐
        │                │                 │
     SSH (22)        HTTP (80)       HTTPS (443)
       Allow           Allow            Allow
                         │
                    Web Server

                Port 9229 (Debug)
                     Explicit DENY

           PostgreSQL (5432)
      Allow ONLY from 10.0.1.0/24

```
---

# Engineering Workflow

Business Requirement

↓

Determine Required Services

↓

Establish Default Security Policy

↓

Allow Business Traffic

↓

Restrict Administrative Access

↓

Restrict Internal Services

↓

Explicitly Block Dangerous Services

↓

Verify Rule Set

↓

Maintain Least Privilege

---

# Firewall Decision Tree

```
Incoming Connection

        │

        ▼

Is there a matching rule?

        │
   ┌────┴────┐
   │         │
 Yes        No
   │         │
   ▼         ▼
Apply     Apply Default
Rule       Policy
   │
   ▼
ALLOW or DENY
```

---

# Commands to Understand

## `sudo ufw status`

Displays whether the firewall is active or inactive.

**Engineering Question**

> Is the firewall currently protecting this server?

---

## `sudo ufw status verbose`

Displays:

- Firewall status
- Default policies
- Active firewall rules

**Engineering Question**

> What security policy is protecting this server?

---

## `sudo ufw status numbered`

Displays firewall rules with line numbers.

**Engineering Question**

> Which specific firewall rule needs to be modified or removed?

---

## `sudo ufw enable`

Enables the firewall and configures it to start automatically at boot.

**Engineering Question**

> Is firewall protection currently enforced?

---

## `sudo ufw disable`

Disables firewall enforcement.

**Engineering Question**

> Is all network traffic currently unrestricted?

---

## `sudo ufw allow 80/tcp`

Allows incoming HTTP traffic on TCP port 80.

**Breakdown**

- `allow` = Permit traffic
- `80` = HTTP port
- `tcp` = TCP protocol

**Engineering Question**

> Which business service should be reachable from the network?

---

## `sudo ufw allow 443/tcp`

Allows incoming HTTPS traffic.

**Breakdown**

- `443` = HTTPS
- `tcp` = TCP protocol

**Engineering Question**

> Is secure web traffic allowed?

---

## `sudo ufw allow 22/tcp`

Allows SSH administrative access.

**Breakdown**

- `22` = SSH
- `tcp` = TCP protocol

**Engineering Question**

> Can administrators remotely manage this server?

---

## `sudo ufw deny 9229/tcp`

Explicitly blocks access to the Node.js debugging port.

**Breakdown**

- `deny` = Reject traffic
- `9229` = Node.js Debug Port
- `tcp` = TCP protocol

**Engineering Question**

> Which service should never be exposed to users?

---

## `sudo ufw allow from 10.0.1.0/24 to any port 5432`

Allows PostgreSQL connections only from the application subnet.

**Breakdown**

- `allow` = Permit traffic
- `from` = Source network
- `10.0.1.0/24` = Trusted subnet
- `to any port` = Destination port
- `5432` = PostgreSQL

**Engineering Question**

> Who is authorized to access this database?

---

## `sudo ufw delete <rule-number>`

Deletes a firewall rule by its rule number.

**Engineering Question**

> How do I safely remove one firewall rule without affecting others?

---

## `sudo ufw delete allow from 10.0.1.0/24 to any port 5432`

Deletes a firewall rule by specifying the entire rule instead of its number.

**Engineering Question**

> How can I remove a rule even if its number changes?

---

## `sudo ufw reset`

Removes all firewall rules and returns UFW to its default state.

---

**Engineering Question**

> How do I rebuild the firewall from a clean configuration?

---

# Engineering Distinctions

## Firewall vs Service

Running nginx does not make it reachable.

The firewall determines whether clients may communicate with it.

---

## Default Policy vs Explicit Rule

The default policy applies only when no rule matches.

Explicit rules always take precedence.

---

## Allow vs Deny

Allow permits communication.

Deny rejects communication.

---

## Anywhere vs Source Restricted

```
Anywhere

Internet
   │
   ▼
Server
```

Anyone may connect.

---

```
10.0.1.0/24

Application Tier

        │

        ▼

Database
```

Only trusted systems may connect.

---

## Host Firewall vs Cloud Firewall

```
Internet

    │

Security Group

    │

Operating System

    │

UFW

    │

Application
```

Both layers must permit traffic.

Blocking at either layer prevents communication.

---

# Evidence Collected

- Firewall initially inactive.
- SSH allowed before firewall activation.
- Default incoming policy set to DENY.
- HTTP and HTTPS explicitly allowed.
- Debug port explicitly denied.
- PostgreSQL restricted to the application subnet.
- Rule deletion verified by number.
- Production-ready rule set confirmed.

---

# Engineering Principles

## Principle 1

Default Deny.

Allow only the minimum traffic required by the business.

---

## Principle 2

Least Privilege.

Grant only the minimum network access necessary.

---

## Principle 3

Defense in Depth.

Protect services with multiple security layers.

Example:

Internet

↓

AWS Security Group

↓

UFW

↓

Application Authentication

---

## Principle 4

Business Capability Drives Firewall Rules.

Open ports because the business requires them—not because developers request them.

---

## Principle 5

Administrative Access Should Always Be Restricted.

SSH should never be open to everyone unless absolutely necessary.

---

## Principle 6

Internal Services Should Never Be Internet Facing.

Databases belong behind application servers.

---

## Principle 7

Document Intent.

Explicit DENY rules communicate security intent to future engineers.

---

# Consulting Perspective

A firewall is not simply a list of ports.

It is the organization's security policy translated into technical controls.

Every rule should answer one business question:

"What business capability requires this communication?"

If no answer exists,

the rule should not exist.

---

# Vocabulary

### Firewall

A security control that permits or denies network traffic according to defined rules.

---

### UFW (Uncomplicated Firewall)

Ubuntu's simplified interface for configuring the Linux Netfilter firewall.

---

### iptables

The Linux userspace utility used to configure Netfilter firewall rules.

UFW builds iptables rules automatically.

---

### Netfilter

The packet filtering framework built into the Linux kernel.

---

### Rule

A condition evaluated against incoming or outgoing traffic.

---

### Policy

The default action taken when no rule matches.

---

### Default Deny

A security model that blocks all traffic unless explicitly permitted.

---

### Least Privilege

Providing only the minimum permissions required to perform a task.

---

### Defense in Depth

Applying multiple independent security controls so that failure of one layer does not expose the system.

---

### CIDR

Classless Inter-Domain Routing.

A notation used to describe IP networks.

Example:

10.0.1.0/24

Represents

10.0.1.0

through

10.0.1.255

---

### Attack Surface

The collection of services that an attacker can potentially reach.

Reducing exposed services reduces attack surface.

---

### Source Restriction

Limiting access based upon the originating IP address or subnet.

---

### Rule Ordering

The sequence in which firewall rules are evaluated.

Understanding rule order is essential when managing traditional firewalls such as iptables.

---

# Interview Language

"When designing host firewalls, I begin with a default-deny posture and explicitly allow only the services required by the business. Administrative interfaces are restricted to trusted sources, internal services are isolated to application networks, and unnecessary services remain blocked. My objective is to minimize attack surface while preserving required business functionality."

---

# Repository Notes

## Security Mindset

Never ask:

> Which ports should I open?

Instead ask:

> Which business capabilities require communication?

---

## Engineering Memory Aid

```
Service

↓

Firewall

↓

Client
```

No service.

No communication.

Firewall blocks.

No communication.

Only when **both** are true does communication succeed.

---

## AWS Connection

| AWS Security Group | UFW |
|--------------------|-----|
| Hypervisor Firewall | Host Firewall |
| Instance Level | Operating System Level |
| Stateful | Stateful |
| Controls EC2 Traffic | Controls Linux Traffic |

Production traffic must successfully pass **both** security layers.
