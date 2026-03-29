# Day 10 — Ingesting Sysmon & Defender Logs into Elasticsearch

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 10 was about getting Sysmon and Microsoft Defender logs flowing from the Windows Server into Elasticsearch so they can be investigated in Kibana. This is the point where the lab starts functioning like a real SOC environment — endpoint telemetry reaching the SIEM.

---

## What Was Done

### Preparation
Updated Vultr firewall rule with current IP before starting — this is now a standard routine at the start of every session.

### Adding the Sysmon Integration

Logged into the Elastic web GUI and navigated to **Integrations**.

**Version difference from tutorial:** Searching for "Sysmon" only returned a Linux version. The Windows Sysmon integration no longer exists as a standalone option in Elasticsearch 9.x. Instead used **Custom Windows Event Logs** and manually entered the channel name:

```
Microsoft-Windows-Sysmon/Operational
```

This is the exact channel name found in Event Viewer under:
`Applications and Services Logs → Microsoft → Windows → Sysmon → Operational`

Assigned the integration to the **MYDFIR-Windows-Policy**.

### Adding the Microsoft Defender Integration

Added a second Custom Windows Event Logs integration for Microsoft Defender. Rather than ingesting all Defender events, filtered to specific Event IDs that matter for SOC detection:

| Event ID | Meaning |
|---|---|
| 1116 | Malware detected |
| 1117 | Malware action taken |
| 5001 | Real-time protection disabled |

**Why filter?** Ingesting all Defender events generates a lot of noise. These three Event IDs cover the critical detection scenarios — malware found, malware actioned and protection being turned off (a common attacker technique).

Assigned to **MYDFIR-Windows-Policy**.

### Verification

Navigated to **Kibana → Discover** and confirmed:
- Sysmon logs were actively flowing into Elasticsearch
- Defender logs were actively flowing into Elasticsearch

---

## Key Difference from Tutorial

In the tutorial video a dedicated Sysmon integration exists in the Elastic integrations marketplace. In Elasticsearch 9.x this is no longer available as a standalone option for Windows. The workaround is to use Custom Windows Event Logs and manually specify the channel name — which achieves the same result.

---

## Key Takeaways

- Custom Windows Event Logs integration is the correct approach for Sysmon in Elasticsearch 9.x
- The channel name must match exactly what appears in Windows Event Viewer
- Filtering to specific Event IDs reduces noise and focuses on what matters
- Event IDs 1116, 1117 and 5001 are the critical Defender events for SOC monitoring
- Firewall prep before starting each session prevents connectivity issues

---

## Stack Status

| Component | Status |
|---|---|
| Elasticsearch | ✅ Active and running |
| Kibana | ✅ Active and running |
| Fleet Server | ✅ Healthy |
| Windows Agent | ✅ Enrolled and healthy |
| Sysmon | ✅ Installed and running |
| Sysmon logs in Elastic | ✅ Flowing |
| Defender logs in Elastic | ✅ Flowing |

**Next:** Setting up the Ubuntu Server and ingesting Linux logs.

---

*Day 10 of 30 complete.*
