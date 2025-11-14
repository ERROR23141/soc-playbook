# What is a SOC? 

## 1. Big picture

- A **Security Operations Center (SOC)** is a team + set of processes + technology that **monitors and defends** an organization's systems and data.
- Think of it as a **24/7 security control room**:
 - Collects logs and alerts from endpoints, servers, network devices, cloud and apps.
 - Detects suspicious activity.
 - Responds to incidents and helps reduce damage.

## 2. Main goals of a SOC

- **Visibility:** See what is hapening across the whole enviroment (logs, network traffic, endpoints).
- **Detection:** Spot attacks, misconfigurations, and risky behavior as early as posible.
- **Responce:** Contain and remediate security incidents (block IP's, isolate hosts, reset accounts, etc.).
- **Improvement:** Learn from incidents, tune alerts, and close gaps so the same issue is less likely to happen again.

## 3. What does a SOC analyst do?

### Daily tasks

- **Monitor alerts** in tools like a SIEM (Splunk, Sentinel, etc.) and EDR.
- **Triage alerts:**
 - Look at logs and context.
 - Decide if it's a false positive or a real threat.
- **Investigate suspisious activity:**
 - Check login history, process execution, network connections, file hashes.
 - Use tools like Wireshark, EDR cosoles, threat intel feeds.
- **Escalate or contain:**
  - Escalate serious cases to senior analysts or incident responders.
  - Suggest actions: block IP's, disable accounts, isolate machines.
- **Document everything:**
  - Write incident tickets or reports.
  - Note what happened, evidence, and actions taken.

### Typical roles / levels

- **Tier 1 (Junior SOC Analyst):**
  - First to see alerts.
  - Does initial triage, closes obvious false positives, escalate real casses.
- **Tier 2 (Intermediate):**
  - 
