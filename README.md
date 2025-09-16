
# 📝 Honeypot Detection Checklist

This checklist helps you quickly identify if a Shodan IP or server is likely a honeypot.  
It covers accounts, services, banners, OS fingerprints, network behavior, and external references.

---

## 1️⃣ /etc/passwd / Accounts Analysis

**Look for too many service accounts:**  

Example: MongoDB, Redis, Tomcat, Magento, Drupal, Elasticsearch, Mosquitto, Shopify, Openshift, Varnish, Gopher  

> Real servers rarely have all these at once.

**Suspicious shells:**  
- `/bin/logbash`  
- `/bin/true`  
- `/bin/false`  

**High-value application accounts:**  
- Shopify, Magento, Drupal, Jupyter, etc.

**Practical tip:** Compare the number of accounts with the expected services on a real server.

---

## 2️⃣ Banner & Service Fingerprinting

- Use `nmap -sV <IP>` or Shodan metadata to view service banners.
- Check for generic or inconsistent versions:  
  Example: SSH banner says Debian, but OS fingerprint is Windows → suspicious.
- Look for honeypot-specific banners:  
  Cowrie, Dionaea, Honeyd, OpenCanary, T-Pot  

Example:

```
SSH-2.0-Cowrie-2.9.0
```

> This clearly indicates a honeypot.

---

## 3️⃣ Ports & Services

- **Unusual port usage:** SSH on 2222, HTTP on 8088 or 8000  
- **All-in-one servers:** Too many services running simultaneously → unlikely in real-world setups.

**Tip:** Compare with standard server setups; a production server usually has only the necessary services.

---

## 4️⃣ Response Behavior

- **Timing analysis:** Instant, highly consistent responses may indicate automation or emulation.  
- **Command execution:** Honeypots simulate shell commands; output may be canned or unrealistic.  
- **Rate-limiting absence:** Real servers usually block repeated login attempts; honeypots may allow unlimited tries.

**Example test:**

```bash
ssh user@ip
# Observe response to invalid password attempts
```

---

## 5️⃣ OS Fingerprinting

- Use `nmap -O <IP>` or Shodan’s OS information.  
- Compare OS fingerprint with service banners.  

**Mismatch example:**  
- Banner: OpenSSH 7.6p1 Debian  
- OS fingerprint: FreeBSD → suspicious.

---

## 6️⃣ Network Behavior Analysis

- **Check ICMP/ping responses:** Highly consistent RTT across multiple probes → may be emulated.  
- **Check TCP/IP stack quirks:** Tools like `hping3` can identify anomalies in SYN/ACK sequences.  
- **Look for extra monitoring traffic:** Honeypots may generate additional logging or alert traffic.

---

## 7️⃣ Known Honeypot Databases

Cross-check IP with:  
- HoneyDB  
- AbuseIPDB  

> If listed → high likelihood of honeypot.

---

## 8️⃣ Metadata & Shodan Clues

- High vulnerability counts across unrelated ports → suspicious.  
- Multiple scan entries in Shodan for unrelated services → honeypot candidate.  

**Example:**  
IP shows HTTP, SSH, FTP, Telnet, and Redis all in one scan → suspicious.

---

## 9️⃣ Heuristics & Risk Scoring

Combine all indicators for probabilistic assessment:

| Risk Level | Indicators |
|------------|------------|
| **High-risk** | Suspicious shells, known honeypot banners, too many services → Likely Honeypot |
| **Medium-risk** | Minor inconsistencies → Possibly Honeypot |
| **Low-risk** | Normal banner and port configuration → Possibly Safe |

**Tip:** Document your findings for each IP to improve accuracy over time.

---

## 🔟 Safety Notes

- Only perform passive reconnaissance unless you have explicit permission.  
- Honeypot detection is not 100% reliable; some sophisticated honeypots are indistinguishable from real servers.  
- Avoid exploits or intrusive scans on suspicious IPs.

---

## ✅ Extra Tips for Practitioners

- Keep a checklist handy to quickly flag potential honeypots.  
- Maintain a database of known honeypots and common banners.  
- When unsure, treat the server as a honeypot to avoid alerting its monitoring system.  
- Use sandboxed testing environments to verify your detection techniques.
