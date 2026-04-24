# Day 17 — RDP Authentication Maps & Dashboard

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 17 was about building geospatial maps for RDP authentication and completing the dashboard with 6 new visualisations — 2 maps and 4 tables.

---

## What Was Done

### Accessing the Saved RDP Search
Version difference from tutorial: saved search was found under **"Open Discover session"** instead of a dedicated saved searches menu.

### RDP Failed Authentication Map
Built using Event ID 4625. One IP from France — `91.238.181.134` — had attempted **45,358 logins** against the Administrator account.

### RDP Successful Authentication Map
Built using:
```
event.code: "4624" and (winlog.event_data.LogonType: 10 or winlog.event_data.LogonType: 7)
```

| LogonType | Meaning |
|---|---|
| 10 | RemoteInteractive — RDP session |
| 7 | Unlock — interactive session resumed |

Result: Only Ghana. Every successful RDP login is the analyst.

### Dashboard — 8 Panels Total
SSH Failed Auth map, SSH Successful Auth map, RDP Failed Auth map, RDP Successful Auth map, plus 4 tables covering top usernames, IPs and countries for all authentication events.

---

## Key Takeaways

- A single French IP attempted 45,358 RDP logins
- LogonType 10 and 7 are the correct filters for remote RDP sessions
- Tables provide investigation detail, maps provide visual context

---

*Day 17 of 30 complete.*
