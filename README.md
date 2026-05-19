# 🛡️ Web Application Security Lab — SafeLine WAF on Apple Silicon

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

## 📸 Highlights

### Attack → Blocked
| Before WAF | After WAF |
|------------|-----------|
| ![SQL Success](screenshots/03-attacks-pre-waf/sql-injection-success.png) | ![SQL Blocked](screenshots/06-waf-in-action/sql-injection-blocked.png) |
| SQL injection dumps full user database | Same payload blocked at WAF layer |

### WAF Dashboard & Attack Logs
![Dashboard](screenshots/05-waf-deployment/safeline-dashboard.png)
![Attack Logs](screenshots/06-waf-in-action/attack-logs.png)

### HTTP Flood Defense
500 requests via `ab` — all blocked by rate limiting.

![HTTP Flood](screenshots/07-advanced-defense/http-flood-blocked.png)

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
- **`screenshots/`** — Phase-by-phase visual documentation (28 screenshots)

---

## 👤 Author

**Sam Patil** | [GitHub](https://github.com/samSecurity04) | [LinkedIn](https://www.linkedin.com/in/samruddhi-p-patil/)

🎓 Security+ | Microsoft SC-900 | EC-Council CASE | Google Cybersecurity | *In progress: SC-200*

---

⭐ *Star this repo if you found it useful!*
