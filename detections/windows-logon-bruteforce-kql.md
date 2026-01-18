# Detections: Windows Logon Brute Force (KQL - Microsoft Sentinel)

##Purpose

Detect possible **brute force attacks against Windows logons** by lookinf for:

- Many **failed logon attempts** (Event ID 4625)
- from the same **account + IP address**
- In a short time window
- Optionally followed by a **successful logon** (Event ID 4624)

---

##Data Source

- Table: SecurityEvent `(Windows Security Logs ingested into Microsoft Sentinel)
- Key fields used (names may vary slightly by enviroment):
  - EventID`-
