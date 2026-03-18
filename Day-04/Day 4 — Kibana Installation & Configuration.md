# Day 4 — Kibana Installation & Configuration

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 4 focused on installing and configuring Kibana — the visualisation and investigation layer of the ELK Stack. With Elasticsearch running from Day 3, Kibana was the next component to bring online, following the MyDFIR 30 Day SOC Analyst Challenge tutorial.

---

## What Was Done

### Installation
Visited the official Elastic website, copied the download link for the Kibana deb package and installed it directly on the server via SSH.

### Configuration — kibana.yml
Edited `/etc/kibana/kibana.yml`:

```yaml
server.host: "YOUR_SERVER_PUBLIC_IP"
server.port: 5601
```

### Service Setup

```bash
systemctl daemon-reload
systemctl enable kibana.service
systemctl start kibana.service
systemctl status kibana.service
```

### Firewall Configuration

Port 5601 must be opened in **two separate places**:

**1. Vultr cloud firewall**
- Added inbound rule: TCP port 5601 from analyst's IP

**2. Ubuntu UFW on the VM**
```bash
ufw allow 5601
```

> Both firewall layers must be configured independently. Opening the port in one does not affect the other.

### Enrollment & Security
- Used Elasticsearch credentials from Day 3 to log in to Kibana for the first time
- Generated an **enrollment token** to securely link Kibana to Elasticsearch
- Configured **API keys** to enable dashboard integrations and saved objects functionality

---

## Pain Points

### Kibana unreachable on first attempt
After starting the service, Kibana was not reachable in the browser. The tutorial identified the cause — port 5601 had been opened on the Vultr firewall but not on the VM itself using UFW. Adding the UFW rule resolved it immediately.

### Different error messages from the video
Kibana 9.x displays different error messages and UI flows from what the tutorial video shows. The challenge was recorded a year ago and the interface has changed. Required reading the actual screen rather than following the video step by step.

---

## Key Takeaways

- **Two firewalls, two rules** — Vultr cloud firewall and Ubuntu UFW are independent. Both must allow a port for it to be reachable
- **Enrollment token** links Kibana to Elasticsearch securely
- **API keys** enable dashboard integrations and saved objects
- **Tutorials are guides not scripts** — version differences mean steps look different in practice

---

## Stack Status

| Component | Status |
|---|---|
| Elasticsearch | ✅ Active and running |
| Kibana | ✅ Active and running |

**Next:** Fleet Server installation and Elastic Agent deployment.

---

*Day 4 of 30 complete.*
