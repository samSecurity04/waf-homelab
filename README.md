# 🛡️ Web Application Security Lab — SafeLine WAF + DVWA on Apple Silicon
End-to-end home lab demonstrating offensive web attacks and WAF-based defense, built on a MacBook Pro M4 (ARM64).

![Platform](https://img.shields.io/badge/Platform-Apple%20Silicon-blue)
![WAF](https://img.shields.io/badge/WAF-SafeLine%20Pro-red)
![Status](https://img.shields.io/badge/Status-Complete-success)

---

## 🎯 What I Built

A complete attack-and-defense lab where I:
1. Deployed DVWA (vulnerable web app) on Ubuntu Server
2. Successfully exploited it from Kali Linux (SQL injection, XSS, command injection)
3. Deployed SafeLine WAF as a reverse proxy
4. Verified the same attacks now get blocked
5. Configured rate limiting and tested HTTP flood defense

## 🏗️ Architecture

Kali Linux (Attacker) → https://dvwa.local:443 → SafeLine WAF → DVWA on port 8080
                                                 (filters traffic)

## 🛠️ Tech Stack

- **Host:** MacBook Pro M4 + VMware Fusion
- **VMs:** Kali Linux ARM64 + Ubuntu Server 22.04 ARM64
- **Stack:** Apache 2 + PHP 8.5 + MySQL 8.4 + DVWA
- **WAF:** SafeLine WAF v9.3.7 (Pro Edition)

---
## 📂 Project Phases

1. Environment & VM Setup
2. LAMP Stack + DVWA Deployment
3. Attack Simulation Before WAF
4. Troubleshooting & Debugging
5. SafeLine WAF Deployment
6. WAF Protection Validation
7. Advanced Defense Configuration
8. Attack Logs & Monitoring
Each phase is documented in the `Lab Screenshots/` directory with supporting screenshots and attack validation results.
---

## 📸 Highlights

### Attack → Blocked

| Before WAF | After WAF |
|------------|-----------|
| ![SQL Success](Lab%20Screenshots/03-attacks-pre-waf/01-sql-injection-success.webp) | ![SQL Blocked](Lab%20Screenshots/06-waf-in-action/01-sql-injection-blocked.webp) |
| SQL injection dumps full user database | Same payload blocked at WAF layer |

### WAF Dashboard & Attack Logs

![Dashboard](Lab%20Screenshots/05-waf-deployment/03-waf-dashboard.webp)
![Attack Logs](Lab%20Screenshots/08-detailed-logs/01-attack-logs.webp)

### HTTP Flood Defense

500 requests via `ab` — all blocked by rate limiting.

![HTTP Flood](Lab%20Screenshots/07-advanced-defense/02-load-test-success.webp)

---

## 🔧 Notable Challenges Solved

- **MySQL 8.4 reserved keyword bug** — `role` column broke DVWA setup. Patched the source to escape with backticks. Full writeup: [LESSONS-LEARNED.md](LESSONS-LEARNED.md)
- **ARM64 compatibility** — Standard x86_64 Docker images failed. Switched the entire stack to native ARM64.
- **WAF bypass discovery** — Direct backend hits bypassed the WAF until traffic was correctly routed through port 443.

---

## 🎓 Skills Demonstrated

`Linux` `Apache` `MySQL` `WAF Deployment` `Reverse Proxy` `SQL Injection` `XSS` `Command Injection` `OWASP Top 10` `SSL/TLS` `Rate Limiting` `Log Analysis` `Troubleshooting`

---

## 📂 Repository Contents

- **`README.md`** — This file
- **`LESSONS-LEARNED.md`** — Detailed troubleshooting writeups
- **`Lab Screenshots/`** — Phase-by-phase visual documentation

---

## 👤 Author

**Sam Patil** | [GitHub](https://github.com/samSecurity04) | [LinkedIn](https://www.linkedin.com/in/samruddhi-p-patil/)

🎓 Security+ | Microsoft SC-900 | EC-Council CASE | Google Cybersecurity | *In progress: SC-200*

---

## 📌 Note

This project was built for educational and cybersecurity lab purposes only.
