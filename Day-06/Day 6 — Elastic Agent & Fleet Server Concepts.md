# Day 6 — Elastic Agent & Fleet Server Concepts

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 6 was a concept day — no infrastructure touched. The focus was understanding two components that tie the entire lab together: the Elastic Agent and Fleet Server. These are the pieces that connect the target machines to the SIEM.

---

## Elastic Agent

A unified agent that replaces the need for multiple individual Beats. Instead of installing Winlogbeat for Windows logs, Filebeat for log files and Metricbeat for system metrics separately, the Elastic Agent handles all of it from a single installation.

It works based on **policies** — you define what data to collect and where to send it, and the agent follows that policy automatically.

---

## Beats vs Elastic Agent

On Day 2 all the individual Beat types were covered. The Elastic Agent is the modern replacement for all of them:

| Beats Approach | Elastic Agent Approach |
|---|---|
| Install one Beat per data type | Install one agent for everything |
| Configure each Beat separately | Manage everything from one policy |
| Update each Beat individually | Push updates centrally via Fleet |

For most use cases the Elastic Agent is the better choice going forward. The MyDFIR challenge uses the Elastic Agent exclusively.

---

## Fleet Server

The central controller that connects all Elastic Agents back to the Elastic Stack. Without Fleet Server, each agent would need to be configured individually on every machine.

**What Fleet Server enables:**
- Push policy updates to all agents at once from one place
- Remote configuration without SSHing into each machine
- Enrollment management — agents check in with Fleet Server to receive their instructions
- Visibility into agent health and status from Kibana

In the context of this lab, Fleet Server sits inside the VPC and manages the Elastic Agents running on the Windows Server and Ubuntu Server — both of which sit outside the VPC.

---

## How They Work Together

```
Windows Server (Elastic Agent)
        ↓ checks in with
Fleet Server (inside VPC)
        ↓ reports to
Elasticsearch + Kibana
```

The agent collects logs on the endpoint, Fleet Server manages the agent's policy and enrollment, and Elasticsearch stores everything for Kibana to display.

---

## Key Takeaways

- Elastic Agent = one agent to replace all Beats
- Fleet Server = central management for all agents
- Policies define what each agent collects and where it sends data
- This architecture scales — adding a new endpoint means enrolling one agent, not configuring multiple Beats

---

*Day 6 of 30 complete.*
