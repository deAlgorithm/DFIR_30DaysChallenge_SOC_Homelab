# Day 15 — Introduction to RDP & Identifying Exposed Servers

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 15 was a concept day focused on Remote Desktop Protocol — what it is, how attackers abuse it and how to identify exposed RDP servers using Shodan and Censys.

---

## What is RDP?

Remote Desktop Protocol (RDP) runs on **port 3389** and allows users to remotely control a Windows machine. It is widely used but one of the most abused protocols in cyberattacks.

**How attackers abuse RDP:**
- Brute force attacks against exposed RDP services
- Using stolen credentials to gain initial access
- Credential dumping and lateral movement once inside

---

## Shodan and Censys Results

| Platform | Query | Results |
|---|---|---|
| Shodan | port:3389 | 2,469,737 exposed globally |
| Censys | port 3389 global | 2,600,000+ hosts |
| Censys | port 3389 Ghana | 341 exposed servers |

Notable Ghanaian organisations found with RDP exposed: Regional Maritime University, COTVET, Ministry of Gender, Registrar General's Department, ARB Apex Bank, University of Mines and Technology.

---

## How to Protect Against RDP Attacks

- Disable RDP if not needed
- Put it behind a VPN or firewall
- Enable MFA
- Use strong passwords, avoid default Administrator accounts
- Monitor for Event ID 4625

---

## Key Takeaways

- Over 5 million exposed RDP servers exist globally across both platforms
- 341 exposed RDP servers in Ghana alone — universities, banks, government agencies
- Ghana's Cybersecurity Act 2020 (Act 1038) exists precisely because of this exposure

---

*Day 15 of 30 complete.*
