# Day 1 — System Design & Architecture

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 1 was not about touching a single command. It was about understanding what I am building before I build it. The focus was on conceptualising the full SOC home lab environment, understanding each component and its role, and designing the system architecture diagram that will guide the next 29 days.

---

## What I Learned Today

### The ELK Stack

ELK stands for Elasticsearch, Logstash and Kibana. Three tools that work together to solve one problem — how do you keep track of everything happening across multiple machines without logging into each one manually.

- **Logstash** collects logs from all your different sources and forwards them into Elasticsearch
- **Elasticsearch** stores and indexes everything so you can search through millions of log entries in seconds
- **Kibana** sits on top and gives you dashboards, visualisations and alerts so you can make sense of what you're looking at

In a SOC environment this is how analysts catch threats. Instead of waiting for something to break they're watching logs in real time, spotting unusual patterns and investigating before things get worse.

### The Architecture I Designed

Rather than a basic ELK setup, the lab is a full SOC simulation environment hosted on Vultr cloud. The architecture has four distinct layers:

![SOC Home Lab Architecture](architecture_diagram.png)

| Layer | Component |
|---|---|
| External | SOC Analyst, Kali Linux attack laptop, Mythic C2 server |
| Internet | Shared highway — all external traffic passes through here |
| Vultr Cloud | Cloud gateway — single public entry point |
| VPC (172.31.0.0/24) | Private network containing all lab components |

### Components Inside the VPC

| Component | Role |
|---|---|
| Kibana + Elasticsearch | SIEM — receives logs, indexes them, provides dashboards and alerting |
| Ticket Server | Receives alerts from Elastic, simulates incident management |
| Fleet Server | Centrally manages Elastic Agents on all target machines |
| Windows Server (RDP) | Intentional attack target — Windows event logging |
| Ubuntu Server (SSH) | Intentional attack target — Linux audit logging |

### The Two Flows That Make This a SOC Lab

**Red team flow:** Attacker or C2 → Internet → Cloud gateway → VPC → Windows via RDP or Ubuntu via SSH

**Blue team flow:** Elastic Agents capture activity → forward to Elasticsearch → surface in Kibana → alerts fire → tickets created

---

## Key Terms

| Term | Definition |
|---|---|
| Telemetry | Automatic data reporting from machines. Elastic Agent ships logs continuously without manual intervention |
| SIEM | Security Information and Event Management. Central platform for log collection, correlation and threat detection |
| Elastic Agent | Lightweight program installed on each machine that watches activity and forwards data to Elasticsearch |
| Fleet Server | Central controller that manages all Elastic Agents from one place |
| Indexing | How Elasticsearch organises incoming logs for fast search |
| C2 (Command & Control) | Attacker's remote control server. Mythic is the C2 framework used in this lab |
| Implant | Malware secretly placed on a compromised machine, beacons back to the C2 server |
| Beaconing | Regular check-ins from an implant to its C2 — a key detection signal |
| VPC | Virtual Private Cloud — logically isolated private network within Vultr |
| RDP | Remote Desktop Protocol — Microsoft's remote control tool, exposed as an attack surface |
| SSH | Secure Shell — Linux remote terminal, exposed as an attack surface |
| Brute Force | Trying thousands of passwords automatically until one works |

---

## Resources

- [MyDFIR 30 Day SOC Analyst Challenge](https://www.youtube.com/@MyDFIR)
- [Elastic Documentation](https://www.elastic.co/docs)
- [Mythic C2](https://github.com/its-a-feature/Mythic)
- [Vultr Cloud](https://www.vultr.com)

---

*Day 1 of 30 complete.*
