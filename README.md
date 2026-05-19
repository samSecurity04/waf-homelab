# 🛡️ Web Application Security Lab — SafeLine WAF + DVWA on Apple Silicon

End-to-end home lab demonstrating offensive web attacks and WAF-based defense, built on a MacBook Pro M4 (ARM64).

Tested on Apple Silicon (ARM64) using VMware Fusion and native ARM containers.

![Platform](https://img.shields.io/badge/Platform-Apple%20Silicon-blue)
![WAF](https://img.shields.io/badge/WAF-SafeLine%20Pro-red)
![Status](https://img.shields.io/badge/Status-Complete-success)

---

## 🎯 What I Built

This project demonstrates offensive web security testing and defensive traffic filtering using SafeLine WAF in a fully virtualized ARM64 homelab environment.

A complete attack-and-defense lab where I:

1. Deployed DVWA (vulnerable web app) on Ubuntu Server
2. Successfully exploited it from Kali Linux (SQL injection, XSS, command injection)
3. Deployed SafeLine WAF as a reverse proxy
4. Verified the same attacks now get blocked
5. Configured rate limiting and tested HTTP flood defense

---

## 🏗️ Architecture

```text
Kali Linux (Attacker)
        ↓
https://dvwa.local:443
        ↓
SafeLine WAF (Reverse Proxy)
        ↓
DVWA Backend Server :8080
```

---

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

<table>
<tr>
<td align="center"><b>Before WAF</b></td>
<td align="center"><b>After WAF</b></td>
</tr>

<tr>
<td>
<img src="Lab%20Screenshots/03-attacks-pre-waf/01-sql-injection-success.webp" width="500">
</td>

<td>
<img src="Lab%20Screenshots/06-waf-in-action/01-sql-injection-blocked.webp" width="500">
</td>
</tr>

<tr>
<td align="center">SQL injection successfully dumps database records</td>
<td align="center">Same payload blocked by SafeLine WAF</td>
</tr>
</table>

---

### WAF Dashboard

<img src="Lab%20Screenshots/05-waf-deployment/03-waf-dashboard.webp" width="900">

---

### Attack Logs & Detection

<img src="Lab%20Screenshots/08-detailed-logs/01-attack-logs.webp" width="900">

---

### HTTP Flood Defense

500 requests were launched using Apache Benchmark (`ab`) to simulate a basic HTTP flood attack.

All malicious requests were successfully blocked through SafeLine rate limiting policies.

<img src="Lab%20Screenshots/07-advanced-defense/02-load-test-success.webp" width="900">

---

## 🔧 Notable Challenges Solved

- **MySQL 8.4 reserved keyword bug** — `role` column broke DVWA setup. Patched the source to escape with backticks.
- **ARM64 compatibility** — Standard x86_64 Docker images failed. Switched the entire stack to native ARM64.
- **WAF bypass discovery** — Direct backend hits bypassed the WAF until traffic was correctly routed through port 443.

---

## 🎓 Skills Demonstrated

`Linux` `Apache` `MySQL` `WAF Deployment` `Reverse Proxy` `SQL Injection` `XSS` `Command Injection` `OWASP Top 10` `SSL/TLS` `Rate Limiting` `Log Analysis` `Troubleshooting`

---

## 📂 Repository Contents

- **`README.md`** — Main project documentation
- **`LESSONS-LEARNED.md`** — Troubleshooting and debugging notes
- **`Lab Screenshots/`** — Phase-by-phase visual documentation

---

## 👤 Author

**Sam Patil**  
[GitHub](https://github.com/samSecurity04)  
[LinkedIn](https://www.linkedin.com/in/samruddhi-p-patil/)

🎓 Comptia Security+ | Microsoft SC-900 | EC-Council CASE (java) | Google Cybersecurity | *In progress: Microsoft SC-200*

---

## 📌 Note

This project was built for educational and cybersecurity lab purposes only.
