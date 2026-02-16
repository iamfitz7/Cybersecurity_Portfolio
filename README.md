# 🔐 Cybersecurity & Networking Labs Portfolio

Welcome! 👋  
This repository documents my hands-on work in **networking, defensive security, and SOC-style investigation workflows**.

This is **not a code repository**.  
It is **evidence of practical technical work** showing how I build environments, validate network/host behavior, investigate security signals in a SIEM, and document findings using **screenshots, logs, configurations, exports, and structured write-ups**.

Everything here is written to reflect real analyst habits: **verify the data → reduce noise → capture evidence → explain what it proves → document limits → decide next action**.

---

## 🎯 Purpose of This Repository

This portfolio exists to:

- ✅ Demonstrate **real hands-on practice** with verifiable proof (screenshots, logs, configs, exports)
- ✅ Build strong fundamentals in **networking + defensive security** that SOC work depends on
- ✅ Practice a repeatable investigation loop: **observe → validate → scope → enrich → document → decide**
- ✅ Show how I think during investigations (what I checked, why I checked it, and what I concluded)
- ✅ Keep my work **accurate and defensible** (no exaggerated claims, no guessing beyond telemetry)

---

## 🧭 What You’ll Find Here

### 🌐 Networking Foundations (Cisco Packet Tracer)
Hands-on labs that prove I can configure, validate, and troubleshoot basic networks.

What I did (evidence-based):
- Assigned **static IPs** correctly on the same subnet and verified connectivity using **ICMP ping**
- Subnetted networks and documented:
  - **network address**
  - **broadcast address**
  - **usable host ranges**
  - common boundaries used in real environments (examples: /24, /25, /26, /28, /16, /22)
- Built **VLAN segmentation** to separate broadcast domains and validated segmentation behavior using connectivity tests
- Performed **traceroute path analysis** to understand hop-by-hop routing visibility and identify where a path breaks
- Practiced explaining what tests actually prove:
  - ping = reachability (not “the app is working”)
  - traceroute = path visibility (not full end-to-end service validation)

Portfolio proof you’ll see:
- Topology screenshots, IP schemes, VLAN diagrams, ping/traceroute validation screenshots, and short write-ups explaining results.

---

### 🕵️ Traffic Analysis (Wireshark / tcpdump)
Packet-level labs showing I can collect traffic, filter effectively, and interpret what happened on the wire.

What I did (evidence-based):
- Captured and explained the **TCP 3-way handshake** (SYN → SYN/ACK → ACK) and what each flag proves
- Compared **TCP vs UDP** behavior using real captures (stateful session vs connectionless packets)
- Captured and analyzed **DNS queries/responses**, explaining what the packets prove about resolution
- Captured **HTTP** traffic and explained request/response behavior (GET/POST, headers, status codes)
- Demonstrated the visibility limits of **HTTPS** and why encryption changes what analysts can confirm from packet captures
- Built “story” captures that show real flow:
  - **DNS resolves → client connects → HTTP request occurs**
- Practiced investigation efficiency:
  - filtering by protocol, port, IP, and key fields to isolate signal fast

Portfolio proof you’ll see:
- PCAP screenshots, filter screenshots, and write-ups that connect packet fields to what’s actually happening.

---

### 🧱 Defensive Network Security (pfSense / Suricata / VPN + Validation)
Labs focused on building and validating defensive controls the way small enterprise environments work.

What I did (hands-on + validation):
- Built firewall policies in **pfSense** and validated enforcement with before/after testing:
  - allow vs deny behavior
  - rule matching and why **rule order matters**
- Configured and documented **NAT** behavior and how it changes traffic flow and visibility
- Deployed **Suricata IDS** and generated alerts using controlled scan-style traffic
- Tuned IDS settings to reduce noise:
  - adjusted thresholds / reduced repetitive alerts
  - documented why tuning is a real SOC skill (signal-to-noise)
- Deployed a **VPN tunnel** and validated it worked end-to-end
- Proved encryption with packet-level evidence:
  - captured traffic while VPN was active
  - confirmed tunnel behavior vs plaintext expectations
- Built an integrated “stack” lab:
  - **Firewall + IDS + VPN** working together as defense-in-depth
  - documented what each layer contributes and what each layer can/can’t see

Portfolio proof you’ll see:
- pfSense rule screenshots, NAT screenshots, Suricata alert evidence, VPN setup evidence, Wireshark validation screenshots, and layered-security explanations.

---

### 🧩 Identity & Windows Security (Active Directory / Group Policy / Windows Logs)
Identity and endpoint monitoring fundamentals that support real investigations.

What I did (hands-on + evidence):
- Built Active Directory structure:
  - created users, groups, and OUs
  - documented why OU structure matters for administration and policy targeting
