# 📝 Honeypot Detection Checklist

This checklist helps you **quickly identify if a Shodan IP or server is likely a honeypot**.

---

## 1️⃣ /etc/passwd / Accounts Analysis
- Too many service accounts (MongoDB, Redis, Tomcat, Magento, Drupal, etc.) → suspicious
- Fake or unusual shells: `/bin/logbash`, `/bin/true`, `/bin/false`
- Accounts for high-value apps that are unlikely together → suspect

---

## 2️⃣ Banner & Service Fingerprinting
- Check banners with `nmap -sV <IP>` or Shodan metadata
- Generic or inconsistent versions (e.g., SSH banner says Debian but OS fingerprint differs)
- Unknown or default honeypot banners (Cowrie, Honeyd, Dionaea, OpenCanary)

---

## 3️⃣ Ports & Services
- Services on unusual ports
- All major services open simultaneously → unlikely in real server

---

## 4️⃣ Response Behavior
- Instant, consistent responses to malformed requests → may indicate automation
- No rate limiting for repeated login attempts → common in honeypots
- Safe simulation of command execution (shell commands don’t behave normally)

---

## 5️⃣ OS Fingerprinting
- Use `nmap -O <IP>` or Shodan OS check
- Mismatch between reported OS and service banner → suspicious

---

## 6️⃣ Known Honeypot Databases
- Check IP in:
  - [HoneyDB](https://honeydb.io)  
  - [AbuseIPDB](https://www.abuseipdb.com)
- IP listed as honeypot → confirmed

---

## 7️⃣ Metadata & Shodan Clues
- Multiple services with high vulnerability counts → suspicious
- IP seen in multiple unrelated scans → honeypot candidate

---

## ⚠️ Safety Notes
- Only passive analysis is recommended
- Honeypot detection is **probabilistic**, not 100% certain
