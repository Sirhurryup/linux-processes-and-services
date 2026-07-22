# Linux Engineering Principles

These principles were earned through hands-on engineering investigations, production simulations, and operational troubleshooting. They represent patterns that extend beyond Linux and apply to systems engineering, cloud architecture, and production operations.

---

# Principle 1
## Observe Before Acting

Every engineering investigation begins by observing system behavior before making changes.

Evidence precedes action.

---

# Principle 2
## Establish a Baseline

Collect measurable evidence before remediation.

Without a baseline, improvement cannot be verified.

---

# Principle 3
## Follow Evidence, Not Assumptions

Commands gather evidence.

Engineers interpret evidence.

Never troubleshoot based on assumptions.

---

# Principle 4
## Runtime and Boot Behavior Are Independent

A service may be:

- active but disabled
- enabled but inactive

Production systems typically require both.

---

# Principle 5
## Production Readiness Is an Operational Characteristic

Applications become production-ready through supervision, automation, monitoring, restart policies, permissions, and operational controls—not necessarily by changing application code.

---

# Principle 6
## Every Configuration Directive Answers an Engineering Question

Engineers should never memorize configuration directives.

Instead, ask:

- What starts the application?
- Who should run it?
- What happens if it crashes?
- Should it survive reboot?
- Does it require dependencies?

Configuration becomes the answer to operational questions.

---

# Principle 7
## References Are Better Than Duplication

Linux frequently references configuration rather than copying it.

Examples include:

- symbolic links
- systemd targets
- mount points

This same architectural principle appears throughout cloud computing:

- IAM Roles reference Policies
- Auto Scaling Groups reference Launch Templates
- Load Balancers reference Target Groups

Reference relationships simplify management while reducing duplication.

---

# Principle 8
## Verify Every Change

A command completing successfully does not prove success.

Always verify the intended behavior.

Examples:

- systemctl is-active
- systemctl is-enabled
- systemctl status
- journalctl
- pgrep

Verification completes the engineering lifecycle.

---

# Principle 9
## Minimize Operational Impact

The best remediation affects only the failing component.

Do not restart an entire server when one process can be safely replaced.

Operational precision improves reliability.

---

# Principle 10
## Technology Exists to Solve Business Problems

Technology should always be evaluated through the business capability it enables.

Understanding the business problem produces better engineering decisions than memorizing technology.

Business understanding
↓

Engineering questions
↓

Evidence

↓

Technical implementation

↓

Operational success