- Created and applied **Group Policy Objects (GPOs)**:
  - verified policies actually applied (not just created)
- Implemented basic hardening via policy (example: password policy controls) and explained why it reduces credential risk
- Used **Windows Event Viewer** to identify authentication anomalies:
  - reviewed Security logs
  - captured evidence of failed logins and explained how logs support detection

Portfolio proof you’ll see:
- AD screenshots, OU/group structure evidence, GPO configuration screenshots, policy validation screenshots, and Windows event log evidence.

---

### 🔐 Cryptography & TLS / PKI (Evidence-Based)
Labs that show I understand crypto in practical security terms — not just definitions.

What I did (hands-on + validation):
- Generated and compared hashes (SHA-256 vs MD5) and documented integrity verification use cases
- Generated a **self-signed certificate** and documented how certificates map identity to key pairs
- Enabled **TLS on a local web server** and documented the configuration and browser trust behavior
- Verified TLS with evidence:
  - confirmed TLS usage via browser indicators
  - validated handshake behavior at a high level in Wireshark (ClientHello/ServerHello concepts)
- Built an integrated security reasoning example:
  - how encryption + identity + secure transport fit together in real systems

Portfolio proof you’ll see:
- OpenSSL outputs, certificate artifacts, TLS configuration evidence, and Wireshark/browsers validation screenshots.

---

### 📊 SIEM & SOC Investigations (Splunk Enterprise / Splunk ES)
The highest job-relevance section: SIEM hygiene, SPL accuracy, Mission Control workflow discipline, and case-style investigations.

#### ✅ SIEM Foundations (How I avoid rookie mistakes)
What I did (hands-on):
- **Index discovery + telemetry mapping**
  - identified which indexes contain SOC-relevant data (example: CrowdStrike-style telemetry index)
  - verified events exist before assuming “no evidence”
- **Field extraction awareness**
  - compared raw logs (`_raw`) vs parsed fields
  - documented how missing extraction causes false “empty results”
- **SPL logic validation**
  - practiced AND/OR precedence, parentheses grouping, and accurate filtering
  - compared GUI click-to-filter SPL vs manual SPL to confirm result consistency
- **Mission Control workflow hygiene**
  - took ownership of alerts
  - updated statuses (New → In Progress)
  - opened detection rules directly from alerts
  - reproduced detection SPL in Search & Reporting to validate alerts before investigating deeper

---

## ⭐ Featured SOC Investigations (High-Signal Work)

These are the investigations that best demonstrate real SOC L1 habits: validation, scoping, enrichment, and defensible decisions.

### 1) Suspicious PowerShell LOLBAS Investigation (Splunk Mission Control + OSINT)
What I did (end-to-end):
- Opened alert in Mission Control and captured alert context (host/user/time window)
- Opened the linked detection rule and reproduced SPL in Search & Reporting
- Reviewed returned events and focused on high-signal fields:
  - host/user identifiers
  - command-line arguments
  - embedded URLs
- Deduplicated indicators to reduce noise and isolate unique signals
- Enriched indicators with OSINT (VirusTotal) to support (not replace) behavioral analysis
- Escalated based on command-line behavior consistent with LOLBAS abuse (not just “VT is red”)

What this proves:
- I can interpret suspicious PowerShell behavior and escalate with evidence, not assumptions.

---

### 2) Malicious Domain Access Allowed (Zscaler Proxy + OSINT Correlation)
What I did (end-to-end):
- Investigated an alert sourced from proxy logs with emphasis on **allowed** traffic exposure risk
- Reproduced SPL and isolated relevant fields:
  - domain, user, category, action (allowed)
- Aggregated results to identify:
  - repeated domain access
  - domains accessed by multiple users (stronger signal)
- Exported and deduplicated domains to reduce noise
- Performed OSINT enrichment (VirusTotal + urlscan.io)
- Classified outcomes:
  - high-confidence malicious → recommend blocking/escalate
  - low-confidence/unconfirmed → monitor or tune, avoid overreaction
  - unresolved domains → document limitations and handle proportionally

What this proves:
- I can triage proxy-based risk and balance security vs noise using evidence.

---

### 3) SOC Decision-Making Lab (True Positive vs False Positive vs Tuning)
What I did:
- Investigated separate alerts and formally documented:
  - classification (TP / FP / tune)
  - evidence used
  - action taken (close / escalate / recommend tuning)
- Documented SOC L1 boundaries clearly:
  - what I can conclude from available telemetry
  - what must be handed off to L2/L3/IR or detection engineering

What this proves:
- I understand that **judgment + restraint** are core SOC skills, not just tool usage.

---

