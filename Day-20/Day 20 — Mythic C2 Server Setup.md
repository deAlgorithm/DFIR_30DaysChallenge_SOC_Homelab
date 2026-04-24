# Day 20 — Mythic C2 Server Setup

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** April 2026

---

## Overview

Day 20 was about deploying and configuring the Mythic C2 server on a new Ubuntu Vultr instance. Mythic was installed, the web UI explored and the server locked down to the analyst IP only.

---

## What Was Done

Updated firewall first. Deployed new Ubuntu Server on Vultr. SSH'd in and ran:

```bash
apt-get update && apt-get upgrade -y
apt-get install docker-compose make -y
git clone https://github.com/its-a-feature/Mythic.git /opt/Mythic
cd /opt/Mythic
make
./mythic-cli start
```

No errors. Docker and make installed cleanly. Configured Vultr firewall to restrict port 7443 to analyst IP only. Accessed Mythic web UI and retrieved credentials from the .env file.

---

## Mythic Dashboard Sections

| Section | Purpose |
|---|---|
| Payloads | Build and manage agents |
| Active Callbacks | Live C2 sessions |
| Search Files | Files transferred via C2 |
| MITRE ATT&CK | Maps activity to ATT&CK techniques |
| Event Feed | Real-time C2 activity log |

---

## Key Takeaways

- Mythic runs on Docker — straightforward install with Docker Compose and make
- Always restrict C2 server access to trusted IPs only
- MITRE ATT&CK mapping in Mythic directly supports detection work in Kibana
- The lab now has both attack and defence infrastructure running simultaneously

---

*Day 20 of 30 complete.*
