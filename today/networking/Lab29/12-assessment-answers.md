# Assessment Answers

### 1. B — It refreshes the package catalog.

Refreshing the catalog ensures the server knows what packages and versions are available.

**Engineering Takeaway:** Always update the catalog before installing software.

---

### 2. B — apt install

`apt` installs trusted operating system packages from Ubuntu repositories.

**Engineering Takeaway:** Use the operating system's package manager for system software.

---

### 3. B — pip3

Flask is a Python library managed by `pip3`, not `apt`.

**Engineering Takeaway:** Different ecosystems use different package managers.

---

### 4. C — curl localhost

Receiving the default nginx page proves the application is functioning.

**Engineering Takeaway:** Verify business functionality, not just installation.

---

### 5. B — Remove orphaned dependencies

`apt autoremove` cleans packages that are no longer required.

**Engineering Takeaway:** Lean systems reduce operational complexity and attack surface.

---

### 6. C — apt purge

`apt purge` removes both the software and its configuration files.

**Engineering Takeaway:** Choose `remove` or `purge` based on whether configuration should be preserved.

---

### 7. B — Verified packages and dependency management

Repositories provide trusted software, dependency resolution, and package integrity.

**Engineering Takeaway:** Professional environments install software from trusted repositories.

---

### 8. C — Business functionality verified

A successful installation is incomplete until the application satisfies the intended business capability.

**Engineering Takeaway:** Engineers validate outcomes, not just completed commands.
