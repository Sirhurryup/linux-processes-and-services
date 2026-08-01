# Linux Incident Response Workflow

## Mission

Restore business capability through evidence-based investigation.

Never guess.

Never skip layers.

Never change multiple things at once.

---

# Step 1 — Understand the Business Problem

Ask:

- What is broken?
- Who is affected?
- When did it begin?
- What changed?
- What does success look like?

Example

"The internal website is timing out."

---

# Step 2 — Reproduce the Problem

Can I observe the same failure?

Examples

curl

Browser

SSH

Application

Never investigate a problem you cannot reproduce.

---

# Step 3 — Identify the Battlefield

Which team owns this layer?

Operating System

Networking

Security

Application

Database

Cloud Infrastructure

Sometimes the answer is:

"I don't know yet."

That is okay.

---

# Step 4 — Investigate Your Layer

As a Linux Systems Administrator

Verify:

✓ Server healthy

✓ Disk healthy

✓ Memory healthy

✓ CPU healthy

✓ Network healthy

✓ DNS healthy

✓ Service running

✓ Socket listening

✓ Firewall

✓ Logs

Collect evidence.

Do not make changes yet.

---

# Step 5 — Develop a Theory

Every piece of evidence should answer:

"What is the most likely explanation?"

Example

Application running

Socket listening

Firewall DROP rule

↓

Theory:

Firewall preventing communication.

---

# Step 6 — Validate the Theory

Can I prove my theory?

If not,

collect more evidence.

---

# Step 7 — Fix the Root Cause

One change.

One validation.

Never shotgun changes.

---

# Step 8 — Verify the Business Outcome

The customer should now succeed.

Examples

HTTP 200

Login successful

Database connection restored

SSH works

This is the only definition of success.

---

# Step 9 — Determine Ownership

Ask

"Is this still my incident?"

Examples

PHP exception

↓

Developer

Slow SQL query

↓

Database Administrator

Security Group

↓

Cloud Engineer

Expired certificate

↓

Security Engineer

Kernel panic

↓

Linux Systems Administrator

---

# Core Principles

Evidence over assumptions.

Business outcome over technical output.

Healthy process does not equal healthy service.

Fix the cause.

Verify the result.

Know where your responsibility ends.

Communicate evidence professionally.
