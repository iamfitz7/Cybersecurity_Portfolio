# 🔐 Cybersecurity & Networking Labs Portfolio

Welcome! 👋  
This repository documents my hands-on work in **networking, cybersecurity, and SOC-style defensive investigations**.

This is **not a code repository**.  
It is **evidence of practical technical work on how I build environments, validate behavior, investigate security signals, and document findings using screenshots, logs, configurations, and structured write-ups.**

---

## 🎯 Purpose of This Repository

This portfolio exists to:

- ✅ Demonstrate real, hands-on technical practice (not theory-only learning)
- ✅ Provide clear proof of work through evidence (screenshots, logs, configs)
- ✅ Build and reinforce strong networking and security fundamentals
- ✅ Practice SOC-style reasoning: **observe → validate → investigate → document → decide**
- ✅ Communicate findings clearly and professionally for others to follow

---

## 🧭 What You’ll Find Here

### 🌐 Networking Foundations
- OSI and TCP/IP model mapping with real traffic examples
- IP addressing and subnetting (network, broadcast, usable ranges)
- VLAN segmentation and basic routing behavior
- Packet Tracer topology builds with connectivity validation (ping, traceroute)

### 🕵️ Traffic Analysis (Wireshark / tcpdump)
- TCP three-way handshake analysis (SYN → SYN/ACK → ACK)
- UDP behavior and investigation differences versus TCP
- DNS query and response visibility (SOC-relevant context)
- HTTP request/response behavior (GET/POST) and HTTPS visibility limits
- Efficient filtering by protocol, port, IP, and fields

### 🧱 Defensive Network Security (pfSense / Suricata / VPN)
- Firewall policy design: allow/deny rules, rule order, and validation testing
- NAT configuration and traffic flow documentation
- IDS alert generation using controlled scans and alert tuning to reduce noise
- VPN deployment with encryption verification via packet captures
- Defense-in-depth demonstrations: firewall + IDS + VPN working together

### 🧩 Identity & Windows Security (Active Directory / Logs)
- Active Directory fundamentals (users, groups, OUs)
- Group Policy creation and enforcement for basic hardening
- Windows Security log review (authentication failures and audit evidence)

### 🔐 Cryptography & TLS / PKI
- Symmetric vs asymmetric encryption (practical understanding)
- Hashing (SHA-256 vs MD5) and integrity verification
- Self-signed certificates and PKI fundamentals
- TLS enablement and verification using browser indicators and Wireshark

### 📊 SIEM & SOC Investigations (Splunk)
- Index discovery and log source validation (avoiding incorrect data assumptions)
- SPL fundamentals and logic validation (AND/OR grouping, field-based searches)
- Raw vs parsed log awareness and field extraction dependencies
- Mission Control workflow discipline (ownership, status tracking, rule validation)
- SOC-style investigations with defensible conclusions and accurate language

---

## ⭐ Featured SOC Investigations (High-Signal Work)

Examples of the most SOC-relevant projects in this repository include:

- **Suspicious PowerShell LOLBAS Investigation**
  - Alert → detection rule → SPL → evidence review → OSINT enrichment → escalation decision

- **Malicious Domain Access Allowed (Proxy + OSINT)**
  - Focused on *allowed* proxy traffic risk using Zscaler logs  
  - Multi-user correlation with VirusTotal and urlscan.io enrichment

- **True Positive vs False Positive vs Detection Tuning**
  - Structured alert classification with clear SOC L1 role boundaries

- **High-Volume Outbound Transfer Detection & Prioritization**
  - Built a reusable SPL workflow to normalize data volume, apply thresholds, and rank offenders  
  - Correct framing: *potential exfiltration patterns*, not confirmed data theft

- **Vulnerable Notepad++ Execution Investigation (Sysmon / Splunk)**
  - Scoped impacted hosts, prioritized by frequency, analyzed process lineage and execution context  
  - Documented telemetry limitations and produced a defensible conclusion

---

## 🗂️ Repository Structure