### 4) High-Volume Outbound Transfer Detection & Prioritization (Zscaler Logs + SPL Workflow)
What I built and validated:
- Verified the correct data source and required fields existed before building logic
- Normalized byte values into MB for human-readable evidence
- Applied a threshold to isolate anomalous transfers (volume anomaly signal)
- Produced ranked “top offenders” views:
  - top users
  - top hosts
  - top departments
  - top destination IPs
- Documented correct framing:
  - volume anomaly ≠ confirmed exfiltration
  - this is triage and prioritization for investigation

What this proves:
- I can turn large log volumes into a short prioritized list (real SOC workflow).

---

### 5) Vulnerable Notepad++ Execution Investigation (Sysmon + Splunk)
What I did (from raw telemetry, not alerts):
- Validated dataset/index availability and set the correct time range
- Identified vulnerable versions and measured frequency (version sprawl exposure)
- Scoped impacted hosts (blast radius) using distinct host counts
- Prioritized by frequency (triage signal, not proof of compromise)
- Analyzed process lineage (parent processes) and execution context (paths/directories)
- Attempted network correlation and documented telemetry limits honestly
- Produced a defensible conclusion and recommended patching to reduce exposure

What this proves:
- I can scope vulnerable software exposure and avoid false claims when telemetry is limited.

---

## 🛡️ Potential Data Exfiltration (Splunk)

Week 12 is a full SOC-style case folder built around a single alert and expanded into a complete investigation package.  
This work reflects how an L1 analyst investigates potential exfiltration in a real SOC: **validate the alert, reduce noise, enrich with context, identify indicators, and escalate with defensible evidence**.

### ✅ Lab 1 (Strongest): SOC Case File — Potential Data Exfiltration to Cloud Storage
What I did (end-to-end case workflow):
- Opened the alert in Mission Control and documented why it fired (alert context + detection intent)
- Reproduced the detection SPL in Search & Reporting to confirm the alert is valid and repeatable
- Converted outbound byte counts to MB to make evidence readable and report-ready
- Built prioritization views to identify the highest-risk behavior:
  - top users, hosts, departments, destinations
- Pivoted from volume-focused logs into proxy/web logs for enrichment:
  - URLs, content types, categories, user agents
- Identified scripted transfer indicators (ex: curl/python user-agent patterns) and separated automation from normal browser traffic
- Documented what is known, unknown, and outside telemetry scope
- Wrote escalation notes with:
  - evidence summary
  - why it matters
  - what IR should validate next (endpoint review, credential review, data ownership confirmation)

Artifacts included:
- Alert context screenshots
- Detection SPL reproduction evidence
- Prioritization tables (ranked offenders)
- Web log enrichment screenshots
- Escalation notes + case summary

Correct framing:
- ✅ “High-volume outbound transfer activity consistent with potential exfiltration”
- ❌ Not “confirmed exfiltration”

---

### ✅ Lab 2 (Support Lab): SPL Workflow — High-Volume Outbound Transfer Triage Pack
What I built:
A reusable set of SPL searches designed to answer the same triage questions fast:

- Who is sending the most outbound data?
- Which hosts are responsible?
- Which departments show spikes?
- Which destinations repeat?
- What time window is hottest?

What I did:
- Confirmed field availability and index readiness before building searches
- Normalized bytes to MB so results are readable
- Built modular queries that produce ranked outputs and time-based spike views
- Documented why this matters:
  - this is how SOCs reduce “tons of logs” into a short prioritized queue

Artifacts included:
- Saved SPL searches
- Ranked results screenshots
- Time-window / spike analysis screenshots

---

## 🧾 Evidence Standards

Most labs include:

- ✅ Clear goal (what I’m validating or investigating)
- ✅ Tools used and what the tool is proving
- ✅ Repeatable steps (enough for someone else to reproduce)
- ✅ Evidence (screenshots, logs, exports, configs)
- ✅ Findings + what the evidence supports
- ✅ Limits (what I cannot confirm from available telemetry)
- ✅ Outcome (close / escalate / recommend tuning / recommend hardening)

I avoid exaggeration.  
If something can’t be proven with telemetry, I document that limitation instead of guessing.

---

## 🧰 Tools & Platforms Used

You may see these tools across the repository:

- Splunk Enterprise / Splunk ES (Search & Reporting, Mission Control, Detection Rules)
- Wireshark and tcpdump
- Cisco Packet Tracer
- pfSense firewall
- Suricata IDS
- VPN deployment + encryption validation workflows
- Windows Server (Active Directory, Group Policy)
- Windows Event Viewer (Security logs)
- Linux CLI utilities (users/groups, permissions, OpenSSL)
- SQL (MariaDB) for structured security event analysis
- OSINT enrichment tools:
  - VirusTotal
  - urlscan.io

