# Assessment Answers

### 1. C — `ss -tlnp`

Always verify that the service is listening before investigating the network or firewall.

**Engineering Takeaway:** Start with the application.

---

### 2. B — The service is waiting for incoming connections.

LISTEN indicates the application is prepared to accept new client connections.

**Engineering Takeaway:** LISTEN does not mean clients are currently connected.

---

### 3. B — Capture and inspect network packets.

`tcpdump` confirms whether traffic reaches the network interface.

**Engineering Takeaway:** Evidence beats assumptions.

---

### 4. B — Disables DNS name resolution.

Numeric output makes captures faster and easier to interpret.

**Engineering Takeaway:** Use `-n` during troubleshooting.

---

### 5. B — A saved packet capture.

A `.pcap` file preserves network traffic for later analysis or sharing.

**Engineering Takeaway:** Save evidence before making changes.

---

### 6. C — First matching rule wins.

`iptables` evaluates rules sequentially from top to bottom.

**Engineering Takeaway:** Rule order is critical.

---

### 7. B — To ensure the traffic is recorded.

A packet capture cannot record events that occurred before it started.

**Engineering Takeaway:** Start the capture before reproducing the issue.

---

### 8. C — Firewall policy.

If the service is listening and packets arrive, the next layer to verify is the firewall.

**Engineering Takeaway:** Investigate one layer at a time.
