# Day 8 — Introduction to Sysmon

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 8 was a concept day focused on understanding Sysmon — a free Microsoft Sysinternals tool that provides the endpoint visibility that Windows default logging cannot. Before installing Sysmon on Day 9, today was about understanding what it does and why it matters for SOC work.

---

## What is Sysmon?

Sysmon (System Monitor) is a free tool from Microsoft Sysinternals that records high quality telemetry about system activity. Windows has built-in event logging but it misses a lot of detail that is critical for threat detection. Sysmon fills that gap.

It tracks:
- Process creations
- Network connections
- File changes
- DNS queries
- Process access events

It is customisable through configuration files — you control exactly which events get logged and at what level of detail.

---

## Why Sysmon Matters for SOC Work

Sysmon records three things that are invaluable for threat hunting:

| Data Point | Why It Matters |
|---|---|
| Process GUIDs | Correlate events across multiple log entries to build a full timeline |
| Command line arguments | See exactly what commands were run and with what parameters |
| File hashes | Identify malicious files even if they have been renamed |

---

## Key Event IDs

| Event ID | Name | Description |
|---|---|---|
| 1 | Process Creation | Logs every process that starts, including full command line and file hashes |
| 3 | Network Connection | Records source and destination IPs and ports — disabled by default |
| 10 | Process Access | Critical for detecting credential dumping from LSASS memory |
| 22 | DNS Query | Logs every domain the machine resolves — useful for detecting C2 beaconing |

---

## Event ID 10 — Why It Matters

Event ID 10 (Process Access) is one of the most important events for detecting attacks. When an attacker attempts credential dumping — reading from LSASS (Local Security Authority Subsystem Service) memory to steal password hashes — this event fires. Without Sysmon this activity is nearly invisible in default Windows logs.

---

## Event ID 22 — DNS Queries

Every time a machine tries to resolve a domain name Sysmon logs it. This is how malware beaconing to a C2 server gets detected — the infected machine will make regular DNS queries to an unusual or unknown domain. In Kibana these show up as a pattern that stands out immediately.

---

## Why Default Windows Logging Isn't Enough

Default Windows Event Logs capture basic activity but miss:
- The full command line of processes that were launched
- File hashes of executables
- Network connection details
- Process-to-process access (critical for detecting credential theft)

Sysmon captures all of this and feeds it into the pipeline where Elastic Agent forwards it to Elasticsearch for analysis in Kibana.

---

## Key Takeaways

- Sysmon is free and essential for meaningful Windows endpoint visibility
- Configuration files control what gets logged — the Olaf Hartong modular config is widely recommended
- Event IDs 1, 3, 10 and 22 are the most critical for SOC threat hunting
- Without Sysmon, many real attacks would be invisible in Windows logs

---

*Day 8 of 30 complete.*
