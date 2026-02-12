# 🔐 Cybersecurity & Networking Labs Portfolio

Welcome! 👋  
This repository is my hands-on lab portfolio where I document practical work in networking, cybersecurity, and SOC-style defensive investigations.

This is **not a code library**. It is **evidence of real technical practice**: how I build, test, troubleshoot, investigate, and document what I learn using screenshots, logs, configurations, and clear write-ups.

---

## 🎯 Why This Repository Exists

The goals of this portfolio are to:

- ✅ Show real hands-on practice (not theory-only learning)
- ✅ Prove work with evidence (screenshots, configs, logs, investigation notes)
- ✅ Build strong networking + security fundamentals
- ✅ Practice SOC thinking: **observe → validate → investigate → document → decide**
- ✅ Write in a clear, repeatable way that others can follow

---

## 🧭 What You’ll Find Here

### 🌐 Networking Foundations
- OSI & TCP/IP mapping (with real traffic examples)
- IP addressing, subnetting, network/broadcast/usable ranges
- VLAN segmentation and basic routing behavior
- Packet Tracer topology builds + connectivity validation (ping/traceroute)

### 🕵️ Traffic Analysis (Wireshark / tcpdump)
- TCP 3-way handshake evidence (SYN → SYN/ACK → ACK)
- UDP behavior and investigation differences vs TCP
- DNS query/response visibility (SOC-relevant)
- HTTP request/response behavior (GET/POST) and what changes under HTTPS
- Filtering skills (protocol, port, IP, fields)

### 🧱 Defensive Network Security (pfSense / Suricata / VPN)
- Firewall policy building: allow/deny rules, rule order, testing before/after
- NAT configuration understanding and documentation
- IDS alert generation (controlled scans) + tuning to reduce noise
- VPN deployment and encryption validation using packet capture evidence
- Defense-in-depth: firewall + IDS + VPN working together

### 🧩 Identity & Windows Security (Active Directory / Logs)
- AD basics: users, groups, OUs
- Group Policy creation and enforcement (security hardening examples)
- Windows Security log review (failed logons and audit evidence)

### 🔐 Cryptography & TLS / PKI
- Symmetric vs asymmetric encryption (practical understanding)
- Hashing (SHA-256 vs MD5) and integrity validation
- Self-signed certificates and basic PKI concepts
- TLS enablement + verification using browser + Wireshark handshake evidence

### 📊 SIEM + SOC Investigations (Splunk)
- Index discovery and log source validation (avoiding “wrong index” mistakes)
- SPL fundamentals and logic validation (AND/OR, grouping, field-based searches)
- Raw vs parsed log understanding (field extraction awareness)
- Mission Control workflow discipline (ownership, status tracking, rule validation)
- SOC-style investigations with defensible conclusions and correct language

---

## ⭐ Featured SOC Investigations (High-Signal Work)

These are examples of the strongest “SOC-ready” projects in this repo:

- **Suspicious PowerShell LOLBAS Investigation**
  - Alert → rule → SPL → evidence review → OSINT enrichment → escalation decision

- **Malicious Domain Access Allowed (Proxy + OSINT)**
  - Focused on *allowed* traffic risk using proxy/Zscaler logs  
  - Multi-user correlation + VirusTotal/urlscan enrichment + decision outcomes

- **True Positive vs False Positive vs Tuning Decisions**
  - Structured decision-making with clear role boundaries (what L1 does vs escalates)

- **High-Volume Outbound Transfer Detection & Prioritization**
  - Built a reusable SPL workflow to convert raw bytes → MB, apply thresholds, and rank top offenders  
  - Correct framing: “potential exfiltration patterns” (not confirmed theft)

- **Vulnerable Notepad++ Execution Investigation (Sysmon / Splunk)**
  - Scoped impacted hosts, ranked by frequency, checked lineage + execution context  
  - Documented telemetry limitations honestly and produced a defensible conclusion

---

## 🗂️ Repository Structure

This repo is organized mainly by **week-based folders**, with each week containing one or more labs and their deliverables (write-ups, screenshots, notes, and sometimes case files).

Typical contents you’ll see inside a lab folder:

- `README.md` (the lab write-up)
- `Case_File.md` (when the lab is written as an investigation case file)
- `screenshots/` (evidence images, consistently named)
- supporting notes / exports (when used)

Example of the current structure:

```text`
Cybersecurity_Portfolio/
├─ Week1_OSI_TCPIP/
├─ Week2_Subnetting_PacketTracer/
├─ Week3_DNS_HTTP_DHCP/
├─ Week4_WiresharkDeepDive/
├─ Week5_pfSense/
├─ Week6_Firewall_Advanced_VPN/
├─ Week7_Windows_Active_Directory/
├─ Week8_CryptographyFundamentals/
├─ Week9_Splunk_SIEM/
├─ Week10_SplunkSIEM2/
├─ Week11_SIEM_Investigations_&_Alert_Prioritization/
└─ README.md

🧾 Evidence Standards I Follow

Most labs include:

✅ Goal (what I’m trying to learn)

✅ Tools used

✅ Steps taken (repeatable)

✅ Screenshots / logs (proof)

✅ Findings + what they mean

✅ A clear outcome (what I would do next in a real environment)

I avoid exaggerating results. If something cannot be proven from the available telemetry, I document that limitation instead of guessing.

🧰 Tools & Platforms Used

You may see these tools across different weeks:

Splunk Enterprise / Splunk ES (Search & Reporting, Mission Control)

Wireshark + tcpdump

Cisco Packet Tracer

pfSense firewall

Suricata IDS

OpenVPN concepts + VPN validation workflows

Windows Server (Active Directory, Group Policy)

Windows Event Viewer / Security logs

Linux CLI utilities (permissions, hashing, OpenSSL)

OSINT tools (used for enrichment in investigations):

VirusTotal

urlscan.io

✅ How To Review This Repo

If you’re browsing this as a reviewer:

Start with Weeks 1–4 for networking + packet analysis fundamentals

Check Weeks 5–8 for defense-in-depth (firewall/IDS/VPN/AD/TLS)

Go to Weeks 9–11 for SIEM + SOC investigation work (highest job relevance)

📌 Safety & Ethics

⚠️ All work in this repository is performed in isolated lab environments.

❌ No unauthorized scanning or testing of real systems

❌ No real-world attacks

✅ Activities are strictly educational and defensive

📫 Contact & Links

GitHub: https://github.com/iamfitz7

LinkedIn: https://www.linkedin.com/in/fitzgerald-afari-minta-868177352/

Thanks for checking out my work! 🙌
