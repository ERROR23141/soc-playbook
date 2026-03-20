# Incident Report: Suspicious Logon Activity

This report demonstrates how a potential brute force authentication attempt could be investigated in a SOC enviroment using log analysis.
  - [Windows Logon Brute Force Detection](../detections/windows-logon-bruteforce-kql.md)
## Summary 
Multiple failed logon attempts were detected against a user account within a short time window followed by a successful login.

## Detection
Alert triggerd from KQL detection: Windows Logon Brute Force.

## Timeline
- 10:05 - Multiple failed logons detected
- 10:07 - Failed attempts increase
- 10:09 - Successful login recorded

## Indicators
Source IP: 185.xx.xxx

Target Account: admin_user

## Investigation
- Checked login logs
- Verified source IP reputation
- Reviewed activity after login

## Response
- Disabled affected account
- Forced password reset
- Blocked source IP
