# soc-playbook (Beginner → Blue → Red)

Hands-on cybersecurity portfolio: blue team labs, detections (KQL/SPL), Wireshark notes, incident reports, and a web attack diary from the defender’s point of view.

**Repo:** https://github.com/ERROR23141/soc-playbook

---

## Highlights

- 🛡️ Detections (KQL/SPL) mapped to **MITRE ATT&CK**
- 📡 Wireshark display filters with **packet analysis notes**
- 📂 Incident reports based on **CTFs / labs**
- 🌐 Web attack diary (defender’s view of common web attacks)
- 🔍 (Planned) Hunts & capstone investigations

---

## Directory

- `/detections` – SIEM-style queries (KQL/SPL) + explanations + ATT&CK IDs  
- `/wireshark` – Cheatsheets, filters, and practice notes from pcaps  
- `/hunts` – Threat hunting ideas, hypotheses, and queries  
- `/incidents` – Structured incident reports (summary, timeline, detection, response, lessons learned)  
- `/web-attack-diary` – Notes on web attacks (SQLi, XSS, etc.) from a blue team perspective  
- `/capstone` – End-to-end case studies combining packets, detections, and incident reports  

---

## Getting Started

If you’re looking through this repo, start here:

1. **Wireshark basics**  
   - [`/wireshark/cheatsheet.md`](./wireshark/cheatsheet.md) – core display filters and notes.

2. **Detections**  
   - [`/detections/`](./detections/) – KQL/SPL queries with:
     - What the query detects  
     - Short explanation  
     - Mapped MITRE ATT&CK technique IDs  

As I learn more, I’ll keep adding:

- New detections  
- More detailed incident reports  
- Web attack defender notes  
- At least one full capstone investigation
