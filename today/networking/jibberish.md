# Engineering Distinction

An interface is **not** an IP address.

An interface is the network connection.

An IP address is assigned to that interface.

One interface may have:

- multiple IP addresses

One server may have:

- multiple interfaces

Never confuse the interface with the address assigned to it.

# Engineering Principle

Never choose a production interface based on its name.

Choose it based on the evidence:

- IP address
- Routing
- Network purpose
- Business function

- # Engineering Principle

Patterns accelerate troubleshooting.

When a network address differs from the expected design, pause and investigate before making assumptions.

Unexpected evidence often leads directly to the root cause.

# Engineering Distinction

Do not choose an interface because it is named `eth0`.

Choose the interface that supports the business capability based on observable evidence such as:

- IP addressing
- Routing
- Gateway
- Network purpose

- # Engineering Distinction

An IP address identifies **where the server is**.

A routing table determines **how the server reaches everything else**.

Knowing your address is not enough—you must also know the path your packets will take.
