<div align="center">

<img width="700" alt="SQL INJECT" src="https://github.com/user-attachments/assets/189baf42-5d86-4f8f-b74f-e67242901bda" />

### Zeek Rule-Based SQLi Detection

*Inspects HTTP request URIs for SQL injection payloads using a broad, case-insensitive signature set.*

<br>

![Zeek](https://img.shields.io/badge/ZEEK-6.0.9-00F5D4?style=for-the-badge&labelColor=0D1117)
![Type](https://img.shields.io/badge/TYPE-PATTERN%20MATCHING-FF3864?style=for-the-badge&labelColor=0D1117)
![Log](https://img.shields.io/badge/LOG-sqli--alert.log-FFD60A?style=for-the-badge&labelColor=0D1117)
![Status](https://img.shields.io/badge/STATUS-WORKING-3FB950?style=for-the-badge&labelColor=0D1117)

</div>

---

<br>

## ⚡ What is this?

`sqli-detect.zeek` hooks into Zeek's `http_request` event and checks the unescaped request URI against a single combined `pattern` covering common SQL injection techniques — from basic tautologies to blind/time-based and out-of-band methods.

<br>

---

## 🎯 Detection Rule

<div>

| # | Rule | Trigger | Severity |
|:---:|:---|:---|:---:|
| 1 | SQL Injection Payload | HTTP request URI matches any SQLi signature (see below) | 🔴 **CRITICAL** |

</div>

**Signatures covered** (all case-insensitive):

| Category | Patterns |
|:---|:---|
| Tautologies | `UNION SELECT`, `OR 1=1`, `AND 1=1`, `' OR '...'=` |
| Comment / termination | `-- `, `; DROP TABLE` |
| Blind / time-based | `SLEEP(`, `BENCHMARK(` |
| Command execution | `EXEC(`, `xp_cmdshell` |
| Enumeration | `INFORMATION_SCHEMA` |
| Type coercion | `CAST(`, `CONVERT(` |

<br>

---

## 📊 Log Output

Alerts are written to **`sqli-alert.log`** (JSON). Fields:

<div>

| Field | Description |
|:---|:---|
| `ts` | Timestamp of the request |
| `uid` | Zeek connection UID |
| `id` | Full connection 4-tuple (`id.orig_h`, `id.orig_p`, `id.resp_h`, `id.resp_p`) |
| `method` | HTTP method (GET, POST, etc.) |
| `payload` | The unescaped URI containing the matched SQLi pattern |

</div>

<br>

---

## 🔔 Notice Type

| Notice | Fires When |
|:---|:---|
| `SQLi_Attempt` | Request URI matches any signature in `sqli_patterns` |

<br>

---

## ⚙️ Setup

```zeek
@load ./sqli-detect.zeek
```

```bash
zeekctl deploy
```

<br>

---

## 📁 Structure

```
sqli-detector/
├── README.md
└── sqli-detect.zeek
```

<br>

---

## 🖥️ Environment Tested

- Zeek 6.0.9 on Debian
- Tested against a local HTTP server serving crafted SQLi request payloads
- JSON log output enabled (`LogAscii::use_json = T`)

<br>

---

## 📝 Notes

> Detection is URI-based only (GET/query-string parameters) — POST body payloads are not currently inspected.
>
> Signature set is defined as a single `pattern` constant (`sqli_patterns`) and is extendable via `&redef`.
>
> Feeds into Wazuh via a custom decoder/rule for centralized alerting.

<br>

<div align="center">

---

*Built with Zeek · 13 Signature Categories · URI-Based Detection*

</div>
