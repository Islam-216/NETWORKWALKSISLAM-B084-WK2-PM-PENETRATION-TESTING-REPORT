# 🔐 Penetration Testing Report — Footprinting & Network Scanning Phases

![Program](https://img.shields.io/badge/Program-Networkwalks%20B082-blue?style=for-the-badge)
![Modules](https://img.shields.io/badge/Modules-W2--PM1%20%7C%20W2--PM5-informational?style=for-the-badge)
![OS](https://img.shields.io/badge/Environment-Kali%20Linux%20%7C%20Windows-success?style=for-the-badge)
![Status](https://img.shields.io/badge/Phase%201--2-Completed-brightgreen?style=for-the-badge)
![Phase 3-5](https://img.shields.io/badge/Phase%203--5-In%20Progress-yellow?style=for-the-badge)

> **Official Assessment Deliverable (`W2-PM-FINAL`)** — Practical reconnaissance, web stack fingerprinting, DNS enumeration, and local subnet discovery conducted as part of the **Networkwalks Cybersecurity & Ethical Hacking Internship (Batch B082)**.

---

## ⚠️ Legal & Liability Disclaimer

> **IMPORTANT — AUTHORIZED EDUCATIONAL & RESEARCH USE ONLY**
>
> All security assessments documented in this repository were executed **strictly** against systems where explicit written authorization was secured (`networkwalks.com`) or private infrastructure owned directly by the tester (`192.168.1.0/24`).
>
> * Do **not** use the commands, methodologies, or findings in this repository to test, scan, or access systems without prior written permission from the asset owner.
> * The author, instructors, and **Networkwalks** assume no liability for misuse of this information. Unauthorized access to computer systems is a criminal offense in most jurisdictions, even when no damage occurs.

---

## 📌 Engagement & Project Information

| Attribute | Details |
| :--- | :--- |
| **Pentester Name** | Islam *(Cybersecurity Professional)* |
| **Program / Batch** | `B082` — Networkwalks Cybersecurity & Ethical Hacking Internship |
| **Report Identifier** | `W2-PM-FINAL` — [Download Full DOCX Report](W2-PM-FINAL-Penetration-Testing-Report-Islam.docx) |
| **Modules Completed** | `W2-PM1` (Kali Linux Recon Suite) & `W2-PM5` (Zenmap Network Scanning) |
| **Assessment Date** | 13 November 2026 |
| **Target 1 (External)** | `networkwalks.com` *(Written permission secured)* |
| **Target 2 (Internal)** | `192.168.1.0/24` *(Tester-owned Local Area Network)* |
| **Raw Terminal Logs** | [My Pentest result.txt](<My Pentest result.txt>) |
| **Topology Diagram** | [zenmap-topology.pdf](zenmap-topology.pdf) |
| **Phases Covered** | ✅ Phase 1: Reconnaissance & Footprinting<br>✅ Phase 2: Scanning & Network Discovery<br>⏳ Phases 3–5: Vulnerability Analysis, Exploitation & Post-Exploitation *(In Progress)* |

---

## 📖 1. Executive Summary & Introduction

This repository documents the Week 2 practical penetration testing assessment (`W2-PM-FINAL`), bridging external passive/active intelligence gathering with internal network host discovery:

1. **Footprinting & Reconnaissance (`W2-PM1`):** Executed six industry-standard Kali Linux reconnaissance utilities (`whois`, `whatweb`, `nslookup`, `curl`, `wafw00f`, and `dnsrecon`) against `networkwalks.com` to map domain ownership, authoritative DNS infrastructure, hosting location, web server headers, and CMS/plugin versions.
2. **Network Scanning & Discovery (`W2-PM5`):** Utilized Windows Command Prompt (`ipconfig`) and **Zenmap** (Nmap GUI) to enumerate live hosts, hardware MAC addresses, and physical/logical topology across the `192.168.1.0/24` local subnet.

**Key Assessment Takeaways:**
* **0 Critical / High** immediate exposures identified during non-intrusive discovery.
* **2 Medium** informational/configuration findings (CMS/plugin version disclosure and unsigned DNSSEC with exposed BIND daemon version).
* **4 Low** informational observations (origin IP visibility, verbose HTTP/REST API headers, HTTPS probe timeout behavior, and local host discoverability).

---

## 🎯 2. Scope & Rules of Engagement

| Target Asset | Scope Type | Authorization Status | Permitted Activities | Prohibited Activities |
| :--- | :--- | :--- | :--- | :--- |
| `networkwalks.com` (`192.232.216.135`) | External Domain & Web Application | Written Permission Secured | Passive OSINT, WHOIS, DNS queries, HTTP header inspection, non-intrusive WAF checks | Denial of Service (DoS), brute-force attacks, exploitation, data modification |
| `192.168.1.0/24` | Internal Local Subnet (LAN) | Self-Owned Lab Environment | ICMP/ARP host discovery (`-sn`), MAC address collection, topology mapping | Scanning external ISP gateways or third-party networks |

---

## 🛠️ 3. Tools & Environment Used

| Tool / Platform | Environment | Command Used | Security Purpose |
| :--- | :--- | :--- | :--- |
| **Kali Linux & Windows** | Host / VM Lab | — | Dual-OS testing setup for external reconnaissance and internal LAN discovery |
| **WHOIS** | Kali Linux | `whois networkwalks.com` | Retrieve registrar details, registration/expiry dates, privacy proxy, EPP status, and name servers |
| **WhatWeb** | Kali Linux | `whatweb networkwalks.com` | Fingerprint web server, CMS version, plugins, JavaScript libraries, cookies, and contact emails |
| **nslookup** | Kali Linux | `nslookup networkwalks.com` | Resolve domain FQDN to its hosting IPv4 address via local DNS resolver |
| **cURL** | Kali Linux | `curl -I https://networkwalks.com` | Inspect raw HTTP/1.1 response headers, cache layers, cookie flags, and API link headers |
| **WAFW00F** | Kali Linux | `wafw00f networkwalks.com` | Probe for active Web Application Firewall (WAF) signatures and rate-limiting behavior |
| **DNSRecon** | Kali Linux | `dnsrecon -d networkwalks.com` | Enumerate SOA, NS, A, MX, TXT/SPF, SRV records, DNSSEC status, and BIND version banners |
| **Zenmap (Nmap GUI)** | Windows PC | `nmap -sn 192.168.1.0/24` | Perform ping sweep across the local `/24` subnet, identify live hosts, MACs, and export topology |
| **Windows CMD** | Windows PC | `ipconfig` / `ipconfig /all` | Verify local interface IPv4 address, subnet mask, default gateway, and physical MAC address |

---

## 🔍 4. Methodology & Technical Walkthrough

### 4.1 Phase 1 — Footprinting & Reconnaissance (`W2-PM1`)

#### 1️⃣ Domain Registration Intelligence (`whois`)
* **Command:**
  ```bash
  whois networkwalks.com
