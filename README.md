# NETWORKWALKS-SOLOMON-B083-WK2-RECONNAISSANCE-NEWORK-SCANNING-PHASE

<p align="center">
  <img src="https://img.shields.io/badge/Skill-Cybersecurity-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Skill-Footprinting-0070C0?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Skill-Maltego-238F89?style=flat-square&labelcolor=000000" />
  <img src="https://img.shields.io/badge/Kali%20Linux-v2026.2-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Network%20Scanning-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Network-10.36.251.0%2F24-238F89?style=flat-square&labelColor=000000" />
  <img src="https://img.shields.io/badge/Penetration%20Testing-C00000?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Skill-Reconnaissance-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/GitHub-404040?style=flat-square&labelColor=0070C0&logo=github&logoColor=white" />
  <img src="https://img.shields.io/badge/Kali%20Tools-404040?style=flat-square&labelColor=C00000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/NetworkWalks-404040?style=flat-square&labelColor=C00000" />
  <img src="https://img.shields.io/badge/Zenmap-404040?style=flat-square&labelcolor=C00000" />
  <img src="https://img.shields.io/badge/Ethical%20Hacking-E87500?style=flat-square&labelColor=000000&logo=kalilinux&logoColor=white" />
  <img src="https://img.shields.io/badge/Solomon%20Diri%20INTERN-C00000?style=flat-square" />
</p>

# Footprinting & Network Scanning — Cybersecurity Internship (Week 2)

> Reconnaissance and network discovery lab performed during the Cybersecurity & Ethical Hacking Internship at **Networkwalks** (Batch B083).

**Modules covered:** W2-PM1 (Kali footprinting tools) · W2-PM3 (Maltego) · W2-PM5 (Zenmap scanning)
**Target(s):** `networkwalks.com` (written authorization obtained) · Local LAN
**Status:** Phase 1–2 complete · Phase 3–5 in progress

---

## ⚠️ Liability Disclaimer

All activities in this repository were carried out only against systems I was explicitly authorized to test, or systems I personally own. This content is for **educational purposes only**. Unauthorized access to a system is a crime in most jurisdictions, regardless of intent or actual harm caused — misuse of this material is the sole responsibility of whoever performs it, not the author or Networkwalks.

---

## 📖 Overview

This lab illustrates how an attacker moves from **passive reconnaissance** to **active network mapping**:

1. **Footprinting** the target domain using six Kali Linux tools plus Maltego, to gather public-facing intelligence without touching the target directly.
2. **Scanning** a local network with Zenmap (Nmap GUI) to enumerate live hosts, IP/MAC addresses, and produce a topology map.

Every step below documents the exact command run, what it returned, and why that finding matters from an attacker's perspective.

---

## 🛠️ Tools Used

| Tool | Purpose |
|---|---|
| Kali Linux / Windows | OS platforms for recon and scanning |
| `whois` | Domain registration details (owner, dates, name servers) |
| `whatweb` | Fingerprint web technologies (CMS, server, plugins) |
| `nslookup` | Resolve domain name → IP address |
| `curl -I` | Inspect HTTP response headers |
| `wafw00f` | Detect presence of a Web Application Firewall |
| `dnsrecon` | Enumerate DNS records (NS, MX, SPF, TXT, SRV) |
| Maltego | OSINT — retrieve corporate email addresses |
| Zenmap (Nmap GUI) | Local subnet host/MAC discovery + topology mapping |
| Windows CMD (`ipconfig`) | Identify local IP/MAC address |

---

## 🔍 Methodology

### 1. Footprinting & Reconnaissance — `networkwalks.com`

```bash
whois networkwalks.com
whatweb networkwalks.com
nslookup networkwalks.com
curl -I https://networkwalks.com
wafw00f https://networkwalks.com
dnsrecon -d networkwalks.com
```

| Step | Finding |
|---|---|
| `whois` | Domain registration data and name servers |
| `whatweb` | WordPress 7.0.4, WP Download Manager 3.3.58 |
| `nslookup` | Resolved IP: `192.232.216.135` |
| `curl -I` | Exposed WordPress REST API endpoint `/wp-json/` |
| `wafw00f` | WAF detected — ModSecurity (SpiderLabs) |
| `dnsrecon` | NS, MX, SPF/TXT, and service records enumerated |
| Maltego | Corporate email address retrieved |

### 2. Network Scanning — Local LAN (Zenmap)

```bash
ipconfig                 # identify local IP + subnet
# Zenmap: Ping Scan against the discovered subnet
```

- Local subnet identified via `ipconfig`
- **3 live hosts** discovered: `10.36.251.1`, `10.36.251.90`, `10.36.251.162`
- 3 corresponding MAC addresses recorded
- Network topology exported as PDF via Zenmap's Topology view

