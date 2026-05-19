# 🔧 Lessons Learned — Real-World Troubleshooting Documentation

This document captures the unexpected challenges encountered during this lab and how each was resolved. These were not in any tutorial — they were solved through log analysis, documentation reading, and hands-on debugging.

---

## 🐛 Issue #1 — MySQL 8.4 Reserved Keyword Breaking DVWA Setup

### Symptom
After installing DVWA and clicking the **"Create / Reset Database"** button on `setup.php`, the page returned an **HTTP 500 Internal Server Error**.

### Investigation
Reading the Apache error log was the key first step:

```bash
sudo tail -30 /var/log/apache2/error.log
```

The log revealed a MySQL SQL syntax error involving the column name `role`:

```
You have an error in your SQL syntax... near 'role VARCHAR(20) DEFAULT 'user''
```

### Root Cause
**MySQL 8.4 added `role` as a reserved keyword** for role-based access control features. The DVWA setup script (`/var/www/html/DVWA/dvwa/includes/DBMS/MySQL.php`) used `role` as a regular column name without escaping it.

This bug doesn't appear in older MySQL versions (5.7, 8.0), which is why most online tutorials don't mention it.

### Resolution
1. Located the offending line in `MySQL.php`:
   ```sql
   ALTER TABLE users ADD COLUMN IF NOT EXISTS role VARCHAR(20) DEFAULT 'user';
   ```
2. Wrapped `role` in backticks to escape the reserved word:
   ```sql
   ALTER TABLE users ADD COLUMN IF NOT EXISTS `role` VARCHAR(20) DEFAULT 'user';
   ```
3. Found and fixed a second occurrence further down the file (a similar `UPDATE` statement).
4. Manually executed the corrected database initialization SQL via the MySQL CLI to bypass the broken setup script entirely.

### Lesson
- **Read the logs first.** The error message points directly to the problem in 90% of cases.
- **Compatibility matters.** Software written 5+ years ago may break on modern dependency versions. Always check the versions involved when something fails inexplicably.

---

## 🐛 Issue #2 — Apple Silicon (ARM64) Compatibility

### Symptom
Initial attempts to run Docker-based DVWA images (`vulnerables/web-dvwa`) failed with:

```
The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8)
```

### Root Cause
Most Docker images on Docker Hub are built for **x86_64 (amd64)** architecture. On Apple Silicon (M-series chips), these images either fail to run or run extremely slowly through QEMU emulation.

### Resolution
- Switched the entire lab to **native ARM64 components**:
  - Kali Linux **ARM64** ISO
  - Ubuntu Server **ARM64** ISO
  - SafeLine WAF (their installer auto-pulled `safeline-chaos-g-arm:9.3.7` — confirming ARM64 support)
- Verified architecture on the running VM:
  ```bash
  uname -m
  # Returns: aarch64  ✅
  ```

### Lesson
- **Apple Silicon viability for security labs is excellent** — but every component must be ARM64-native.
- **Check architecture support before committing** to any tool when working on M-series Macs.

---

## 🐛 Issue #3 — VirtualBox vs VMware Fusion on Apple Silicon

### Initial Attempt
The original tutorial assumed VirtualBox. On Apple Silicon, **VirtualBox is unreliable** — Oracle has been slow to release stable ARM64 support.

### Resolution
Switched to **VMware Fusion Pro**, which is:
- ✅ Free for personal use
- ✅ Has full native Apple Silicon support
- ✅ Handles ARM64 guests smoothly
- ✅ Uses `open-vm-tools` for clipboard/shared folders (similar to VirtualBox Guest Additions)

### Lesson
- **The tool ecosystem on Apple Silicon is different.** Don't assume Intel-Mac tutorials work as-is. Verify each tool's ARM64 support before committing.

---

## 🐛 Issue #4 — Bypassing the WAF by Accident

### Symptom
After deploying SafeLine WAF, the SQL injection attack **still succeeded** when tested.

### Root Cause
The Kali browser was hitting the **direct backend URL** (`http://192.168.50.200:8080/DVWA`), which bypassed the WAF entirely. The WAF only protects traffic that flows through its listening port (443).

### Resolution
- Confirmed the traffic flow:
  - ❌ `http://192.168.50.200:8080/DVWA` → bypasses WAF
  - ✅ `https://dvwa.local/DVWA` → routes through WAF on port 443
- Updated `/etc/hosts` on Kali to resolve `dvwa.local` to the Ubuntu server's IP.
- Accepted the self-signed SSL warning.
- Re-ran the SQL injection — **blocked with "Access Forbidden"** as expected.

### Lesson
- **A WAF is only effective when traffic actually flows through it.** Network architecture matters as much as the WAF rules themselves.
- This is exactly how real-world WAF bypasses work — attackers find the **origin server IP** and skip the WAF entirely. Hiding backend infrastructure (e.g., Cloudflare-style obfuscation) is part of proper WAF deployment.

---

## 🐛 Issue #5 — Clipboard Sharing Between macOS Host & Ubuntu VM

### Symptom
Could not copy/paste commands from macOS host into the Ubuntu VM terminal.

### Resolution
Two options worked:

**Option A — Install `open-vm-tools`:**
```bash
sudo apt install open-vm-tools open-vm-tools-desktop -y
sudo reboot
```

**Option B — Use SSH from the Mac terminal:**
```bash
ssh samruddhi@192.168.50.200
```
This gave native clipboard support (Cmd+V worked normally) and was faster than fighting the VM's terminal.

### Lesson
- **SSH from the host terminal is often faster** than working inside a VM window, especially for command-heavy work.

---

## 🎯 Summary

| Issue | Root Cause | Fix |
|-------|-----------|------|
| 500 Error on DVWA Setup | MySQL 8.4 reserved keyword | Escape `role` with backticks |
| Docker image won't run | x86_64 image on ARM64 host | Switch to ARM64-native images |
| VirtualBox unreliable | Limited Apple Silicon support | Use VMware Fusion Pro |
| SQL injection still works | Hitting backend port directly | Route through WAF domain on port 443 |
| Can't copy/paste in VM | Missing `open-vm-tools` | Install tools or SSH from host |

---

## 💡 What This Demonstrates

These were not pre-scripted tutorial problems — they were real obstacles that required:
- **Log analysis** (Apache error logs revealing the MySQL bug)
- **Documentation reading** (MySQL 8.4 release notes, SafeLine docs)
- **Architectural understanding** (why traffic must flow through the WAF)
- **Patience and iteration** (multiple wrong fixes before identifying the actual root cause)

These are exactly the skills employed daily by SOC analysts, incident responders, and security engineers.
