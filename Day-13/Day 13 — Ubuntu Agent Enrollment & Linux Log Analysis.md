# Day 13 — Ubuntu Agent Enrollment & Linux Log Analysis

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 13 was about enrolling the Ubuntu Server into Fleet and seeing real attack traffic in both the terminal and Kibana Discover. The server had been live since Day 12 and by Day 13 it had already accumulated dozens of brute force attempts from real external IPs.

---

## What Was Done

### Preparation
Updated Vultr firewall rule with current IP — standard routine before every session.

### Elastic Agent Installation — Architecture Issue Again
Attempted to install the Elastic Agent on the Ubuntu Server. Hit the same architecture mismatch as the Fleet Server installation — had downloaded the `arm64` package on an `x86_64` server.

**Resolution:**
```bash
uname -m
# returned x86_64

rm -rf elastic-agent-9.3.2-linux-arm64
rm elastic-agent-9.3.2-linux-arm64.tar.gz

wget https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-9.3.2-linux-x86_64.tar.gz
tar -xzf elastic-agent-9.3.2-linux-x86_64.tar.gz
cd elastic-agent-9.3.2-linux-x86_64

sudo ./elastic-agent install --url=https://155.138.228.224:8220 --enrollment-token=TOKEN --insecure
```

### Verification
- Ubuntu agent appeared as **Healthy** in Kibana Fleet
- Linux logs were immediately visible in Kibana Discover

---

## Auth Log Analysis

### Filtering Failed Login Attempts
```bash
grep -i failed auth.log | grep -i root | cut -d ' ' -f 9
```

**Result:** Multiple IPs returned including `161.82.250.31` and `14.18.108.138` — automated brute force attempts against root.

### Kibana Discover Verification
**Query:** `111.228.24.82 and authentication failure`
**Result:** 82 documents. IP `111.228.24.82` actively hammering the server with SSH brute force attempts.

---

## Key Takeaways

- Always check architecture with `uname -m` before downloading packages
- Real attack telemetry is more valuable for learning than simulated data
- SSH brute force from multiple IPs begins within hours of server deployment
- Kibana Discover is the first place to verify logs are flowing correctly

---

*Day 13 of 30 complete.*
