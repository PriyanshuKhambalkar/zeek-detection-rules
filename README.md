<div align="center">
# 🦉 ZEEK DETECTION SUITE
 
### Custom Zeek Network Security Monitoring Scripts
 
*A collection of rule-based, lightweight Zeek detectors built for real-time attack detection — no third-party packages, no external dependencies, pure Zeek scripting.*
 
![Zeek](https://img.shields.io/badge/Zeek-6.0.9-1793D1?style=for-the-badge)
![NSM](https://img.shields.io/badge/NSM-Network%20Security%20Monitoring-critical?style=for-the-badge)
![JSON](https://img.shields.io/badge/Logs-JSON-yellow?style=for-the-badge)
![SIEM](https://img.shields.io/badge/SIEM-Wazuh%20Ready-green?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Debian-orange?style=for-the-badge)
 
</div>
---
 
## ⚡ What is this?
 
This repository is a set of **custom Zeek detection scripts** built as part of my **NativeDefence** cybersecurity home lab. Each script hooks into Zeek's event engine to identify a specific attack pattern on the wire and writes structured JSON alerts, ready to be ingested by a SIEM (Wazuh) for centralized alerting and correlation.
 
Every detector is self-contained in its own folder with its own script and documentation.
 
---
 
## 🎯 Detectors
 
| # | Detector | Detects | Log Output | Docs |
|---|---|---|---|---|
| 1 | 🔓 **Brute-Force** | Repeated failed auth attempts from a single source (SSH/HTTP) | `bruteforce.log` | [bruteforce-detector/README.md](bruteforce-detector/README.md) |
| 2 | 💉 **SQL Injection** | SQLi payloads in HTTP requests (URI/params) | `sqli.log` | [sqli-detector/README.md](sqli-detector/README.md) |
| 3 | 🕸️ **XSS** | Cross-site scripting payloads in HTTP requests | `xss.log` | [xss-detector/README.md](xss-detector/README.md) |
 
---
 
## 🖥️ Environment
 
| Component | Detail |
|---|---|
| Zeek version | `6.0.9` |
| OS | Debian (`ndos`) |
| Log format | JSON (`LogAscii::use_json = T`) |
| Target lab | DVWA VM, Windows workstation |
| Traffic source | Sophos SFOS firewall span/mirror |
 
---
 
## 📁 Project Structure
 
```
zeek-detection-suite/
├── README.md
├── .gitignore
├── bruteforce-detector/
│   ├── README.md
│   └── bruteforce-detect.zeek
├── sqli-detector/
│   ├── README.md
│   └── sqli-alert.zeek
└── xss-detector/
    ├── README.md
    └── xss-detect.zeek
```
 
---
 
## ⚙️ Setup
 
1. Copy the desired detector folder's `.zeek` file into your Zeek scripts directory.
2. Load it from `local.zeek`:
```zeek
   @load ./bruteforce-detect.zeek
   @load ./sqli-alert.zeek
   @load ./xss-detect.zeek
```
3. Deploy:
```bash
   zeekctl deploy
```
4. Alerts land in `/var/log/zeek/current/` as JSON, ready for Wazuh ingestion via custom decoders/rules.
---
 
## 📝 Notes
 
- Built and tested on **Zeek 6.0.9**, where `SumStats` has compatibility issues — detectors here use manual table-based tracking instead.
- Each detector is independent — use one, two, or all three.
---
 
## 🧩 Part of NativeDefence
 
This repo is one component of a larger structured home lab (Zeek + Wazuh SIEM + Sophos firewall) built as a cybersecurity portfolio project.
 
---
 
<div align="center">
**License:** MIT
 
</div>