---

## 📊 Risk Analysis / Impact

| # | Finding | Impact | Risk |
|---|---|---|---|
| 1 | Web technology exposed (WordPress + plugin version) | Aids attacker vulnerability research | 🟠 Medium |
| 2 | Server IP resolvable | Reveals hosting/network location | 🟢 Low |
| 3 | HTTP headers expose `/wp-json/` | Assists fingerprinting/enumeration | 🟢 Low |
| 4 | WAF identifiable (ModSecurity) | Discloses security architecture | 🟢 Low |
| 5 | DNS infrastructure exposed | Broadens org infrastructure profile | 🟠 Medium |
| 6 | Corporate email retrieved via Maltego | Enables targeted phishing | 🟠 Medium |
| 7 | Multiple live hosts on local network | Unauthorized devices may be present | 🟠 Medium |

> These are observations from information-gathering only — **no exploitation or vulnerability validation was performed.** A version, IP, or DNS record being visible does not confirm an exploitable vulnerability; further authorized testing would be required for that.

---

## ✅ Recommendations

1. Regularly review publicly exposed technology/CMS/plugin information
2. Keep CMS, plugins, and web technologies patched and current
3. Periodically audit HTTP response headers for unnecessary disclosure
4. Review DNS records on an ongoing basis
5. Keep the WAF (ModSecurity) enabled, tuned, and monitored
6. Conduct regular internal network discovery scans
7. Investigate any unrecognized device found during scanning
8. Maintain up-to-date network topology documentation
9. Only perform recon/scanning against explicitly authorized targets

---

## 🧠 Key Takeaways

- Passive reconnaissance alone can reveal a surprising amount about a target's technology stack, infrastructure, and personnel — before any active exploitation is attempted.
- A solid security report answers: *what was done → what was found → why it matters → what risk it poses → how to fix it.*
- Scope and authorization are non-negotiable at every stage of engagement.

---

## 📂 Evidence
![image alt](https://github.com/McDIRI/NETWORKWALKS-SOLOMON-B083-WK2-RECONNAISSANCE-NEWORK-SCANNING-PHASE/blob/ade689c9168e63a91b01e0412f0c3dc8cd8c8a67/KALI-TOOL/whois%20screenshut1.png)

![image alt](https://github.com/McDIRI/NETWORKWALKS-SOLOMON-B083-WK2-RECONNAISSANCE-NEWORK-SCANNING-PHASE/blob/ade689c9168e63a91b01e0412f0c3dc8cd8c8a67/KALI-TOOL/whatweb%20screenshut.png)

![image alt](https://github.com/McDIRI/NETWORKWALKS-SOLOMON-B083-WK2-RECONNAISSANCE-NEWORK-SCANNING-PHASE/blob/ade689c9168e63a91b01e0412f0c3dc8cd8c8a67/KALI-TOOL/wafw00f%20screenshut.png)

![image alt](https://github.com/McDIRI/NETWORKWALKS-SOLOMON-B083-WK2-RECONNAISSANCE-NEWORK-SCANNING-PHASE/blob/ade689c9168e63a91b01e0412f0c3dc8cd8c8a67/MALTEGO/Screenshot%20(52).png)

![image alt](https://github.com/McDIRI/NETWORKWALKS-SOLOMON-B083-WK2-RECONNAISSANCE-NEWORK-SCANNING-PHASE/blob/ade689c9168e63a91b01e0412f0c3dc8cd8c8a67/MALTEGO/Screenshot%20(55).png)

![image alt](https://github.com/McDIRI/NETWORKWALKS-SOLOMON-B083-WK2-RECONNAISSANCE-NEWORK-SCANNING-PHASE/blob/ade689c9168e63a91b01e0412f0c3dc8cd8c8a67/ZENMAP/Screenshot%20(69).png)

![image alt](https://github.com/McDIRI/NETWORKWALKS-SOLOMON-B083-WK2-RECONNAISSANCE-NEWORK-SCANNING-PHASE/blob/ade689c9168e63a91b01e0412f0c3dc8cd8c8a67/ZENMAP/Screenshot%20(70).png)

Screenshots for each step (WHOIS, WhatWeb, Nslookup, Curl, Wafw00f, DNSRecon, Zenmap scan results, and topology map) are included in the folders above.

---

## 👤 Author

**Solomon Isaiah Diri**
Cybersecurity Intern, Batch B083 — Networkwalks
🔗 [LinkedIn](https://www.linkedin.com/in/mcdiri/)

---

## 📌 Project Info

| | |
|---|---|
| **Program** | Cybersecurity Internship — Networkwalks |
| **Week** | 02 |

