# Day 11 — Brute Force Attacks: Concepts & Defence

**Challenge:** MyDFIR 30 Day SOC Analyst Challenge
**Date:** March 2026

---

## Overview

Day 11 was a concept day covering brute force attacks — one of the most common attack types targeting internet-exposed services. With RDP open on the Windows Server and SSH open on the Ubuntu Server, understanding brute force attacks is directly relevant to what is happening in this lab right now.

---

## What is a Brute Force Attack?

A brute force attack is when an attacker systematically tries every possible password combination until they find the correct one. Think of it like trying every combination on a luggage lock — given enough time and attempts, any lock will eventually open.

Modern brute force attacks are automated, meaning attackers can test thousands of combinations per second using tools running on powerful hardware or botnets.

---

## Types of Brute Force Attacks

### Simple Brute Force
Automated trial and error that tests every possible character combination. Slow but guaranteed to eventually succeed against weak passwords. Effective against short passwords — a 6-character password can be cracked in seconds.

### Dictionary Attack
Instead of random combinations, the attacker uses a wordlist of common passwords, phrases, names and previously leaked credentials. Much faster than pure brute force because most people use predictable passwords. Tools like `rockyou.txt` — a massive leaked password list — are commonly used.

### Credential Stuffing
Uses stolen username and password pairs from one data breach to attempt logins on completely different services. Highly effective because people reuse passwords across multiple accounts. If your email and password leaked from one site and you use the same combination elsewhere, attackers will find it.

---

## Common Attacker Tools

| Tool | Purpose |
|---|---|
| Hydra | Network login cracker — attacks SSH, RDP, FTP, HTTP and more |
| Hashcat | Password hash cracking — extremely fast using GPU acceleration |
| John the Ripper | Password cracker — works on a wide range of hash types |

All three are available in Kali Linux and will be used on the attack side of this lab later in the challenge.

---

## How to Defend Against Brute Force Attacks

### Long Passwords and Passphrases
Use passwords of 15 or more characters. A passphrase like `correct-horse-battery-staple` is both memorable and extremely resistant to brute force. Use a password manager — you only need to remember one master password.

### Multi-Factor Authentication (MFA)
MFA means even if an attacker cracks your password they still cannot log in without the second factor. Use an authenticator app (Google Authenticator, Authy) rather than SMS — SMS can be intercepted via SIM swapping attacks.

### Check for Breached Credentials
Visit **haveibeenpwned.com** to check if your email address and passwords have appeared in known data breaches. If they have, change those passwords immediately everywhere you used them.

### Question Unexpected Login Requests
If you receive an MFA prompt you did not trigger, someone is actively trying to log in with your credentials. Deny the request and change your password immediately.

### Account Lockout Policies
Lock accounts after a certain number of failed attempts. This makes automated brute force attacks impractical.

---

## Relevance to This Lab

The Windows Server has RDP exposed to the internet. The Ubuntu Server has SSH exposed. Both are actively receiving brute force attempts from automated bots within minutes of deployment. The failed login attempts generate **Windows Event ID 4625** on Windows and auth.log entries on Linux — exactly the logs that will be hunted for in Kibana.

Understanding brute force attacks from the attacker's perspective makes detection on the defender's side much more effective.

---

## Key Takeaways

- Brute force attacks are automated and constant — any internet-exposed service will be targeted
- Dictionary attacks and credential stuffing are more effective than pure brute force
- Long passwords, MFA and account lockout policies are the primary defences
- The same attack traffic hitting this lab is what SOC analysts detect and respond to in real environments

---

*Day 11 of 30 complete.*
