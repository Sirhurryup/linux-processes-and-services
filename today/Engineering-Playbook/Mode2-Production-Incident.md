# Mode 2 

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
