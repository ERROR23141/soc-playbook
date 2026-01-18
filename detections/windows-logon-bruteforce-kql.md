# Detections: Windows Logon Brute Force (KQL - Microsoft Sentinel)

##Purpose

Detect possible **brute force attacks against Windows logons** by lookinf for:

- Many **failed logon attempts** (Event ID 4625)
- from the same **account + IP address**
- In a short time window
- Optionally followed by a **successful logon** (Event ID 4624)

---

##Data Source

- Table: SecurityEvent (Windows Security Logs ingested into Microsoft Sentinel)
- Key fields used (names may vary slightly by enviroment):
  - EventID - Windows event ID
  - TimeGenerated - time of the event
  - Account - username/account that attempted to log in
  - IpAddress - source IP address of the logon

> May need to adjust field names (e.g. Accounts - TargetAccounts or SubjectUserName) depending how logs are parsed.

---

##Query 1 - Simple Brute Forece: Many Failed Logons

**Use case:** find accounts that had **>= 10 failed logons** from the same IP in a 10 minute window.

kusto
// Simple brute force detection: many failed logons in 10 minutes
SecurityEvent
| where TimeGenerated > ago(1d)
