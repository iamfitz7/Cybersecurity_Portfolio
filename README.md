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

```text
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
---

## 🧾 What Each Lab Includes

Most labs include:

✅ **Goal of the lab** (what I’m trying to learn)  
✅ **Tools used** (example: Wireshark, Packet Tracer, pfSense, etc.)  
✅ **Steps taken** (so someone else can repeat it)  
✅ **Screenshots** (proof of work + evidence)  
✅ **What I learned** (short, honest takeaways)  
✅ **Security / real-world note** (why this matters in real systems)

---

## 🧰 Tools I Commonly Use

These may appear throughout the repo:

- 🖥️ VirtualBox / VMware (virtual lab environments)
- 🧪 Kali Linux + Windows + Linux VMs
- 🌐 Wireshark (packet capture + analysis)
- 🧱 pfSense (firewall + NAT practice)
- 🛡️ Suricata (IDS alert testing)
- 🧠 Packet Tracer (network builds + simulations)
- 📊 Log tools / SIEM practice (searching + alerts)
- 🔎 Scanning tools (only in controlled environments)

---

## ✅ How To Use This Repo

If you’re reviewing this repo, here are easy ways to navigate:

1. Start with **Networking Foundations** if you want basics
2. Check **Wireshark** folders if you like traffic analysis
3. Look at **Firewall/IDS** if you want defensive work
4. Open the **Investigation write-ups** if you want to see how I think through alerts

---

## 📌 Notes & Safety

⚠️ All work in this repository is performed in isolated lab environments.

❌ No scanning or testing of systems I do not own

❌ No unauthorized or real‑world attacks

✅ All activities are strictly for educational and defensive learning purposes

This repository is intended for learning, documentation, and professional development.

---

## 📫 Contact & Links

If you want to connect or have suggestions for improving my documentation:

- GitHub: https://github.com/iamfitz7
- LinkedIn: https://www.linkedin.com/in/fitzgerald-afari-minta-868177352/?trk=public-profile-join-page

Thanks for checking out my work! 🙌
