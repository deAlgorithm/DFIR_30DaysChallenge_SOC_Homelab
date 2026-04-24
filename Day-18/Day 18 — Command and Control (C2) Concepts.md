# Day 18 — Command and Control (C2) Concepts

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 18 was a concept day covering Command and Control — how attackers maintain communication with compromised systems after gaining initial access.

---

## What is Command and Control?

Based on the **MITRE ATT&CK framework**, C2 consists of techniques adversaries use to communicate with systems they have compromised. This persistent channel allows attackers to steal credentials, move laterally, deploy ransomware and maintain long-term access.

---

## Common C2 Frameworks

| Framework | Description |
|---|---|
| Metasploit | Foundational exploitation framework — most widely taught |
| Cobalt Strike | Commercial tool most widely encountered in real incidents |
| Sliver | Open source alternative to Cobalt Strike — HTTP/S, DNS, mTLS |
| Mythic | Go-based framework with web UI — used in this challenge |

---

## C2 Indicators to Watch For

| Indicator | Description |
|---|---|
| Sysmon Event ID 22 | DNS queries to unusual domains — C2 beaconing |
| Sysmon Event ID 3 | Unexpected outbound network connections |
| Regular beacon intervals | C2 callbacks at consistent time intervals |
| Sysmon Event ID 10 | LSASS access — credential dumping |

---

## Key Takeaways

- C2 is how attackers maintain persistent access after initial compromise
- Cobalt Strike is the most important framework for SOC analysts to understand defensively
- Mythic will be used for hands-on C2 simulation in coming days
- Understanding C2 from the attacker side directly improves detection on the defender side

---

*Day 18 of 30 complete.*
