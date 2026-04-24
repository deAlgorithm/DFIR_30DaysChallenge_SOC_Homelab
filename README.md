<img width="735" height="410" alt="image" src="https://github.com/user-attachments/assets/2fce8215-d286-413c-bfff-a4ed56c0a455" />


# DFIR 30 Days Challenge — SOC Home Lab

**Author:** Ishaque Otabil
**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**LinkedIn:** linkedin.com/in/ishaqueotabil
**GitHub:** github.com/deAlgorithm
**Stack:** Elasticsearch · Kibana · Fleet Server · Elastic Agent · Sysmon · Vultr Cloud

---

## Status — Completed to Day 21

This repository documents 21 days of the MyDFIR 30 Day SOC Analyst Challenge. The challenge was paused at Day 21 due to final year university exams and cloud infrastructure costs.

**Why the C2 simulation didn't complete:** The tutorial was recorded on an older Mythic build. Mythic v3.4.32 changed how the HTTP C2 profile handles callback routing. The callback host in the payload must specify the full protocol and IP in the format `http://[MythicIP]` and the C2 Docker container must be running and healthy for agent messages to reach Mythic. A wrong IP, wrong protocol or stopped container silently breaks the callback with no error. The agent ran on the target and the process was visible in PowerShell but no callback appeared in Mythic Active Callbacks.

**Way forward:** Instead of rebuilding the same lab on a different cloud provider, the focus is moving to hands-on GitHub projects covering Splunk, Windows Forensics, Active Directory, Log Analysis and Vulnerability Management — the skill gaps this challenge exposed.

---

## About This Repository

Each day includes a markdown writeup covering what was done, technical details, version differences encountered and key takeaways. The lab ran on Vultr cloud infrastructure using ELK Stack 9.x with real internet-exposed servers generating real attack telemetry.

---

## Lab Architecture

| Component | Role | IP |
|---|---|---|
| ELK Server | Elasticsearch + Kibana SIEM | 66.42.86.121 |
| Fleet Server | Elastic Agent management | 155.138.228.224 |
| Windows Server 2025 | RDP exposed target machine | 155.138.210.93 |
| Ubuntu Server 24.04 | SSH exposed target machine | Destroyed |
| Mythic C2 Server | Attack simulation | 66.42.84.121 (Destroyed) |
| Kali Linux | Attack machine | Local VM |

> All cloud servers destroyed April 2026 after challenge pause.

---

## Progress

| Day | Topic | Status |
|---|---|---|
| Day 1 | System design & architecture | ✅ Done |
| Day 2 | ELK Stack concepts & Splunk comparison | ✅ Done |
| Day 3 | Elasticsearch installation & configuration | ✅ Done |
| Day 4 | Kibana installation & configuration | ✅ Done |
| Day 5 | Windows Server 2025 deployment | ✅ Done |
| Day 6 | Elastic Agent & Fleet Server concepts | ✅ Done |
| Day 7 | Fleet Server setup & Windows agent enrollment | ✅ Done |
| Day 8 | Introduction to Sysmon | ✅ Done |
| Day 9 | Sysmon installation & configuration | ✅ Done |
| Day 10 | Ingesting Sysmon & Defender logs into Elasticsearch | ✅ Done |
| Day 11 | Brute force attacks — concepts & defence | ✅ Done |
| Day 12 | Ubuntu Server deployment & auth log analysis | ✅ Done |
| Day 13 | Ubuntu agent enrollment & Linux log analysis | ✅ Done |
| Day 14 | SSH brute force detection, alerts & dashboard | ✅ Done |
| Day 15 | Introduction to RDP & identifying exposed servers | ✅ Done |
| Day 16 | RDP brute force detection in Kibana | ✅ Done |
| Day 17 | RDP authentication maps & dashboard | ✅ Done |
| Day 18 | Command and Control (C2) concepts | ✅ Done |
| Day 19 | Attack diagram & planning | ✅ Done |
| Day 20 | Mythic C2 server setup | ✅ Done |
| Day 21 | Full attack simulation — initial access to exfiltration | ✅ Done |
| Day 22 | Paused — exams & cloud costs | ⏸ Paused |
| Days 23-30 | Paused | ⏸ Paused |

---

## What Was Built

**SIEM Infrastructure**
- Elasticsearch 9.x + Kibana deployed on Vultr cloud
- Fleet Server managing Windows and Ubuntu Elastic Agents
- Sysmon installed on Windows Server with Olaf Hartong modular config
- Custom Windows Event Log integrations for Sysmon and Microsoft Defender

**Detection & Alerting**
- SSH brute force detection rules with threshold and advanced alerts
- RDP brute force detection using Event ID 4625
- Geospatial authentication dashboards for SSH and RDP
- 4 maps and 4 data tables in a live Kibana dashboard

**Attack Simulation (Partial)**
- Full attack diagram mapped across 6 phases with MITRE ATT&CK mapping
- Mythic C2 server deployed with Apollo agent and HTTP C2 profile
- Payload delivered to Windows Server via Python HTTP server (confirmed 200 response)
- C2 callback unresolved due to Mythic v3.4.32 version differences

**Real Attack Telemetry**
- Live SSH brute force from multiple countries (Germany, France, Iran, Bangladesh)
- Live RDP brute force — single French IP attempted 45,358 logins
- Real attacker IPs visible in Kibana Discover and dashboard

---

## Key Ports Covered

| Port | Protocol |
|---|---|
| 22 | SSH |
| 443 | HTTPS |
| 3389 | RDP |
| 5601 | Kibana |
| 8220 | Fleet Server |
| 9200 | Elasticsearch |

---

## Key Event IDs Covered

| Event ID | Meaning |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 1116 | Defender — malware detected |
| 1117 | Defender — malware action taken |
| 5001 | Defender — real-time protection disabled |
| Sysmon 1 | Process creation |
| Sysmon 3 | Network connection |
| Sysmon 10 | Process access — LSASS |
| Sysmon 22 | DNS query |

---

## Next Steps

Moving to hands-on GitHub projects to close skill gaps identified during this challenge:

1. Splunk
2. Windows Forensics
3. Log Analysis
4. Active Directory
5. Security Assessments
6. Vulnerability Management
7. Malware Analysis

---

## About

Ishaque Otabil — Final year IT student at the University of Ghana, ALX Cybersecurity Fellow, ISC2 CC candidate and SOC analyst in training.

Connect on [LinkedIn](https://linkedin.com/in/ishaqueotabil)

Following the [MyDFIR 30 Day SOC Analyst Challenge](https://www.youtube.com/@MyDFIR)
