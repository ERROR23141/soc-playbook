# Detections

This folder will store detection ideas and queries (KQL/SPL/pseudo-code).

Each detection will try to follow this format:

- **Name**
- **Log source** (e.g. Windows Security, Azure AD, Firewall)
- **Query** (KQL/SPL/pseudo-code is fine for now)
- **What it detects**
- **MITRE ATT&CK technique**

---

## Example Detection: Multiple failed logins followed by success

- **Name:** Possible password guessing (multiple failures then success)
- **Log source:** Windows Security Logs
- **What it detects:** 
  - A user or IP that has many failed logon attempts
  - Followed by a successful logon in a short time window
- **MITRE ATT&CK:** T1110 – Brute Force

**Pseudo-query idea:**

```text
Look for 5+ failed logons (Event ID 4625) 
from the same username or IP within 10 minutes,
followed by a successful logon (Event ID 4624)
for the same username or IP.
```
**As I learn more KQL/SPL, I’ll replace pseudo-queries with real queries.**
