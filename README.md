🔐 Cybersecurity & Networking Labs Portfolio

Welcome! 👋
This repository is my hands-on lab portfolio documenting practical work in networking, cybersecurity, and SOC-style defensive investigations.

This is not a code library.
It is evidence of real technical practice — how I build environments, test scenarios, investigate alerts, validate findings, and document what I learn using screenshots, logs, configurations, and clear write-ups.

🎯 Why This Repository Exists

The purpose of this portfolio is to:

✅ Demonstrate real hands-on work (not theory-only learning)

✅ Provide verifiable proof of effort (screenshots, configs, logs, investigations)

✅ Build strong networking and security fundamentals

✅ Practice SOC analyst thinking:
observe → validate → investigate → document → decide

✅ Write clearly and honestly, without exaggeration or guesswork

This repo is intentionally detailed so reviewers can see how I think, not just what tools I touched.

🧭 What You’ll Find Here
🌐 Networking Foundations

OSI & TCP/IP mapping using real traffic examples

IP addressing and subnetting (network, broadcast, usable ranges)

VLAN segmentation and basic routing behavior

Packet Tracer topology builds with connectivity validation (ping, traceroute)

🕵️ Traffic Analysis (Wireshark / tcpdump)

TCP 3-way handshake evidence (SYN → SYN/ACK → ACK)

UDP behavior and how investigations differ from TCP

DNS query/response visibility (SOC-relevant traffic)

HTTP request/response behavior and changes under HTTPS

Practical filtering by protocol, port, IP, and fields

🧱 Defensive Network Security (pfSense / Suricata / VPN)

Firewall policy design (allow/deny rules, rule order, before/after testing)

NAT configuration and traffic flow understanding

IDS alert generation from controlled scans

Alert tuning to reduce false positives

VPN deployment and encrypted traffic validation using packet capture

Defense-in-depth concepts: firewall + IDS + VPN working together

🧩 Identity & Windows Security (Active Directory / Logs)

Active Directory basics: users, groups, OUs

Group Policy creation and enforcement (security hardening examples)

Windows Security log review (failed logons, audit evidence)

🔐 Cryptography & TLS / PKI

Symmetric vs asymmetric encryption (practical understanding)

Hashing concepts (SHA-256 vs MD5) and integrity validation

Self-signed certificates and basic PKI concepts

TLS enablement and verification using browser + Wireshark handshakes

📊 SIEM & SOC Investigations (Splunk)

Index discovery and log source validation

SPL fundamentals with logic verification (AND/OR, grouping, thresholds)

Raw vs parsed log awareness and field usage

Mission Control workflows (ownership, status, rule context)

SOC-style investigations with defensible conclusions and correct language

⭐ Featured SOC Investigations (High-Signal Work)

Examples of the strongest SOC-relevant projects in this repository:

Suspicious PowerShell LOLBAS Investigation
Alert → detection rule → SPL → evidence review → OSINT enrichment → decision

Malicious Domain Access Allowed (Proxy + OSINT)
Investigating allowed traffic risk using proxy/Zscaler logs
Multi-user correlation + VirusTotal / urlscan.io enrichment

True Positive vs False Positive vs Rule Tuning Decisions
Clear decision-making with proper L1 boundaries and escalation logic

High-Volume Outbound Transfer Detection & Prioritization
Built reusable SPL workflows to convert bytes → MB, apply thresholds, and rank offenders
Correct framing: potential exfiltration patterns, not confirmed theft

Vulnerable Notepad++ Execution Investigation (Sysmon / Splunk)
Scoped impacted hosts, ranked frequency, reviewed lineage and execution context
Documented telemetry limits honestly and reached a defensible conclusion

🗂️ Repository Structure

This repository is organized primarily by week-based folders, with each week containing one or more labs and their deliverables.

Typical lab contents include:

README.md — lab write-up

Case_File.md — investigation-style case file (when applicable)

screenshots/ — evidence images with consistent naming

Supporting notes or exports (when used)

Example Structure:

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

✅ Clear goal (what I’m learning)

✅ Tools used

✅ Repeatable steps

✅ Screenshots and logs as proof

✅ Findings and what they mean

✅ A realistic outcome (what I would do next)

If something cannot be proven from the available telemetry, I document that limitation instead of guessing.

🧰 Tools & Platforms Used

You may see these tools throughout the repo:

Splunk Enterprise / Splunk ES (Search & Reporting, Mission Control)

Wireshark and tcpdump

Cisco Packet Tracer

pfSense firewall

Suricata IDS

OpenVPN concepts and validation workflows

Windows Server (Active Directory, Group Policy)

Windows Event Viewer and Security logs

Linux CLI utilities (permissions, hashing, OpenSSL)

OSINT tools used for enrichment (when appropriate):

VirusTotal

urlscan.io

✅ How To Review This Repo

If you’re reviewing this portfolio:

Start with Weeks 1–4 for networking and packet analysis fundamentals

Review Weeks 5–8 for defense-in-depth (firewall, IDS, VPN, AD, TLS)

Focus on Weeks 9–11 for SIEM and SOC investigation work (highest job relevance)

📌 Safety & Ethics

⚠️ All work in this repository is performed in isolated lab environments.

❌ No unauthorized scanning

❌ No real-world attacks

✅ Strictly educational and defensive activities only

📫 Contact & Links

GitHub: https://github.com/iamfitz7

LinkedIn: https://www.linkedin.com/in/fitzgerald-afari-minta-868177352/

Thanks for checking out my work! 🙌
