<div align="center">

# 🛡️ Web Application Security Lab - SafeLine WAF + DVWA

### *Attack first. Defend second. Prove the difference.*

![Platform](https://img.shields.io/badge/Platform-Apple_Silicon_ARM64-000000?style=for-the-badge&logo=apple&logoColor=white)
![SafeLine](https://img.shields.io/badge/WAF-SafeLine_v9.3.7-red?style=for-the-badge)
![DVWA](https://img.shields.io/badge/Target-DVWA-orange?style=for-the-badge)
![VMware](https://img.shields.io/badge/VMware_Fusion-ARM64-607078?style=for-the-badge&logo=vmware&logoColor=white)
![Status](https://img.shields.io/badge/Status-Complete-2ea44f?style=for-the-badge)

<br/>

> End-to-end home lab demonstrating offensive web attacks and WAF-based defense, built on a MacBook Pro M4 (ARM64).
>
> **The same attack that dumps a database in phase 3 gets blocked in phase 6. That contrast is the whole point.**

<br/>

---

</div>

## 🎯 What I Built

This project demonstrates offensive web security testing and defensive traffic filtering using SafeLine WAF in a fully virtualized ARM64 homelab.

A complete attack and defense lab where I:

1. Deployed DVWA (vulnerable web app) on Ubuntu Server
2. Successfully exploited it from Kali Linux (SQL injection, XSS, command injection)
3. Deployed SafeLine WAF as a reverse proxy
4. Verified the same attacks now get blocked
5. Configured rate limiting and tested HTTP flood defense

---

## 🏗️ Architecture

<div align="center">

```text
Kali Linux (Attacker)
        ↓
https://dvwa.local:443
        ↓
SafeLine WAF (Reverse Proxy)
        ↓
DVWA Backend Server :8080
```

</div>

---

## 🛠️ Tech Stack

| Component | Detail |
|---|---|
| Host | MacBook Pro M4 + VMware Fusion |
| VMs | Kali Linux ARM64 + Ubuntu Server 22.04 ARM64 |
| Stack | Apache 2 + PHP 8.5 + MySQL 8.4 + DVWA |
| WAF | SafeLine WAF v9.3.7 (Pro Edition) |

---

## 📂 Project Phases

1. Environment and VM Setup
2. LAMP Stack and DVWA Deployment
3. Attack Simulation Before WAF
4. Troubleshooting and Debugging
5. SafeLine WAF Deployment
6. WAF Protection Validation
7. Advanced Defense Configuration
8. Attack Logs and Monitoring

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

<div align="center">
  <img src="Lab%20Screenshots/05-waf-deployment/03-waf-dashboard.webp" width="750">
</div>

---

### Attack Logs and Detection

<div align="center">
  <img src="Lab%20Screenshots/08-detailed-logs/01-attack-logs.webp" width="750">
</div>

---

### HTTP Flood Defense

500 requests were launched using Apache Benchmark (`ab`) to simulate a basic HTTP flood attack. All malicious requests were successfully blocked through SafeLine rate limiting policies.

<div align="center">
  <img src="Lab%20Screenshots/07-advanced-defense/02-load-test-success.webp" width="600">
</div>

---

## 🔧 Notable Challenges Solved

- **MySQL 8.4 reserved keyword bug.** The `role` column broke DVWA setup. I patched the source to escape it with backticks.
- **ARM64 compatibility.** Standard x86_64 Docker images failed. I switched the entire stack to native ARM64.
- **WAF bypass discovery.** Direct backend hits bypassed the WAF until traffic was correctly routed through port 443.

---

## 💡 What I Learned

- How a reverse-proxy WAF sits in front of an application and inspects traffic before it reaches the backend
- That a WAF only protects what actually routes through it, direct backend access defeats the whole control
- How the same attack payload behaves differently with and without filtering in place
- How rate limiting mitigates volumetric attacks like HTTP floods

A full breakdown is available in [LESSONS-LEARNED.md](LESSONS-LEARNED.md).

---

## 🎓 Skills Demonstrated

`Linux` `Apache` `MySQL` `WAF Deployment` `Reverse Proxy` `SQL Injection` `XSS` `Command Injection` `OWASP Top 10` `SSL/TLS` `Rate Limiting` `Log Analysis` `Troubleshooting`

---

## 📂 Repository Contents

- **README.md** — Main project documentation
- **LESSONS-LEARNED.md** — Troubleshooting and debugging notes
- **Lab Screenshots/** — Phase-by-phase visual documentation

---

<div align="center">

**Samruddhi (Sam) Patil** — Aspiring SOC Analyst

[![GitHub](https://img.shields.io/badge/GitHub-samSecurity04-181717?style=for-the-badge&logo=github)](https://github.com/samSecurity04)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-samruddhi--p--patil-0A66C2?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/samruddhi-p-patil/)

🎓 CompTIA Security+ &nbsp;|&nbsp; Microsoft SC-900 &nbsp;|&nbsp; EC-Council CASE (Java) &nbsp;|&nbsp; Google Cybersecurity

*This project was built for educational and cybersecurity lab purposes only.*

</div>
