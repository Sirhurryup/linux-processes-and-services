# Assessment

### 1.

Why should a firewall use a **default deny** policy?

A. It improves server performance.

B. It blocks all traffic unless explicitly permitted.

C. It automatically updates firewall rules.

D. It encrypts network traffic.

---

### 2.

Which command displays the firewall's default policies and active rules?

A. sudo ufw enable

B. sudo ufw status verbose

C. sudo ufw reset

D. sudo ufw allow

---

### 3.

Why should SSH be allowed **before** enabling the firewall on a remote server?

A. SSH requires HTTPS.

B. To prevent locking yourself out of the server.

C. SSH automatically enables UFW.

D. It improves firewall performance.

---

### 4.

Which principle is demonstrated by allowing PostgreSQL only from `10.0.1.0/24`?

A. High Availability

B. Principle of Least Privilege

C. Fault Tolerance

D. Load Balancing

---

### 5.

What is the primary benefit of an explicit **DENY** rule for port 9229 even when the default policy already denies incoming traffic?

A. Faster packet forwarding.

B. It documents security intent and continues protecting the port if the default policy changes.

C. It enables debugging.

D. It reduces CPU usage.

---

### 6.

What does the following command accomplish?

```bash
sudo ufw allow from 10.0.1.0/24 to any port 5432
```

A. Allows everyone to access PostgreSQL.

B. Allows only hosts in the 10.0.1.0/24 subnet to reach PostgreSQL.

C. Blocks PostgreSQL completely.

D. Enables HTTPS.

---

### 7.

Why should engineers use:

```bash
sudo ufw status numbered
```

before deleting rules?

A. It refreshes the firewall.

B. Rule numbers change after deletions.

C. It backs up firewall rules.

D. It restarts UFW.

---

### 8.

A web server is unreachable.

`nginx` is running.

UFW allows ports 80 and 443.

AWS Security Groups deny inbound HTTP.

Where is the failure?

A. UFW

B. nginx

C. AWS Security Group

D. DNS

---

### 9.

Which statement best describes **Defense in Depth**?

A. One firewall protects everything.

B. Multiple independent security controls protect the system.

C. Multiple passwords protect the server.

D. Encrypt every file.

---

### 10.

A consultant asks:

> "Why is port 80 open?"

Which response demonstrates the strongest engineering mindset?

A. Because nginx uses it.

B. Because Linux requires it.

C. Because the business requires customers to access the web application over HTTP.

D. Because it was already configured.