---

## ✅ How to Review This Repository

For reviewers:

- Start with **Networking + Traffic Analysis** to see fundamentals and evidence discipline
- Review **Defensive Network Security + Identity** to see control implementation and validation
- Focus on **SIEM + SOC Investigations** for the most job-relevant work:
  - SPL accuracy
  - Mission Control workflow discipline
  - case files with evidence + escalation reasoning
  - Week 12 is the strongest “full case” example

---

## 📌 Safety & Ethics

⚠️ All work in this repository is performed in **isolated lab environments**.

- ❌ No unauthorized scanning or testing of real systems
- ❌ No real-world attacks
- ✅ Educational and defensive practice only

---

## 📫 Contact & Links

- **GitHub:** https://github.com/iamfitz7  
- **LinkedIn:** https://www.linkedin.com/in/fitzgerald-afari-minta-868177352/

Thanks for taking the time to review my work! 🙌

---

## 🗂️ My Repository Structure

```text
Cybersecurity_Portfolio/
├── README.md
├── screenshots-guidelines.md

├── networking-fundamentals/
│   ├── packet-tracer-labs/
│   ├── subnetting-exercises/
│   ├── vlan-segmentation/
│   ├── traceroute-analysis/
│   ├── screenshots/
│   └── README.md

├── traffic-analysis/
│   ├── wireshark-handshakes/
│   ├── dns-analysis/
│   ├── http-https-analysis/
│   ├── tcp-vs-udp-comparisons/
│   ├── filters-notes/
│   ├── combined-traffic-narratives/
│   ├── screenshots/
│   └── README.md

├── defensive-network-security/
│   ├── firewall-rules/
│   ├── nat-analysis/
│   ├── ids-alert-testing/
│   ├── ids-tuning/
│   ├── vpn-validation/
│   ├── layered-defense-stack/
│   ├── screenshots/
│   └── README.md

├── identity-and-endpoint-security/
│   ├── active-directory/
│   ├── group-policy/
│   ├── password-policy-hardening/
│   ├── windows-event-logs/
│   ├── vpn-ad-integration/
│   ├── screenshots/
│   └── README.md

├── cryptography-and-tls/
│   ├── hashing-integrity/
│   ├── certificates-pki/
│   ├── tls-server-setup/
│   ├── tls-handshake-validation/
│   ├── screenshots/
│   └── README.md

├── siem-and-detections/
│   ├── splunk-installation/
│   ├── index-validation/
│   ├── spl-workflows/
│   ├── alert-logic/
│   ├── log-parsing-and-fields/
│   ├── mission-control-workflow/
│   ├── screenshots/
│   └── README.md

├── soc-investigations/
│   ├── suspicious-powershell-lolbas/
│   │   ├── osint-enrichment/
│   │   ├── escalation-notes/
│   │   ├── screenshots/
│   │   └── README.md
│   │
│   ├── malicious-domain-access-allowed/
│   │   ├── proxy-log-analysis/
│   │   ├── osint-enrichment/
│   │   ├── escalation-decision/
│   │   ├── screenshots/
│   │   └── README.md
│   │
│   ├── decision-notes/
│   │   ├── escalation-summaries/
│   │   ├── false-positive-closures/
│   │   ├── tuning-recommendations/
│   │   └── README.md
│   │
│   ├── volume-detection/
│   │   ├── zscaler-high-volume-transfers/
│   │   ├── screenshots/
│   │   └── README.md
│   │
│   ├── vulnerable-software/
│   │   ├── notepadpp-analysis/
│   │   ├── screenshots/
│   │   └── README.md
│   │
│   ├── fallback-endpoint-pivot/
│   │   ├── telemetry-inventory/
│   │   ├── limitations-and-escalation/
│   │   ├── screenshots/
│   │   └── README.md
│   │
│   └── week-12-potential-data-exfiltration/
│       ├── soc-case-file/
│       │   ├── alert-context/
│       │   ├── detection-reproduction/
│       │   ├── prioritization/
│       │   ├── enrichment/
│       │   ├── escalation-notes/
│       │   ├── screenshots/
│       │   └── README.md
│       │
│       └── triage-pack/
│           ├── spl-searches/
│           ├── ranked-results/
│           ├── time-window-analysis/
│           ├── screenshots/
│           └── README.md

└── additional-security-labs/
    ├── linux-iam-permissions/
    ├── suricata-tcpdump-installation/
    ├── sql-security-event-analysis/
    ├── openssl-crypto-and-integrity/
    ├── network-ir-traffic-analysis/
    ├── compliance-controls-assessment/
    ├── web-app-incident-response/
    ├── kc7-scenarios/
    └── README.md
