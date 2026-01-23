# Threat Hunting – Wazuh SIEM Incident (Financial Sector Lab)

**Lab type:** Threat Hunting / SOC  
**Tooling:** Wazuh SIEM (Docker), YAML config, MITRE ATT&CK  
**Role:** Junior Cybersecurity Analyst  

---

## 1. Scenario

I am acting as a junior cybersecurity analyst for a **multinational financial organization** that has seen a series of **unauthorized access attempts** against its internal network.

The Security Operations Center (SOC) has collected logs from multiple sources:

- Firewalls  
- Intrusion Detection Systems (IDS)  
- Endpoint security solutions  

My task in this lab was to:

- Import these logs into **Wazuh SIEM** (running in Docker configured via a provided YAML file)
- Analyze the events to identify **indicators of compromise (IOCs)**
- Map attacker behavior to **MITRE ATT&CK techniques**
- Determine a likely **threat actor group or profile** based on TTPs
- Recommend **mitigation strategies** to reduce the chance of similar incidents in the future

---

## 2. Objective

> Use Wazuh SIEM to investigate suspicious activity in a financial sector network, identify IOCs and attacker behavior and propose realistic mitigations from a SOC analyst perspective.

Key goals:

- Normalize and ingest the provided log dataset into Wazuh  
- Build and run searches to surface suspicious patterns (failed/successful logons, scans, lateral movement, etc.)  
- Extract **IOCs** (IP addresses, usernames, hosts, patterns)  
- Map the observed activity to **MITRE ATT&CK** and compare it to known **threat actor playbooks**  
- Produce a short threat hunting style report with **findings + recommended actions**

---

## 3. Environment

- **Platform:** Wazuh deployed via **Docker** using a lab provided `docker-compose`/YAML configuration  
- **Data sources (lab logs):**
  - Firewall events (connections, blocked/allowed, ports)
  - IDS alerts (signatures for scanning, brute force, exploit attempts)
  - Endpoint/host logs (auth events, process activity)
- **Reference:**  
  - **MITRE ATT&CK** for mapping tactics/techniques  
  - Wazuh rules / decoders for parsing and alerting

---

## 4. Approach

1. **Ingest & verify logs**
   - Brought up Wazuh in Docker using the provided YAML configuration.
   - Verified that firewall, IDS and endpoint logs were being parsed and generating alerts.

2. **Form hunting hypotheses**
   - Based on the scenario (financial sector, unauthorized access attempts) I focused on:
     - Credential attacks (brute force, password guessing)
     - Lateral movement (RDP/SMB/SSH)
     - Suspicious external IPs repeatedly hitting internal services

3. **Search & pivot in Wazuh**
   - Queried Wazuh for:
     - Repeated login failures followed by success
     - Scanning patterns (one source → many destinations / ports)
     - IDS signatures tied to known ATT&CK techniques
   - Pivoted on suspicious IPs, usernames and hosts to build a **timeline** of the incident.

4. **Map to MITRE & characterize the actor**
   - Mapped observed TTPs to **MITRE ATT&CK** (e.g. brute force, remote services, discovery).
   - Compared the pattern to common **financial sector threat actors** (e.g. credential stealing and lateral movement heavy groups) focusing less on naming a specific group and more on the **behavior profile**.

5. **Document findings & mitigations**
   - Summarized IOCs, TTPs and impact.
   - Recommended mitigations around:
     - Hardening external access (VPN/MFA for admins)
     - Improving logging/alerting in Wazuh
     - Tuning or adding rules for repeated failures, scan patterns and high-value asset activity.

---

## 5. Timeline (from Wazuh & logs)

Reconstructed timeline from the lab:

- **09:15 UTC** – Repeated **RDP login failures** (Windows Event ID 4625) from an external IP  
- **09:42 UTC** – **Successful RDP login** (Event ID 4624) from the same IP  
- **09:50 UTC** – **SMB scanning** from another external IP, targeting multiple internal hosts on TCP/445  
- **10:05 UTC** – **SSH brute-force attempts** against a Linux host from an external IP (multiple failed logins)  
- **10:15 UTC** – **Telnet attempts** against internal systems from another external IP on TCP/23  

This sequence looks like:

> **Brute force → possible initial access → internal scans → more brute force / remote access attempts**

---

## 6. IOCs (Indicators of Compromise / Concern)


| IOC (IP)      | Activity                             | Approx Time | Log Source / Evidence                         |
|---------------|--------------------------------------|------------:|-----------------------------------------------|
| `203.0.113.75` | Repeated RDP failures, then success  | 09:15–09:42 | Windows Security Logs (4625 → 4624)          |
| `203.0.113.35` | SMB scan against many hosts (TCP/445)| ~09:50      | Firewall / IDS logs (horizontal 445 scans)   |
| `198.51.100.63`| SSH brute force (multiple failures)  | ~10:05      | Linux auth logs (failed SSH logins)          |
| `198.51.100.53`| Telnet login attempts (TCP/23)       | ~10:15      | Firewall / IDS logs (external Telnet probes) |

---

## 7. Recommendations (Mitigations & Detections)

### 7.1. Mitigations

1. **Restrict external RDP**
   - Require VPN/ZTNA and MFA for any remote admin access.
   - Remove direct RDP exposure to the internet where possible.

2. **Harden authentication**
   - Enforce **account lockout** or throttling for repeated login failures.
   - Require **MFA** for admin/privileged accounts.

3. **Block legacy and risky services**
   - Block SMB (`445/139`) and Telnet (`23`) at the perimeter.
   - Disable **SMBv1** and limit SMB access to only necessary internal systems.

4. **Protect web apps**
   - Place public web services behind a **WAF**.
   - Keep internet facing applications patched to reduce exploit risk (`T1190`).

5. **Egress & identity controls**
   - Implement **egress filtering** so compromised hosts can’t freely talk to the internet.
   - Monitor for **new admin accounts** or group changes on critical systems.

---

### 7.2. Detection Ideas (What a SOC Should Alert On)

These can be implemented as Wazuh rules / SIEM correlation searches:

1. **RDP brute-force with follow up success**
   - Correlate:
     - Many `4625` failures from one IP → one `4624` success soon after.
   - Alert as **“Potential RDP account compromise”**.

2. **Horizontal SMB scan**
   - One external IP hitting **multiple internal IPs** on `445` in a short period.
   - Alert as **“External SMB scanning – possible lateral movement prep”**.

3. **SSH brute-force**
   - Threshold based alert on repeated `Failed password for <user>` from a single IP in Linux auth logs.

4. **Telnet usage**
   - Any inbound Telnet (`TCP/23`) from the internet should be extremely rare.
   - Treat as **suspicious** by default.

5. **Privileged account changes**
   - Monitor and alert on:
     - New local/domain admins
     - Changes to sensitive AD groups
   - Useful for detecting post compromise privilege escalation.
