# Day 12 — Ubuntu Server Deployment & Auth Log Analysis

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 12 was about deploying the Ubuntu Server — the second target machine in the lab — and getting a first look at real attack traffic in the authentication logs. The server was live for minutes before failed login attempts started appearing.

---

## What Was Done

### Preparation
Updated Vultr firewall rule with current IP — standard routine before every session.

### Server Deployment
- Deployed a new **Ubuntu Server 24.04** Cloud Compute instance on Vultr
- Set hostname following the challenge naming convention
- Connected via SSH through PowerShell

### System Update
Updated all package repositories:

```bash
apt-get update && apt-get upgrade -y
```

### Auth Log Analysis
Navigated to the Linux authentication log:

```bash
cat /var/log/auth.log
```

This file records every authentication attempt on the server — successful logins, failed logins, sudo usage and SSH activity.

**What was visible immediately:** Failed login attempts from external IP addresses. Automated bots scanning the internet for open SSH ports had already found the server and were attempting to brute force the root account.

Used `grep` to filter for failed attempts:

```bash
grep "Failed password" /var/log/auth.log
```

---

## What is /var/log/auth.log?

The auth.log file is the primary security log on Ubuntu/Debian Linux systems. It records:

| Entry Type | Description |
|---|---|
| Failed password | Failed login attempt — key indicator of brute force |
| Accepted password | Successful login |
| Invalid user | Login attempt for a username that doesn't exist |
| sudo | Privilege escalation events |
| Disconnected | Session termination |

---

## Why Failed Logins Appear Immediately

The Ubuntu Server has SSH (port 22) exposed to the internet with no firewall restrictions. Automated scanners constantly probe the internet looking for open SSH ports. Within minutes of a new server going live:

- Bots identify the open port
- Automated brute force tools begin testing common usernames and passwords
- Failed login attempts flood the auth.log

This is intentional in the lab — the goal is to generate real attack telemetry that can be analysed in Kibana. These are not simulated logs — they are actual malicious actors attempting to compromise the server.

---

## Key Takeaways

- `/var/log/auth.log` is the first place to look for suspicious activity on a Linux server
- Internet-exposed SSH servers receive brute force attempts within minutes of going live
- `grep` is a powerful tool for filtering specific events from large log files
- Real attack traffic is more valuable for learning detection than simulated data

---

## Stack Status

| Component | Status |
|---|---|
| Elasticsearch | ✅ Active and running |
| Kibana | ✅ Active and running |
| Fleet Server | ✅ Healthy |
| Windows Server | ✅ Running, RDP exposed |
| Sysmon | ✅ Installed and running |
| Sysmon logs in Elastic | ✅ Flowing |
| Defender logs in Elastic | ✅ Flowing |
| Ubuntu Server | ✅ Deployed, SSH exposed |

**Next:** Installing Elastic Agent on the Ubuntu Server and ingesting Linux logs.

---

*Day 12 of 30 complete.*
