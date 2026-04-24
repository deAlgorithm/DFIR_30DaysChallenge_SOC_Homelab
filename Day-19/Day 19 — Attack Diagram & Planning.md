# Day 19 — Attack Diagram & Planning

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** April 2026

---

## Overview

Day 19 was about planning the full attack simulation before executing it. Using draw.io, a complete attack lifecycle diagram was created mapping all six phases of the simulated attack against the Windows Server.

---

## The Attack Diagram — Six Phases

### Phase 1 — Initial Access
- **Method:** RDP Brute Force from Kali Linux against the Windows Server
- **Detection:** Event ID 4625 (failed attempts) then 4624 (success)

### Phase 2 — Discovery
- **Commands:** whoami, ipconfig, net user, net group
- **Detection:** Sysmon Event ID 1 (process creation with command line)

### Phase 3 — Defense Evasion
- **Action:** Disable Windows Defender real-time protection
- **Detection:** Event ID 5001 (Defender disabled)

### Phase 4 — Execution
- **Method:** PowerShell IEX downloads and runs Mythic agent from C2 server
- **Detection:** Sysmon Event ID 1 (PowerShell IEX), Event ID 3 (network connection)

### Phase 5 — Command and Control
- **Result:** Stable C2 session between Windows Server and Mythic
- **Detection:** Sysmon Event ID 3 (regular outbound connections), Event ID 22 (DNS queries)

### Phase 6 — Exfiltration
- **Target:** passwords.txt in Documents directory
- **Method:** Download via active C2 session to Mythic server
- **Detection:** Sysmon Event ID 3 (data transfer)

---

## MITRE ATT&CK Mapping

| Phase | Technique | Key Event |
|---|---|---|
| Initial Access | T1110 Brute Force | Event ID 4625, 4624 |
| Discovery | T1033, T1016 | Sysmon ID 1 |
| Defense Evasion | T1562 | Event ID 5001 |
| Execution | T1059 | Sysmon ID 1 |
| C2 | T1071 | Sysmon ID 3, 22 |
| Exfiltration | T1041 | Sysmon ID 3 |

---

## Key Takeaways

- Planning the attack before executing it forces methodical thinking about each phase
- Every phase generates specific telemetry — the diagram becomes a detection checklist
- MITRE ATT&CK mapping connects attacker behaviour to defender detection logic

---

*Day 19 of 30 complete.*