The repository is primarily organized into **week-based folders**, each containing one or more labs and their supporting materials.

Typical contents within a lab folder include:

- `README.md` — Lab write-up
- `Case_File.md` — Investigation-style case file (when applicable)
- `screenshots/` — Evidence images with consistent naming
- Supporting notes or exports (when used)

---

## 🧾 Evidence Standards

Most labs include:

- ✅ Clear goal (what the lab is validating or demonstrating)
- ✅ Tools used
- ✅ Repeatable steps
- ✅ Screenshots and logs as proof
- ✅ Findings and interpretation
- ✅ A clear outcome (what I would do next in a real environment)

I avoid exaggeration.  
If something cannot be proven with available telemetry, that limitation is explicitly documented.

---

## 🧰 Tools & Platforms Used

You may see the following tools throughout the repository:

- Splunk Enterprise / Splunk ES (Search & Reporting, Mission Control)
- Wireshark and tcpdump
- Cisco Packet Tracer
- pfSense firewall
- Suricata IDS
- VPN technologies and validation workflows
- Windows Server (Active Directory, Group Policy)
- Windows Event Viewer and Security logs
- Linux CLI utilities (permissions, hashing, OpenSSL)
- OSINT tools for investigation enrichment:
  - VirusTotal
  - urlscan.io

---

## ✅ How to Review This Repository

For reviewers:

- Start with **Weeks 1–4** for networking and packet analysis fundamentals
- Review **Weeks 5–8** for defense-in-depth (firewall, IDS, VPN, AD, TLS)
- Focus on **Weeks 9–11** for SIEM and SOC investigation work (highest job relevance)

---

## 📌 Safety & Ethics

⚠️ All work in this repository is performed in **isolated lab environments**.

- ❌ No unauthorized scanning or testing of real systems
- ❌ No real-world attacks
- ✅ Activities are strictly educational and defensive

---

## 📫 Contact & Links

- **GitHub:** https://github.com/iamfitz7  
- **LinkedIn:** https://www.linkedin.com/in/fitzgerald-afari-minta-868177352/

Thanks for taking the time to review my work! 🙌

## 🗂️ My Repository Structure

```text
Cybersecurity_Portfolio/
├── README.md

├── networking-fundamentals/
│   ├── packet-tracer-labs/
│   ├── subnetting-exercises/
│   ├── screenshots/
│   └── README.md

├── traffic-analysis/
│   ├── wireshark-handshakes/
│   ├── dns-http-analysis/
│   ├── filters-notes/
│   ├── screenshots/
│   └── README.md

├── defensive-network-security/
│   ├── firewall-rules/
│   ├── nat-analysis/
│   ├── ids-alert-testing/
│   ├── vpn-validation/
│   ├── screenshots/
│   └── README.md

├── identity-and-endpoint-security/
│   ├── active-directory/
│   ├── group-policy/
│   ├── windows-event-logs/
│   ├── screenshots/
│   └── README.md

├── cryptography-and-tls/
│   ├── hashing-integrity/
│   ├── certificates-pki/
│   ├── tls-handshake-validation/
│   ├── screenshots/
│   └── README.md

├── siem-and-detections/
│   ├── index-validation/
│   ├── spl-workflows/
│   ├── alert-logic/
│   ├── screenshots/
│   └── README.md

├── soc-investigations/
│   ├── volume-detection/
│   │   ├── spl-workflows/
│   │   ├── screenshots/
│   │   └── README.md
│   │
│   ├── vulnerable-software/
│   │   ├── notepadpp-analysis/
│   │   ├── screenshots/
│   │   └── README.md
│   │
│   ├── alert-enrichment/
│   │   ├── web-log-pivots/
│   │   ├── osint-enrichment/
│   │   ├── screenshots/
│   │   └── README.md
│   │
│   ├── decision-notes/
│   │   ├── escalation-summaries/
│   │   ├── false-positive-closures/
│   │   ├── tuning-recommendations/
│   │   └── README.md
│
└── screenshots-guidelines.md
