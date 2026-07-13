<div align="center">

<img width="700"  alt="banner" src="https://github.com/user-attachments/assets/4531c7aa-7980-4621-b20f-d8e34c3e57c3" />

### Custom Zeek Network Security Monitoring Scripts

*A lightweight, rule-based Zeek detection suite — no third-party packages, no external dependencies, pure Zeek scripting.*

<br>

![Zeek](https://img.shields.io/badge/ZEEK-6.0.9-00F5D4?style=for-the-badge&labelColor=0D1117)
![NSM](https://img.shields.io/badge/NSM-NETWORK%20MONITORING-FF3864?style=for-the-badge&labelColor=0D1117)
![Logs](https://img.shields.io/badge/LOGS-JSON-FFD60A?style=for-the-badge&labelColor=0D1117)
![SIEM](https://img.shields.io/badge/SIEM-WAZUH%20READY-3FB950?style=for-the-badge&labelColor=0D1117)
![Platform](https://img.shields.io/badge/PLATFORM-DEBIAN-58A6FF?style=for-the-badge&labelColor=0D1117)

</div>

<br>

---

## ⚡ What is this?

This repository is a set of **custom Zeek detection scripts** built as part of my **NativeDefence** cybersecurity home lab. Each script hooks into Zeek's event engine to identify a specific attack pattern on the wire and writes structured JSON alerts, ready to be ingested by a SIEM (Wazuh) for centralized alerting and correlation.

Every detector is self-contained in its own folder with its own script and documentation.

<br>

---

## 🎯 Detectors

<div>

| # | Detector | Detects | Log Output | Docs |
|:---:|:---|:---|:---|:---:|
| 1 | 🔓 **Brute-Force** | Repeated failed auth attempts from a single source (SSH/HTTP) | `bruteforce.log` | [→ README](bruteforce-detector/README.md) |
| 2 | 💉 **SQL Injection** | SQLi payloads in HTTP requests (URI/params) | `sqli.log` | [→ README](sqli-detector/README.md) |
| 3 | 🕸️ **XSS** | Cross-site scripting payloads in HTTP requests | `xss.log` | [→ README](xss-detector/README.md) |

</div>

<br>

---

## 🖥️ Environment

<div>

| Component | Detail |
|:---|:---|
| 🧠 Zeek version | `6.0.9` |
| 💻 OS | Debian (`ndos`) |
| 📄 Log format | JSON (`LogAscii::use_json = T`) |
| 🎯 Target lab | Local HTTP server, Windows workstation |
| 🌐 Traffic source | Sophos SFOS firewall span/mirror |

</div>

<br>

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

<br>

---

## ⚙️ Setup

**1.** Copy the desired detector folder's `.zeek` file into your Zeek scripts directory.

**2.** Load it from `local.zeek`:
```zeek
@load ./bruteforce-detect.zeek
@load ./sqli-alert.zeek
@load ./xss-detect.zeek
```

**3.** Deploy:
```bash
zeekctl deploy
```

**4.** Alerts land in `/var/log/zeek/current/` as JSON, ready for Wazuh ingestion via custom decoders/rules.

<br>

---

## 📝 Notes

> Built and tested on **Zeek 6.0.9**, where `SumStats` has compatibility issues — detectors here use manual table-based tracking instead.
>
> Each detector is independent — use one, two, or all three.

<br>

---

## 🧩 Part of NativeDefence

This repo is one component of a larger structured home lab **(Zeek + Wazuh SIEM + Sophos firewall)** built as a cybersecurity portfolio project.

<br>

---

## 📜 License

This project is licensed under the [MIT License](https://github.com/PriyanshuKhambalkar/zeek-detection-rules/blob/main/LICENSE) - see the LICENSE file for details.<br/>
For commercial use or redistribution, please get in touch with the author.

---

## 👤 Author

**Priyanshu Khambalkar**

---


<div align="center">

*Built with Python · No ML · No External Rule Files · Cross-Platform*

⭐ **Star this repo if you found it useful**

</div>
