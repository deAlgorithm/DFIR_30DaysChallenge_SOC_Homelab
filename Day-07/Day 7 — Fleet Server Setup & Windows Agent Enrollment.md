# Day 7 — Fleet Server Setup & Windows Agent Enrollment

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 7 was the most technically challenging day so far. The goal was to deploy the Fleet Server and enroll the Windows Server agent into Fleet. What looked straightforward on paper turned into a series of errors that required methodical troubleshooting to resolve.

---

## What Was Done

### Fleet Server Deployment
- Deployed a new Ubuntu server on Vultr to act as the Fleet Server
- Generated a Fleet Server policy in Kibana using the server's public IP
- SSHed into the new server to install and enroll the Fleet Server agent

### Windows Agent Enrollment
- Generated an enrollment token in Kibana Fleet for the Windows policy
- Installed the Elastic Agent on the Windows Server via PowerShell
- Troubleshot connectivity and enrollment issues
- Eventually achieved a healthy enrolled Windows agent in Kibana Fleet

---

## Errors Encountered & Resolutions

### Error 1 — Wrong Architecture Package
**Problem:** Downloaded `elastic-agent-9.3.1-linux-arm64` but the Fleet Server is `x86_64`.

**Error message:**
```
./elastic-agent: 1: ELF+@8: not found
Syntax error: word unexpected (expecting ")")
```

**Resolution:** Checked architecture with `uname -m` which returned `x86_64`. Downloaded the correct `elastic-agent-9.3.1-linux-x86_64` package and re-ran the install command.

---

### Error 2 — Windows Agent Not Appearing in Kibana
**Problem:** Elastic Agent service was running on Windows Server and port 8220 was reachable but the agent never appeared in Kibana Fleet.

**Investigation:** Read the agent logs:
```powershell
Get-Content "C:\Program Files\Elastic\Agent\data\elastic-agent-*\logs\elastic-agent-*.ndjson" -Tail 50
```

**Root cause found in logs:**
```
dial tcp 127.0.0.1:9200: connectex: No connection could be made
```

The agent was trying to connect to `127.0.0.1` (localhost) instead of the actual Elasticsearch server IP. The agent was running in standalone mode — it was never properly enrolled into Fleet.

---

### Error 3 — Re-enrollment Failures
**Problem:** Multiple attempts to re-enroll the agent failed due to:
- Uninstall path errors — must run from outside the installation directory
- Service socket errors during enrollment
- Service timeout on reinstall

**Resolution attempts:**
- Tried editing config files — not possible as the running config is encrypted in `fleet.enc`
- Tried re-enrolling with `enroll` command — failed due to service not running
- Tried uninstalling from wrong directory — failed

---

### Final Resolution — Clean Start
1. Unenrolled both broken Windows agent entries from Kibana Fleet
2. Deleted the Windows policy entirely
3. Removed the Elastic Agent installation from the Windows Server
4. Created a fresh Windows policy in Kibana
5. Generated a new enrollment token
6. Ran a clean install with `--insecure` flag added:

```powershell
.\elastic-agent.exe install --url=https://155.138.228.224:8220 --enrollment-token=YOUR_TOKEN --insecure
```

The `--insecure` flag is required because the Fleet Server uses a self-signed certificate. This was shown in the tutorial but missed on the first attempt.

---

## Firewall Ports Opened

| Server | Port | Purpose |
|---|---|---|
| ELK Server | 9200 | Elasticsearch API access |
| Fleet Server | 8220 | Fleet Server communication |
| Fleet Server | 443 | HTTPS |

---

## Key Takeaways

- **Always check architecture** before downloading packages — `uname -m` confirms x86_64 vs arm64
- **Read the logs** — the localhost:9200 error was the root cause, visible immediately in the logs
- **`--insecure` flag is required** when Fleet Server uses a self-signed certificate
- **Know when to start over** — spending time trying to fix a broken enrollment wastes more time than a clean reinstall
- **Two separate firewall layers** — Vultr cloud firewall and Ubuntu UFW must both allow the required ports

---

## Stack Status

| Component | Status |
|---|---|
| Elasticsearch | ✅ Active and running |
| Kibana | ✅ Active and running |
| Fleet Server | ✅ Healthy |
| Windows Agent | ✅ Enrolled and healthy |

**Next:** Ubuntu Server agent enrollment.

---

*Day 7 of 30 complete.*
