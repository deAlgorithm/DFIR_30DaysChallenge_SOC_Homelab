# Day 9 — Sysmon Installation & Configuration

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 9 was the first clean installation day in a while — no errors, no troubleshooting. The focus was installing Sysmon on the Windows Server using the Olaf Hartong modular configuration file to enable high quality endpoint telemetry.

---

## What Was Done

### Preparation
- Updated Vultr firewall rule with current IP — now a routine step at the start of every session due to dynamic IP assignment
- Connected to Windows Server via RDP

### Downloading Sysmon
- Downloaded Sysmon version 15.15 from the Microsoft Sysinternals website
- Navigated to the Olaf Hartong modular config on GitHub
- Viewed the `sysmonconfig.xml` file as raw and saved it directly into the Sysmon directory

### Baseline Verification
Before installing anything, opened **Services** and **Event Viewer** to confirm Sysmon was not present. This establishes a baseline so you know exactly what changed after installation.

**Result:** Sysmon was absent — as expected.

### Installation
Ran the install command in PowerShell as Administrator from the Sysmon directory:

```
.\Sysmon64.exe -i sysmonconfig.xml
```

### Verification
- Opened **Services** — Sysmon service was present and running
- Opened **Event Viewer → Applications and Services Logs → Microsoft → Windows → Sysmon → Operational**
- Logs were already flowing immediately after installation
- First log entry visible was **Event ID 3** (Network Connection) — expected since the server is exposed to the internet with RDP open

---

## Why the Olaf Hartong Config?

Sysmon without a configuration file logs everything — which generates enormous amounts of noise. The Olaf Hartong modular config is a community-maintained configuration that balances comprehensive coverage with practicality. It logs what matters for threat detection without overwhelming analysts with irrelevant events.

---

## Key Takeaways

- Always verify baseline before making changes — confirm absence before installation
- The Olaf Hartong config is the community standard for Sysmon configuration
- Sysmon starts generating logs immediately — Event ID 3 appeared within seconds on an internet-exposed server
- Dynamic IP firewall updates are now a standard routine before every session

---

## Stack Status

| Component | Status |
|---|---|
| Elasticsearch | ✅ Active and running |
| Kibana | ✅ Active and running |
| Fleet Server | ✅ Healthy |
| Windows Agent | ✅ Enrolled and healthy |
| Sysmon | ✅ Installed and running |

**Next:** Ingesting Sysmon and Defender logs into Elasticsearch.

---

*Day 9 of 30 complete.*
