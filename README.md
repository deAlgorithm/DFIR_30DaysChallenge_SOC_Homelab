# 30 Days SOC Home Lab Challenge — MyDFIR

A public documentation of my 30-day SOC Analyst home lab journey, following the **MyDFIR 30 Day SOC Analyst Challenge**. Built from scratch on Vultr cloud infrastructure, simulating real attacks and detecting them using the ELK Stack.

---

## What This Is

This repository documents every day of the challenge — what I built, what I learned, what broke and how I fixed it. The goal is to build a fully functional SOC home lab environment that mirrors how real security operations centers work.

---

## Lab Architecture

| Component | Role | Platform |
|---|---|---|
| Elasticsearch + Kibana | SIEM — log storage, search, dashboards | Vultr VM |
| Fleet Server | Centrally manages Elastic Agents | Vultr VM |
| Windows Server (RDP) | Attack target — Windows event logs | Vultr VM |
| Ubuntu Server (SSH) | Attack target — Linux audit logs | Vultr VM |
| Kali Linux | Attack machine | External |
| Mythic C2 | Command & Control framework | External |

> Note: Architecture components may change as the challenge progresses. Any changes will be documented and explained.

---

## Progress

| Day | Topic | Status |
|---|---|---|
| Day 1 | System design & architecture | ✅ Done |
| Day 2 | ELK Stack deep dive & Splunk comparison | ✅ Done |
| Day 3 | Vultr setup, VPC, firewall, Elasticsearch installation | ✅ Done |
| Day 4 | Kibana installation & configuration | ✅ Done |
| Day 5 | Windows Server 2025 deployment | ✅ Done |
| Day 6 | Coming soon | 🔄 In progress |

---

## Tools & Technologies

- Elasticsearch 9.x
- Kibana
- Elastic Agent / Beats
- Fleet Server
- Vultr Cloud
- Ubuntu Server
- Windows Server
- Kali Linux
- Mythic C2 (Command & Control)
- UFW (Uncomplicated Firewall)

---

## Daily Documentation

Each day has its own folder with a Word document covering:
- What was done
- What was learned
- Pain points and how they were resolved

---

## About

I'm Ishaque Otabil — an IT student at the University of Ghana and ALX Cybersecurity Fellow, working towards a career as a SOC Analyst.

This challenge is my way of building real, hands-on experience before stepping into the field.

Connect with me on [LinkedIn](https://www.linkedin.com/in/ishaque-otabil)

---

*Following the MyDFIR 30 Day SOC Analyst Challenge*
