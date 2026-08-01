# Assessment Answers

### 1. B — It blocks all traffic unless explicitly permitted.

A default-deny policy begins from the safest security posture by rejecting all unsolicited inbound traffic.

**Engineering Takeaway**

Start with zero trust. Open only what the business requires.

---

### 2. B — `sudo ufw status verbose`

This command displays:

- Firewall status
- Default policies
- Active firewall rules

**Engineering Takeaway**

Always verify the firewall configuration before making changes.

---

### 3. B — To prevent locking yourself out of the server.

If SSH is not allowed before enabling UFW, remote administrative access may be lost.

**Engineering Takeaway**

Protect administrative access before enforcing security controls.

---

### 4. B — Principle of Least Privilege

The database accepts connections only from the application subnet instead of the entire Internet.

**Engineering Takeaway**

Grant only the minimum network access required.

---

### 5. B — It documents security intent and continues protecting the port if the default policy changes.

Explicit DENY rules communicate design decisions and provide additional protection against future configuration changes.

**Engineering Takeaway**

Security rules should document intent, not just enforce policy.

---

### 6. B — Allows only hosts in the 10.0.1.0/24 subnet to reach PostgreSQL.

This restricts database access to trusted application servers.

**Engineering Takeaway**

Internal services should never be exposed unnecessarily.

---

### 7. B — Rule numbers change after deletions.

Deleting one rule causes the remaining rules to be renumbered.

**Engineering Takeaway**

Always verify rule numbering immediately before deleting.

---

### 8. C — AWS Security Group

Traffic must pass both cloud-level and host-level firewalls.

Since UFW already allows HTTP, the Security Group is preventing access.

**Engineering Takeaway**

Always investigate every security layer.

---

### 9. B — Multiple independent security controls protect the system.

Defense in Depth ensures that if one security control fails, others continue protecting the system.

Examples include:

- AWS Security Groups
- UFW
- Application Authentication

**Engineering Takeaway**

No single security control should be trusted as the only line of defense.

---

### 10. C — Because the business requires customers to access the web application over HTTP.

Firewall rules should exist because they support a business capability—not simply because an application uses a port.

**Engineering Takeaway**

Business requirements drive security architecture. Engineers translate those requirements into firewall policy.
