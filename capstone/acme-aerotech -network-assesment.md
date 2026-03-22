# Capstone Project - Network Security & Resilience Assesment

## Overview

This project simulates a security and resilience assesment for a fictional aerospace company, Acme Aerotech.

The objective was to analyze the company's existing network architecture to identify identify security and availability risks and design and improved network that enhances both security and reliability.

---

## Organizational Context

Acme Aerotech is a mid sized aerospace manufacturer with approximately 75 employees and a small internal IT team.

The company recently secured a goverment contract requiring stronger cybersecurity controls and improved network resilience prompting a formal security assessment.

---

## Key Findings

### Flat Network Architecture
- No segmentation between users, servers, IOT devices and infrastructure
- Increased risk of lateral movement

### Weak Access Controls
- Default credentials used on critical systems
- No least privilige enforcement

### No Patch Management
- Systems exposed to known vulnerabilities

### Single Points of Failure
- Single firewall, router and switch
- High risk of total network outage

---

## Risk Analysis

These issues create a high risk enviroment where:

- Attackers can move laterrally after initial compromise
- Critical systems are easily accessible
- Downtime risk is high due to lack of redundancy
- Security posture does not meet goverment contract expectations

---

## Redesigned Network Architecture

The redesigned network introduces:

### Network Segmentation
- DMZ for public facing services
- Seperate VLANs for:
  - Users (by department)
  - Servers
  - IOT / facilities
  - Management network
  - Wireless networks

### Improved Access Control
- Restricted communication between zones
- Least privilege access enforced

### Security Enhancements
- Isolation of IOT devices
- Secure corporate vs guest wireless seperation

### High Availability
- Firewall in active passive configuration
- Reduced single points of failure

---

## Security Improvements

- Limits lateral movement
- Protects sensitive systems
- Improves monitoring and control
- Reduces blast radius of attacks

---

## Resilience Improvements

- Redundant firewall setup
- Modular design for scalability
- Reduced downtime during failures or maintenance

---

## Tools & Concepts Used

- Network segmentation (VLANs)
- Defense in depth
- Least privilege access control
- High availability design
- Security architecture principles

---

## Challenges

- Balancing strong security with simplicity for a small IT
- Maintaining clear readable network diagrams
- Designing realistic implementable solutions

---

## Lessons Learned 

- Security and availability are closely connected
- Segmentation improves both security and troubleshooting
- Clear documentation is critical in cybersecurity

---

## Future Improvements

- Add logging and monitoring architecture (SIEM integration)
- Perform cost analysis of solutions
- Evaluate cloud based infrastructure options

---

## Supporting Materials 

### Presentation Slides
[View Slides]
