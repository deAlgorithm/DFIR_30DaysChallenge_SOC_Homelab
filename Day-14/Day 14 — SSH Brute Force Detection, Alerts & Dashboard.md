# Day 14 — SSH Brute Force Detection, Alerts & Dashboard

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 14 was about turning raw SSH logs into actionable detections — querying logs in Kibana Discover, creating automated alerts and building a geospatial dashboard to visualise where attacks are coming from globally.

---

## What Was Done

### Querying SSH Logs in Kibana Discover

Filtered SSH authentication logs using specific fields:

| Field | Purpose |
|---|---|
| `system.auth.ssh.event` | Identifies the type of SSH event |
| `user.name` | The username being targeted |
| `source.ip` | The IP address of the attacker |

### Creating a Brute Force Alert

Set up a **Search Threshold Rule** in Kibana:
- **Trigger:** More than 5 failed SSH login attempts
- **Timeframe:** Within 5 minutes

### Building the Geospatial Maps

**SSH Failed Authentication Map:** Attack attempts from Ukraine, Romania, Iran, Saudi Arabia, Bangladesh, Nigeria and many more countries.

**SSH Successful Authentication Map:** Only Ghana. Every successful login is the analyst.

**Version difference from tutorial:** The tutorial shows a flat 2D choropleth map. Kibana 9.x renders a full interactive spherical globe.

### Dashboard Creation

Combined all visualisations into a single **MYDFIR-Authentication-Activity** dashboard with maps and tables for both failed and successful SSH authentication.

---

## Key Takeaways

- Threshold-based alerting automates detection so analysts don't need to manually watch logs
- Geospatial maps provide immediate visual context for where attacks originate
- The gap between failed and successful logins is the story
- UI changes between versions are expected — concepts remain the same

---

*Day 14 of 30 complete.*
