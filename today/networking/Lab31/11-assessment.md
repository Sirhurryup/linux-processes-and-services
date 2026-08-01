# Assessment

### 1.

A customer reports that a website times out.

Which action should be performed first?

A. Restart the web server.

B. Reboot the server.

C. Reproduce the customer's problem.

D. Delete firewall rules.

---

### 2.

`systemctl status sitehttp.service` reports:

```
active (running)
```

What does this prove?

A. Customers can reach the application.

B. The application process is running.

C. The firewall is configured correctly.

D. DNS is functioning.

---

### 3.

`curl http://localhost:8080`

hangs for several seconds before timing out.

`ss -ltnp`

shows:

```
LISTEN 0.0.0.0:8080
```

What is the BEST next investigation step?

A. Restart the service.

B. Inspect the firewall.

C. Reboot Linux.

D. Reinstall Python.

---

### 4.

Which command displays firewall rules exactly as they were created?

A.

```bash
iptables -L
```

B.

```bash
iptables -S
```

C.

```bash
iptables -F
```

D.

```bash
iptables-save
```

---

### 5.

Why does a client usually **hang** instead of immediately failing when a firewall uses a **DROP** rule?

A. The application is overloaded.

B. Packets are silently discarded without a response.

C. DNS cannot resolve.

D. TCP automatically retries forever.

---

### 6.

Which command removes the exact firewall rule blocking TCP port 8080?

A.

```bash
sudo iptables -A INPUT -p tcp --dport 8080 -j DROP
```

B.

```bash
sudo iptables -D INPUT -p tcp --dport 8080 -j DROP
```

C.

```bash
sudo iptables -L INPUT
```

D.

```bash
sudo iptables -F INPUT
```

---

### 7.

Which statement best describes **root cause analysis**?

A. Restarting services until the problem disappears.

B. Fixing the underlying condition responsible for the incident.

C. Clearing system logs.

D. Rebooting the server after every outage.

---

### 8.

Which result provides the strongest evidence that the incident has been resolved?

A.

```
systemctl status
```

shows active.

B.

The DROP rule no longer exists.

C.

The customer successfully receives HTTP 200 and the expected webpage.

D.

The firewall configuration has changed.

---

### 9.

Which engineering principle is demonstrated by using both

```
systemctl
```

and

```
curl
```

during troubleshooting?

A. High Availability

B. Defense in Depth

C. Separate application health from business availability.

D. Fault Tolerance

---

### 10.

A consultant tells a client:

> "The application was healthy. The firewall prevented customer traffic from reaching it."

What is the consultant communicating?

A. The application failed.

B. The operating system crashed.

C. Business capability failed because network communication was interrupted.

D. The service must be reinstalled.
