# Day 2 — ELK Stack Deep Dive & Splunk Comparison

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 2 was a full research day. No infrastructure was touched. The focus was going deeper into each ELK Stack component and understanding how they compare to Splunk — the other major SIEM platform used in enterprise SOC environments.

---

## Splunk vs ELK — Component Mapping

If you already know Splunk, this mapping makes ELK immediately familiar:

| ELK Component | Splunk Equivalent | What It Does |
|---|---|---|
| Beats | Universal Forwarder | Sits on source machines and ships raw data |
| Logstash | Heavy Forwarder | Parses, transforms and routes data before sending it on |
| Elasticsearch | Indexer | Stores, indexes and responds to search queries |
| Kibana | Search Head + Web UI | Front end for search, investigation and dashboards |

**Key difference:** Splunk is proprietary and expensive. ELK is open source and you build it yourself. Same job, different tools.

---

## ELK Component Deep Dive

### Elasticsearch
The database at the heart of the stack. Stores all log types — Windows events, syslog, firewall logs. Search is powered by **ESQL** (Elasticsearch Query Language) and everything communicates through **RESTful APIs**.

### Logstash
The data pipeline. Collects, transforms and filters data before pushing it into Elasticsearch. Supports filtering by specific **Event IDs** — so only relevant events are stored. This reduces ingestion costs and noise.

### Kibana
The visualisation and investigation layer:
- **Kibana Lens** — drag-and-drop dashboard building
- **Anomaly detection** — built-in machine learning
- **Alerting** — configure alerts to fire on specific conditions
- All accessible from a browser

---

## Beats — Types and Uses

Beats are lightweight agents installed on machines to collect specific data types and forward them to Elasticsearch:

| Beat | Purpose |
|---|---|
| Winlogbeat | Windows event logs |
| Filebeat | Log files from the filesystem |
| Metricbeat | System and service metrics |
| Packetbeat | Network traffic |
| Auditbeat | Linux audit framework data |
| Heartbeat | Service uptime monitoring |

> **Note:** The MyDFIR challenge uses the **Elastic Agent** — a unified agent that replaces individual Beats and is managed centrally through Fleet Server.

---

## Key Takeaway

Understanding the Splunk-to-ELK mapping made everything click. Both platforms solve the same problem — centralised log collection, storage, search and visualisation for security operations. The tooling differs but the concepts are identical. Any analyst familiar with one will find the other approachable.

---

*Day 2 of 30 complete.*
