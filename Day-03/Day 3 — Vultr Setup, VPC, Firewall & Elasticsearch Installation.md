# Day 3 — Vultr Setup, VPC, Firewall & Elasticsearch Installation

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 3 was the first hands-on day of the challenge. The focus was provisioning the Vultr cloud environment, configuring the network and firewall, and installing Elasticsearch on the server via SSH. Several real-world issues were encountered and resolved along the way.

---

## Environment Setup — Vultr Dashboard

### VPC Configuration
Created a Virtual Private Cloud (VPC) on Vultr to isolate the lab environment:
- **Network:** 172.31.0.0/24
- **Subnet mask:** 255.255.255.0
- **Usable hosts:** 254

### Firewall Rules
Configured a Vultr firewall group to restrict SSH access (port 22) to the analyst's IP address only. All other inbound traffic is dropped by default.

> **Key lesson:** The ISP assigns a dynamic IP address that changes on every new internet connection. The firewall rule must be updated with the current IP each session before SSH will work.

### Hostname
Set server hostname to `MyDFIR` to match the challenge naming convention.

---

## Elasticsearch Installation — Terminal over SSH

### System Update
Ran `apt-get update && apt-get upgrade -y` before installing packages.

**Issue encountered:** Package manager lock error on first attempt — the server was running automatic background updates post-provisioning. Resolved by waiting for the background process to complete and clearing stale lock files.

```bash
rm /var/lib/dpkg/lock-frontend
rm /var/lib/dpkg/lock
rm /var/cache/apt/archives/lock
dpkg --configure -a
```

### Installation
Downloaded and installed **Elasticsearch 9.3.1** — a full major version ahead of what the MyDFIR challenge video references (8.x).

### Key Change in Version 9
Elasticsearch 9.x enables **TLS security by default** on all connections. Unlike version 8.x in the video:
- Plain HTTP connections to port 9200 are not permitted
- All interactions require the CA certificate and credentials
- The auto-generated superuser password is shown **once** during installation — save it immediately

### Configuration
Edited `/etc/elasticsearch/elasticsearch.yml` to bind the network host to the server's public IP:

```yaml
network.host: YOUR_SERVER_IP
```

> **Pain point:** A typo in the IP address (one extra digit) caused Elasticsearch to fail on startup. Identified by reading the error logs at `/var/log/elasticsearch/elasticsearch.log`.

### Starting the Service

```bash
systemctl daemon-reload
systemctl enable elasticsearch.service
systemctl start elasticsearch.service
systemctl status elasticsearch.service
```

Expected output:
```
Active: active (running)
```

---

## Pain Points

| Issue | Resolution |
|---|---|
| Package manager lock on first run | Waited for background process, cleared lock files |
| Broken openssh-server package | Reinstalled with apt-get |
| Typo in network.host IP | Found in Elasticsearch logs, corrected in yml file |
| SSH timeout after firewall setup | Dynamic IP had changed — updated firewall rule |

---

## Key Takeaways

- **Always read the logs** — every error in this session was explained clearly in the log output
- **Tutorials go stale** — version 9.x has real configuration differences from the 8.x video
- **Dynamic IPs require firewall rule updates** on every reconnection
- **Two firewall layers** — Vultr cloud firewall and Ubuntu UFW operate independently

---

*Day 3 of 30 complete.*
