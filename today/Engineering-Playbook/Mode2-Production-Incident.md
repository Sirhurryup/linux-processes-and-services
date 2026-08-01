# Mode 2

# Beginner Connectivity Playbook
## Version 1.0

> Goal: Never guess. Ask one question, collect evidence, then move to the next question.

---

# The Five Questions

## Question 1
### Is the service running?

**Tool**
```bash
systemctl status <service>
```

Examples

```bash
systemctl status ssh
systemctl status nginx
systemctl status apache2
```

Purpose

- Determines whether systemd successfully started the service.
- If the service is stopped or failed, stay with Linux.
- Do not investigate networking yet.

---

## Question 2
### Is the service listening?

**Tool**

```bash
sudo ss -tlnp
```

Example

```bash
sudo ss -tlnp | grep :22
```

Purpose

- Determines whether the application successfully opened a network socket.
- Look for:
    - LISTEN
    - Port number
    - Process name

If nothing is listening:

The service may be running but failed to bind to a port.

---

## Question 3
### Are packets arriving?

**Tool**

```bash
sudo tcpdump -i eth0 -n port <port>
```

Example

```bash
sudo tcpdump -i eth0 -n port 22
```

Purpose

- Determines whether traffic is actually reaching the server.
- If no packets arrive:
    - Think routing
    - Security Groups
    - Network path
    - Wrong destination IP

---

## Question 4
### Is Linux blocking the traffic?

**Tool**

```bash
sudo iptables -L INPUT -n --line-numbers
```

Purpose

- Determines whether Linux itself is dropping packets.
- Look for:
    - DROP
    - ACCEPT
    - Rule order

Remember:

First matching rule wins.

---

## Question 5
### What do the logs say?

**Tool**

```bash
journalctl -u <service>
```

Example

```bash
journalctl -u ssh
journalctl -u nginx
```

Purpose

If Linux, networking, and the firewall all look healthy, investigate the application's logs.

---

# Investigation Rule

Never ask a question that your evidence has already answered.

Example

systemctl

↓

Service is running

↓

Do not ask systemctl again.

Move to the next unanswered question.

---

# My Thought Process

Customer reports a problem

↓

Can I reproduce it?

↓

Question 1

↓

Collect evidence

↓

Interpret evidence

↓

Question 2

↓

Collect evidence

↓

Interpret evidence

↓

Continue until the evidence identifies the owner.

---

# Ownership

Linux
- Service failed
- Port not listening

Networking
- Packets never arrive

Security
- Firewall blocks traffic

Developer
- Application receives traffic but fails

Database
- Application healthy but backend unavailable

---

# Golden Rule

Evidence first.

Interpretation second.

Never guess.




This mode applies the system. 

# Incident Investigation Framework

```
Customer Problem
        ↓
Can I reproduce it?
        ↓
What dependency is failing?
        ↓
Application
        ↓
Service
        ↓
Socket
        ↓
Firewall
        ↓
Routing
        ↓
Interface
```

# Process 

```
Ticket

↓

What is actually failing?

↓

Which dependency does that belong to?

↓

Which tool answers THAT question?

↓

Collect evidence.

↓

Interpret evidence.

↓

Next dependency.

```



# Toolbelt 

| Question                           | Tool         |
| ---------------------------------- | ------------ |
| Is the service listening?          | `ss`         |
| Are packets reaching the server?   | `tcpdump`    |
| Is Linux blocking them?            | `iptables`   |
| Is the service managed correctly?  | `systemctl`  |
| Is the application logging errors? | `journalctl` |


# Dependency Chain 

systemd

↓

Application starts

↓

Application opens socket

↓

ss confirms LISTEN

↓

Packets must arrive

↓

Firewall must allow

↓

Application responds
