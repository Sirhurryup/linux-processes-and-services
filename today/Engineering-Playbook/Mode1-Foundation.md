# Mode 1 
Start at Layer 1 and prove everything. 

```

Identity
↓
Routing
↓
Gateway
↓
DNS
↓
Application

```


ip route tells Linux how to deliver packets. If the destination belongs to my local subnet, Linux delivers it directly. If the destination is outside my subnet and no more specific route exists, Linux forwards the packet to the default gateway, which knows the next hop.

# think in dependencies
```
Can I reach myself?

↓

Can I reach my gateway?

↓

Can I reach another host?

↓

Can I resolve DNS?

↓

Can I reach an application?
```
Each answer unlocks the next question.

# That's why we use different tools for different questions.

| Question                         | Tool   |
| -------------------------------- | ------ |
| Is another host reachable by IP? | `ping` |
| Can I resolve a hostname?        | `dig`  |
| Can I retrieve a web page?       | `curl` |

Each tool investigates a different layer. 


# Build a Habit 
Every time you choose a host to test, ask yourself these three questions: 

1. Is it in my subnet?

If yes, Linux delivers directly.

2. Is it outside my subnet?

If yes, Linux must use the default gateway.

3. Why am I choosing this host?

Every test should answer one question.

# Engineering Habit 

When troubleshooting networking:

Who am I? → ip addr
Where will packets go? → ip route
Can I reach my first hop? → ping <default gateway>
Can I reach another known host by IP?
Can I resolve names? → dig
Can I reach the application? → curl

Notice this is no longer a checklist of commands.

It's a sequence of engineering questions.

# Think Like the OSI Model

Application         curl
------------------------------
Name Resolution     dig
------------------------------
Network             ping
------------------------------
Routing             ip route
------------------------------
Interface           ip addr

# Each command has **one responsibillity**

| Question                         | Tool       | Responsibility     |
| -------------------------------- | ---------- | ------------------ |
| Who am I?                        | `ip addr`  | Identity           |
| Where do I send packets?         | `ip route` | Routing            |
| Can I reach my first dependency? | `ping`     | Connectivity       |
| Can I translate names?           | `dig`      | Name Resolution    |
| Can the application respond?     | `curl`     | Application Health |

# My framework is built off dependencies to seek the truth.

Every single step leads toward 

Question → Evidence → Interpretation → Next Question
