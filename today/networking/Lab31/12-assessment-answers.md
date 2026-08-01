# Assessment Answers

### 1. C — Reproduce the customer's problem.

Effective incident response begins by observing the same failure the customer experiences.

**Engineering Takeaway**

Never troubleshoot an incident you cannot reproduce.

---

### 2. B — The application process is running.

`systemctl` verifies the service is operational but does not prove customers can access it.

**Engineering Takeaway**

A healthy process does not guarantee a healthy business service.

---

### 3. B — Inspect the firewall.

The application is listening, yet clients cannot connect.

The next logical layer is the firewall.

**Engineering Takeaway**

Investigate one layer at a time using evidence.

---

### 4. B — `iptables -S`

`iptables -S` displays rules using the same syntax required to recreate or delete them.

**Engineering Takeaway**

Understanding rule syntax simplifies troubleshooting and recovery.

---

### 5. B — Packets are silently discarded without a response.

DROP causes clients to wait until they eventually time out because no response is ever returned.

**Engineering Takeaway**

Timeouts often indicate packet filtering rather than application failure.

---

### 6. B — `sudo iptables -D INPUT -p tcp --dport 8080 -j DROP`

Deleting the exact rule removes the root cause without affecting unrelated firewall rules.

**Engineering Takeaway**

Make precise changes during incident response.

---

### 7. B — Fixing the underlying condition responsible for the incident.

Engineers solve causes, not symptoms.

**Engineering Takeaway**

Root cause analysis prevents recurring incidents.

---

### 8. C — The customer successfully receives HTTP 200 and the expected webpage.

The incident is not complete until the original business capability has been restored.

**Engineering Takeaway**

Verification is measured from the customer's perspective.

---

### 9. C — Separate application health from business availability.

`systemctl` measures process health.

`curl` measures customer experience.

Both are required for accurate diagnosis.

**Engineering Takeaway**

Never confuse operational status with service availability.

---

### 10. C — Business capability failed because network communication was interrupted.

The application itself was healthy, but customers could not benefit from it because the firewall blocked access.

**Engineering Takeaway**

Consultants communicate business impact—not simply technical failures.
