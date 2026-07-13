<div align="center">

<img width="700" alt="BRUTEFORCE" src="https://github.com/user-attachments/assets/46cb8c61-c69d-4b0d-bd7e-056b1a5e7d0c" />

### Zeek Rule-Based Brute-Force Detection

*Detects SSH and HTTP form-based brute-force login attempts, successful logins after guessing, and first-contact successes from untrusted sources.*

<br>

![Zeek](https://img.shields.io/badge/ZEEK-6.0.9-00F5D4?style=for-the-badge&labelColor=0D1117)
![Type](https://img.shields.io/badge/TYPE-THRESHOLD%20%2B%20HEURISTIC-FF3864?style=for-the-badge&labelColor=0D1117)
![Log](https://img.shields.io/badge/LOG-brute.log-FFD60A?style=for-the-badge&labelColor=0D1117)
![Status](https://img.shields.io/badge/STATUS-WORKING-3FB950?style=for-the-badge&labelColor=0D1117)

</div>

<br>

## ⚡ What is this?

`bruteforce-detect.zeek` monitors both **SSH** and **HTTP form-based** authentication and tracks failed attempts per source IP using manual table-based state (no `SumStats`, for Zeek 6.0.9 compatibility). It logs every single failed attempt with a running count, flags a source once it crosses a threshold, and layers on two extra heuristics: catching a flagged attacker who later succeeds, and catching a successful login from a source with *no prior history at all*.

HTTP failures are detected two ways: an HTTP response code of `401`/`403`, or a configurable failure string matched inside the HTTP response body (for form-based logins that return `200` with an error message in the page).

<br>

## 🎯 Detection Rules

<div align="center">

| # | Rule | Trigger | Severity |
|:---:|:---|:---|:---:|
| 1 | Brute-Force Threshold | 12+ failed auth attempts from the same source IP within a 3-minute window (SSH or HTTP) | 🔴 **HIGH** |
| 2 | Successful Login After Guessing | Source previously flagged for brute-forcing SSH later succeeds a login | 🔴 **CRITICAL** |
| 3 | First-Contact Success (External) | A source **outside the trusted networks** succeeds an SSH login on its very first-ever observed contact, with zero prior attempt history | 🟠 **HIGH** |

</div>

*Threshold (`attempts_threshold`), window (`brute_window`), trusted networks (`trusted_networks`), and the HTTP failure-body string are all configurable at the top of the script via `&redef`.*

<br>

## 📊 Log Output

All events are written to `brute.log` (JSON). Every failed attempt is logged individually (with a running `total_attempts`); threshold-crossing, window-summary, and heuristic events add a `message`.

<div align="center">

| Field | Description |
|:---|:---|
| `ts` | Timestamp of the event |
| `uid` | Zeek connection UID |
| `id` | Full connection 4-tuple (`id.orig_h`, `id.orig_p`, `id.resp_h`, `id.resp_p`) |
| `service` | Event context — see table below |
| `total_attempts` | Running failed-attempt count for this source *(optional)* |
| `message` | Human-readable note, present on alert-level events *(optional)* |

</div>

**`service` values:**

| Value | Meaning |
|:---|:---|
| `ssh` | Individual SSH failed-attempt log |
| `ssh-window-summary` | End-of-window summary, logged if total attempts exceeded the threshold |
| `ssh-success-after-guessing` | Source succeeded SSH login after being flagged |
| `ssh-first-contact-success` | External source succeeded SSH login with no prior history |
| `http` | Individual HTTP failed-attempt log |
| `http-window-summary` | End-of-window summary, logged if total attempts exceeded the threshold |

<br>

## 🔔 Notice Types

| Notice | Fires When |
|:---|:---|
| `Brute_Force_Detected` | Attempt count crosses `attempts_threshold` (SSH or HTTP) |
| `Successful_Login_After_Guessing` | Previously flagged SSH source logs in successfully |
| `First_Contact_Success` | External source with no history succeeds SSH login on first contact |

<br>

## ⚙️ Setup

```zeek
@load ./bruteforce-detect.zeek
```

```bash
zeekctl deploy
```

<br>

## 📁 Structure

```
bruteforce-detector/
├── README.md
└── bruteforce-detect.zeek
```

<br>

## 🖥️ Environment Tested

- Zeek 6.0.9 on Debian
- Tested against a local HTTP server serving form-based auth, plus SSH
- JSON log output enabled (`LogAscii::use_json = T`)

<br>

## 📝 Notes

> `trusted_networks` defaults to `172.16.1.0/24` and `192.168.100.0/24` — update these to match your own lab/network before deploying.
>
> The HTTP form-failure string is configurable (`dvwa_fail_string`) — point it at whatever failure message your target application returns.
>
> Feeds into Wazuh via a custom decoder/rule for centralized alerting.

<br>

<div align="center">

```
▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓
```

</div>
