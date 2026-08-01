lab-05-network-incident-response/
# Lab 05 – Network Incident Response

# Objective

Develop a structured incident response methodology for diagnosing and restoring network connectivity by separating application failures from network and firewall failures.

The objective is not simply to "fix the problem," but to collect evidence, identify the root cause, implement the correct fix, and verify the business outcome.

---

# Business Problem

The Service Desk escalates an incident:

> "The internal status site is unavailable. Browsers and curl requests to http://localhost:8080 hang or eventually time out. The application process is running."

Initial evidence suggests:

- The application is healthy.
- Users still cannot access the service.

Your responsibility is to determine **why communication is failing** without making assumptions.

---

# Core Architecture

```
                     Client

                       │

                 HTTP Request

                       │

                 TCP Port 8080

                       │

             Linux Firewall (iptables)

                       │

              Listening Socket (ss)

                       │

          systemd Service (sitehttp.service)

                       │

                 Static Website

                 CUMULAI-OK
```

---

# Incident Response Workflow

```
Business Complaint

        │

        ▼

Reproduce the Failure

        │

        ▼

Collect Evidence

        │

        ▼

Develop Theory

        │

        ▼

Validate Theory

        │

        ▼

Implement Fix

        │

        ▼

Verify Business Outcome

        │

        ▼

Close Incident
```

---

# Engineering Workflow

Customer reports outage

↓

Verify service health

↓

Reproduce customer experience

↓

Verify listening socket

↓

Inspect firewall

↓

Identify root cause

↓

Remove blocker

↓

Verify business functionality

↓

Close incident

---

# Commands to Understand

## `systemctl status sitehttp.service --no-pager`

Displays the operational status of the web service.

**Engineering Question**

> Is the application actually running?

---

## `cat /srv/site/index.html`

Displays the expected website content.

**Engineering Question**

> What should the application return when functioning correctly?

---

## `curl -v --max-time 5 http://localhost:8080`

Attempts to access the application while displaying detailed connection information.

**Breakdown**

- `-v` = Verbose output
- `--max-time 5` = Stop waiting after five seconds

**Engineering Question**

> Can a client successfully reach the application?

---

## `ss -ltnp | grep 8080`

Displays listening TCP sockets on port 8080.

**Breakdown**

- `-l` = Listening sockets
- `-t` = TCP
- `-n` = Numeric addresses
- `-p` = Process ownership

**Engineering Question**

> Is the application listening for connections?

---

## `sudo iptables -L INPUT -n --line-numbers`

Lists INPUT chain firewall rules.

**Engineering Question**

> Is the firewall preventing traffic from reaching the application?

---

## `sudo iptables -S INPUT`

Displays firewall rules in command syntax.

**Engineering Question**

> Which rule created the firewall behavior?

---

## `sudo iptables -D INPUT -p tcp --dport 8080 -j DROP`

Deletes the firewall rule blocking port 8080.

**Engineering Question**

> How do I remove the root cause instead of treating symptoms?

---

## `curl -s --max-time 5 http://localhost:8080`

Verifies application accessibility.

**Engineering Question**

> Can customers now reach the application?

---

## `curl -s -o /dev/null -w '%{http_code}\n'`

Displays only the HTTP status code.

**Engineering Question**

> Did the application return a successful response?

---

# Engineering Distinctions

## Running Process ≠ Reachable Service

```
systemctl

Active

        ≠

Customer Success
```

Applications may be healthy while users remain unable to connect.

---

## Listening Socket ≠ Successful Communication

```
LISTEN

        ≠

Traffic Reaches Application
```

A listening socket proves the application is ready—not that packets arrive.

---

## Timeout vs Connection Refused

### Timeout

Usually indicates packets are silently dropped.

Often caused by:

- Firewall
- Network ACL
- Security Group

---

### Connection Refused

Usually indicates:

No process is listening.

---

## Root Cause vs Symptom

Removing symptoms restores nothing permanently.

Fix the underlying cause.

---

# Evidence Collected

✅ Service running

✅ Expected webpage identified

✅ curl timed out

✅ Socket listening on 8080

✅ Firewall DROP rule discovered

✅ DROP rule removed

✅ HTTP 200 returned

✅ Website content restored

---

# Engineering Principles

## Principle 1

Always reproduce the customer's problem before changing the system.

---

## Principle 2

Evidence drives investigation.

Never troubleshoot assumptions.

---

## Principle 3

Separate application health from service availability.

---

## Principle 4

Collect evidence before implementing changes.

---

## Principle 5

Fix the root cause—not the symptom.

---

## Principle 6

Every incident ends with verification.

No verification.

No closure.

---

## Principle 7

Business capability determines success.

Customers care about successful outcomes—not process status.

---

# Consulting Perspective

A consultant investigates business impact—not technical symptoms.

Questions asked:

- What is failing?
- Can I reproduce it?
- What evidence supports the complaint?
- Where is communication interrupted?
- What changed?
- How will I prove success?

The consultant's goal is restoring business capability—not simply restarting services.

---

# Vocabulary

### Incident

An unplanned interruption or degradation of a business service.

---

### Root Cause

The underlying condition responsible for the observed problem.

---

### Symptom

An observable effect of an underlying problem.

---

### Verification

The process of proving that corrective actions restored expected functionality.

---

### Listening Socket

A network endpoint waiting for incoming client connections.

---

### Firewall Rule

A policy determining whether network traffic is permitted or denied.

---

### DROP

Silently discards network traffic.

The sender receives no response.

---

### HTTP Status Code 200

Indicates a successful HTTP request.

---

### Timeout

A communication failure caused by waiting longer than the permitted interval.

---

### Service Availability

The ability for customers to successfully access an application.

---

### Business Capability

The function the customer expects the technology to provide.

Example:

"Users can reach the internal status page."

---

# Interview Language

"When responding to production incidents, I avoid making assumptions. I first reproduce the customer's experience, verify application health, inspect network connectivity, evaluate firewall policy, identify the root cause, implement the minimum corrective action, and verify that the business capability has been restored."

---

# Principal Engineer's Takeaway

Incident response is an evidence-driven discipline.

Healthy services do not guarantee healthy business outcomes.

The strongest engineers distinguish between:

- Running applications
- Reachable applications
- Successful customer experiences

The goal is never to restart services blindly.

The goal is to restore business capability while understanding why the failure occurred.

---

# Where Else Will I See This?

| Technology | Same Principle |
|------------|----------------|
| Apache | Service running but unreachable |
| Nginx | Listening socket blocked by firewall |
| Flask | Application healthy but inaccessible |
| Docker | Container running but port unpublished |
| Kubernetes | Pod running but Service misconfigured |
| AWS EC2 | Security Group blocks traffic |
| AWS ALB | Target healthy but listener missing |
| Azure NSG | Network policy blocks application |
| Palo Alto | Enterprise firewall policy |
| Cisco ASA | ACL blocks communication |

---

# Repository Notes

## Incident Investigation Checklist

```
Customer Complaint

↓

Reproduce

↓

Application

↓

Socket

↓

Firewall

↓

Network

↓

Fix

↓

Verify

↓

Close
```

---

## Engineering Memory Aid

Never ask:

> "Is the service running?"

Instead ask:

> "Can the customer successfully use the service?"

That question always leads to the correct investigation.

## Principle Reinforced 

| Symptom            | Likely Cause                     |
| ------------------ | -------------------------------- |
| Timeout            | Firewall DROP / packet filtering |
| Connection Refused | No application listening         |
| Name not resolved  | DNS problem                      |

A great diagnostic table to remember.
