# Repository Notes

## Investigation Framework

Every networking investigation begins with seven questions:

1. Who is experiencing the problem?
2. What business capability are they trying to accomplish?
3. What does the reported symptom actually mean?
4. Where should the traffic go?
5. How should the traffic get there?
6. What evidence supports the failure?
7. Which networking component is preventing communication?

---

## Four Failure Domains

Every networking problem eventually traces back to one or more of these areas:

- Addressing
- Routing
- Name Resolution
- Connectivity

Every diagnostic command exists to eliminate one of these possibilities.

---

## Evidence Over Assumptions

A failed test proves only the capability being tested.

Examples:

- Failed `ping` proves ICMP communication was unsuccessful.
- Successful `dig` proves DNS resolution succeeded.
- Successful `curl` proves application-layer communication succeeded.

Avoid drawing conclusions beyond the evidence.
