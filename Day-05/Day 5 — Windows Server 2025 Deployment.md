# Day 5 — Windows Server 2025 Deployment

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 5 was about deploying the first target machine — a Windows Server instance on Vultr with RDP intentionally exposed to the internet. This machine will generate real attack logs from brute force attempts and unauthorised access that will later be analysed in Kibana.

---

## Architecture Update

The diagram was updated this day to reflect that both the Windows Server and Ubuntu Server sit **outside the VPC**. This is a deliberate security isolation decision — if either target machine is compromised, the attacker cannot pivot directly into the VPC where Elasticsearch and Kibana live.

![Updated Architecture Diagram](architecture_diagram_v2.png)

---

## What Was Done

### Deploying the Server
- Deployed a **Windows Server 2025** Cloud Compute instance on Vultr
- The tutorial uses Windows Server 2022 — 2025 was what was available on Vultr at the time of this challenge

### Network Configuration
- **No VPC selected** — the Windows Server is deliberately kept outside the VPC
- This isolates it from the core infrastructure (Elasticsearch, Kibana, Fleet Server)
- If the server is compromised, the attacker has no direct path into the private network

### Firewall Settings
- Firewall group left blank — the server is fully exposed to the public internet
- This is intentional — RDP exposure generates real failed login attempts from automated scanners and bots around the world, producing authentic attack logs for analysis

### Accessing the Server
- Used the **Vultr console** to access the server directly from the browser
- Retrieved the initial administrator password from the console
- Confirmed the server was running and accessible

---

## Why Expose RDP to the Internet?

Leaving RDP open to the internet is intentional in this lab context. Within minutes of deployment, automated bots and scanners across the internet will attempt to brute force the RDP login. These failed login attempts generate **Windows Event ID 4625** (failed logon) entries — exactly the kind of attack telemetry that SOC analysts hunt for in real environments.

This gives the lab authentic attack data to work with rather than simulated logs.

---

## Version Difference from Tutorial

| Component | Tutorial (Video) | This Lab |
|---|---|---|
| Windows Server | 2022 | 2025 |

Same setup process, newer version.

---

## Key Takeaways

- **Isolation by design** — exposed attack targets should never share a network with core security infrastructure
- **Real attack data starts immediately** — RDP exposed to the internet attracts brute force attempts within minutes
- **Vultr console** provides direct browser-based access without needing RDP credentials initially

---

## Stack Status

| Component | Status |
|---|---|
| Elasticsearch | ✅ Active and running |
| Kibana | ✅ Active and running |
| Windows Server 2025 | ✅ Deployed, RDP exposed |

**Next:** Fleet Server installation and Elastic Agent deployment.

---

*Day 5 of 30 complete.*
