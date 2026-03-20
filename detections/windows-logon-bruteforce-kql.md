# Detections: Windows Logon Brute Force (KQL - Microsoft Sentinel)

## Purpose

Detect possible **brute force attacks against Windows logons** by looking for:

- Many **failed logon attempts** (Event ID 4625)
- From the same **account + IP address**
  Short time window
- Optionally followed by a **successful logon** (Event ID 4624)

---

## Data Source

- Table: SecurityEvent (Windows Security Logs ingested into Microsoft Sentinel)
- Key fields used (names may vary slightly by enviroment):
  - EventID - Windows event ID
  - TimeGenerated - time of the event
  - Account - username/account that attempted to log in
  - IpAddress - source IP address of the logon

> May need to adjust field names (e.g. Accounts - TargetAccounts or SubjectUserName) depending how logs are parsed.

Events used:

- **4625** - Failed logon
- **4624** - Successful logon

---

## Query 1 - Simple Brute Forece: Many Failed Logons

**Use case:** find accounts that had **>= 10 failed logons** from the same IP in a 10 minute window.

``` kusto
// Simple brute force detection: many failed logons in 10 minutes
SecurityEvent
| where TimeGenerated > ago(1d)          // look at last 24 (adjust as needed)
| where EventID == 4625                  // failed logon
| summarize FailedCount = count()
     by bin (TimeGenerated, 10m), Account, IpAddress      // group in 10 minute windows
| where FailedCount >= 10
| order by FailedCount desc
```
## What This Detection Looks For

This query searches for:
  - repeated failed Windows logons
  - against the same account
  - from the same source IP
  - grouped into 10 minute windows

This can indicate:
  - password spraying
  - brute force attacks
  - repeated unauthorized access attempts

---

## Query 2 - Failures Followed by Success (More Dangerous)

**Use case:** catch the situation where an attacker **eventually succeeds** after many failures.

``` kusto
let TimeFrame = 1d;
let FailedLogons =
    SecurityEvent
    | where TimeGenerated > ago(TimeFrame)
    | where EventID == 4625          // failed logon
    | summarize FailedCount = count(),
          FirstFailure = min(TimeGenerated),
          LastFailure = max(TimeGenerated)
        by Account, IpAddress;
let SuccesfulLogons =
    SecurityEvent
    | where TimeGenerated > ago(TimeFrame)
    | where EventID == 4624          // successful logon
    | summarize SuccessTime = min(TimeGenerated)
        by Account, IpAdress;
FailedLogons
| join kind=inner SuccesfulLogons on Account, IpAdress
| where FailedCount >= 5          // at least 5 failures
    and SuccessTime > LastFailure          // success after the failures
    and SuccessTime < LastFailure + 15m          //within 15 minutes
| project Account,
      IpAdress,
      FailedCount,
      FirstFailure,
      LastFailure,
      SuccessTime
| order by FailedCount desc
```
## What This Detection Looks For

This queary seaches for:
  - multiple failed logons
  - followed by a succesful logon
  - from the same account and IP
  - whithin a short time period

This is more dangerous than failures alone because it may indicate:
  - a successful brute force compromise
  - unauthorized account access
  - a valid credential being quessed or stolen

---

## MITRE ATT&CK Mapping

Technique: T1110 - Brute Force

Tactic:
  - Credential Access
  - Initial Access

---

## Analyst Investigation Steps

If the detection triggers:
  1. Identify the account being targeted
  2. Review the source IP address
  3. Check whether the IP is internal or external
  4. Investigate whether the successful logon was expected
  5. Review any activity after the successful login:
       - new process
       - privilege changes
       - lateral movement
  6. Reset or disable the account if compromise is suspected

 ---

 ## SOC Relevance

 Brute force and password spraying attempts are common in real enviroments.

 This detection helps a SOC analyst:
   - spot authentication abuse early
   - prioritize accounts at risk
   - identify possible account compromise
   - correlate failed logons with follow up activity

---

## Related Incident Example

See example investigation:

 - [Brute Force Incident Report](../incidents/brute-force-investigation.md)
