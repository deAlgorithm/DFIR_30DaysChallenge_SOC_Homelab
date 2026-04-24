# Day 16 — RDP Brute Force Detection in Kibana

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 16 was about building brute force detection rules for the Windows Server in Kibana — turning raw RDP failed login data into automated alerts.

---

## What Was Done

### Preparation
Updated Vultr firewall rule — dynamic IP had changed again.

### Querying RDP Failed Logins

Filtered logs by Windows agent and searched for Event ID 4625:
```
event.code: "4625"
```
Saved the search as **"RDP Failed Activity"** for quick triage access.

### Detection Rules Built

**Rule 1 — Basic Threshold Rule:** Triggers when failed logons cross a set number within a time window.

**Rule 2 — Advanced Detection Rule:**
- Scoped to Event ID 4625, Administrator account, MYDFIR-Windows agent
- Severity: Medium
- Risk score: 47
- Returns source IP, username, timestamp

Both rules fired immediately with real attacker IPs.

---

## Key Event IDs

| Event ID | Meaning |
|---|---|
| 4625 | Failed logon — brute force indicator |
| 4624 | Successful logon |

---

## Key Takeaways

- Event ID 4625 is the starting point for all RDP brute force detection
- Threshold rules catch volume attacks, advanced rules catch targeted attacks
- Alerts fired immediately — server was actively being attacked at rule creation time

---

*Day 16 of 30 complete.*
