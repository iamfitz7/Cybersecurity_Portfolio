# 🔐 Cybersecurity Portfolio

## 🛡️ Applied Security Projects, Technical Investigations, Engineering, Automation & Control Validation

Welcome to my cybersecurity portfolio!

This repository documents hands-on cybersecurity work across **network security, endpoint security, identity and access management, incident response, detection engineering, cloud security, security automation, Linux administration, infrastructure security, and technical risk/control validation**.

The purpose of this portfolio is not simply to show which tools I have used. Each project is designed to demonstrate how I approach technical security work through **implementation, investigation, validation, troubleshooting, evidence collection, decision-making, remediation, and documentation**.

My general workflow is:

```text
Build / Configure
      ↓
Generate or Observe Activity
      ↓
Collect Telemetry
      ↓
Investigate
      ↓
Correlate Evidence
      ↓
Validate Findings
      ↓
Make a Decision
      ↓
Remediate or Improve
      ↓
Re-Validate
      ↓
Document
```

Projects throughout this repository include **configurations, scripts, KQL queries, SPL searches, logs, packet captures, screenshots, process trees, detection rules, investigation timelines, control assessments, reports, risk documentation, and other technical evidence**.

> ### 👀 Short on time?
>
> Start with the projects below. They represent some of the strongest and most complete work currently in this portfolio:
>
> ☁️ **Microsoft Defender XDR, Microsoft Sentinel & Azure Security**  
> 🛡️ **NIST SP 800-171 / CMMC Technical Control Assessment**  
> 🐧 **Linux Server Administration & Security Hardening**  
> ⚙️ **Python & Security Automation**  
> 🔎 **Windows / Endpoint Incident Investigations**  
> 🌐 **Network Defense, Firewall, IDS/IPS & VPN Security**

---

# 📌 Portfolio at a Glance

| Security Area | Hands-On Work Demonstrated | Technologies / Concepts |
|---|---|---|
| 🌐 **Networking & Network Security** | Network design, subnetting, VLANs, packet analysis, firewall policy, IDS/IPS, VPN validation | TCP/IP, DNS, DHCP, Cisco Packet Tracer, Wireshark, tcpdump, pfSense, Suricata |
| 🪟 **Windows & Identity Security** | Active Directory administration, Group Policy, authentication analysis, access control | Windows Server, AD DS, GPO, Windows Security Logs |
| 🔎 **Security Monitoring & Investigation** | Alert triage, telemetry analysis, event correlation, investigation timelines, evidence review | Splunk, Elastic Security, Microsoft Sentinel |
| 🛡️ **Endpoint Security & EDR** | Process analysis, endpoint timelines, command-line review, malware prevention investigation | Microsoft Defender XDR, Defender for Endpoint, Elastic Defend |
| 🚨 **Incident Response** | Triage, scoping, evidence correlation, classification, response recommendations, validation | Windows Events, EDR telemetry, SIEM data, MITRE ATT&CK |
| 🎯 **Detection Engineering** | Detection logic, KQL hunting, analytics rules, thresholds, ATT&CK mapping, detection validation | Microsoft Sentinel, KQL, SPL |
| ☁️ **Cloud Security** | Identity monitoring, RBAC analysis, administrative activity, least privilege, cloud detections | Microsoft Azure, Entra ID, Azure RBAC, Log Analytics, Sentinel |
| ⚙️ **Security Automation** | Structured data processing, scripting, IOC-focused workflows, repeatable security tasks | Python, PowerShell, Bash, JSON, CSV, APIs, regex |
| 🐧 **Linux Security** | Administration, SSH, user permissions, service management, firewalling, auditing, hardening | Ubuntu Linux, Kali Linux, Bash |
| 📋 **Risk & Control Validation** | Requirement mapping, technical evidence, control assessment, risk identification, remediation tracking | NIST SP 800-171 Rev. 2, CMMC Level 2, POA&M-style documentation |

---

# ⭐ Featured Technical Projects

## ☁️ 1. Microsoft Security Environment Deployment, Defender for Endpoint Onboarding & EDR Investigation

📁 **[Explore Week 17 — Microsoft Defender XDR & Microsoft Sentinel Investigations](./Week17_Defender_XDR_Sentinel_Investigations)**

Designed and deployed a Microsoft security lab environment integrating:

- Microsoft Azure
- Microsoft Entra ID
- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Microsoft Sentinel
- Azure Log Analytics
- Windows 11 Enterprise
- PowerShell
- IIS
- Advanced Hunting
- KQL

### 🔧 Environment Deployment & Troubleshooting

One of the most important parts of this project involved troubleshooting Microsoft Defender for Endpoint onboarding.

The original Windows endpoint failed to onboard and returned **Error ID 15**.

Rather than forcing an unsupported workaround, I investigated the endpoint and identified that the Microsoft Defender for Endpoint **SENSE service was unavailable because the operating system edition did not properly support the required MDE functionality**.

I replaced the endpoint with a supported **Windows 11 Enterprise** system and rebuilt the deployment correctly.

I then validated:

- ✅ Microsoft Defender Antivirus service health
- ✅ Microsoft Defender for Endpoint `SENSE` service
- ✅ Endpoint onboarding status
- ✅ Required registry configuration
- ✅ Communication with Microsoft Defender cloud services
- ✅ Device visibility inside Defender XDR
- ✅ Endpoint telemetry collection

### 🧪 EDR Detection Validation

After successful deployment, I performed Microsoft's official EDR detection test and investigated the generated activity through Defender XDR.

The investigation included:

- 🚨 Incident review
- 🔎 Alert analysis
- 🧭 Device timeline investigation
- 🌳 Process tree review
- 💻 Command-line analysis
- 🎯 Advanced Hunting
- 🧪 KQL queries
- 👤 User correlation
- 🖥️ Device correlation
- ⚙️ Process correlation
- 🌐 Network activity review

Based on the available evidence and known testing context, the activity was classified as:

> **Informational / Expected Activity / Security Testing**

### 🧹 Cleanup & Validation

The project did not stop after generating the detection.

Temporary testing components were removed, IIS-related services were stopped and disabled, temporary files were deleted, TCP port 80 exposure was removed, and the endpoint was returned to a safer state.

### 💡 Key Takeaway

This project demonstrated the complete workflow of:

**deployment → troubleshooting → telemetry validation → detection testing → investigation → classification → cleanup → re-validation**

---

## 🎯 2. Azure Identity, RBAC Monitoring, KQL Threat Hunting & Detection Engineering

📁 **[Explore Week 17 — Defender XDR, Sentinel & Azure Security](./Week17_Defender_XDR_Sentinel_Investigations)**

Built an Azure security monitoring and detection workflow focused on **identity, administrative activity, authorization, RBAC, least privilege, and cloud telemetry**.

### 🧰 Technologies Used

- Microsoft Azure
- Microsoft Entra ID
- Azure RBAC
- Microsoft Sentinel
- Azure Log Analytics
- Azure Activity Logs
- Sign-in Logs
- KQL
- Sentinel Analytics Rules
- Sentinel Automation Rules

### 🔎 Investigation Workflow

I analyzed cloud activity using telemetry including:

```text
AzureActivity
SigninLogs
```

The investigation connected several important cloud security concepts:

```text
Authentication
      ↓
Authorization
      ↓
Administrative Action
      ↓
Telemetry
      ↓
Detection
      ↓
Investigation
      ↓
Remediation
```

### 🚨 Custom Microsoft Sentinel Detection

I developed a custom Sentinel analytics rule:

> **Lab - Azure RBAC Role Assignment Activity**

The rule was configured with:

- 🟡 **Severity:** Medium
- ⏱️ **Query frequency:** Every 5 minutes
- 🕒 **Lookback period:** 10 minutes
- 🎯 **Alert threshold:** Greater than 0
- 🧭 **MITRE ATT&CK mapping:** Privilege Escalation

The detection focused on successful Azure RBAC role-assignment activity.

### 🔐 Least-Privilege Remediation

Temporary Contributor permissions used during testing were removed.

Final access was validated using **Reader** permissions, restoring a more appropriate least-privilege configuration.

### ⚠️ Documented Limitation

Microsoft Entra Conditional Access was reviewed as an additional security control.

It was **not implemented** because the lab did not have the required Microsoft Entra ID P1 licensing.

Rather than presenting the control as completed, the limitation was documented clearly.

A scoped Microsoft Sentinel automation rule was also configured to support incident triage workflow.

### 💡 Key Takeaway

This project connected:

**cloud identity → permissions → administrative activity → telemetry → KQL → detection engineering → investigation → least-privilege remediation**

---

## 📋 3. NIST SP 800-171 Rev. 2 / CMMC Level 2 Technical Control Assessment

📁 **[Explore Technical-to-GRC Risk Bridge Projects](./Technical_To_GRC_Risk_Bridge_Projects)**

Built an isolated security environment and used it to connect technical security implementation with **control requirements, evidence, assessment, risk, remediation, and validation**.

### 🏗️ Lab Environment

The environment included:

- 🔥 pfSense boundary firewall/router
- 🖥️ Windows Server 2022 Domain Controller
- 👥 Active Directory Domain Services
- 🌐 Windows DNS
- 🔐 Group Policy
- 💻 Windows 11 Enterprise endpoint
- 🛡️ Microsoft Defender Antivirus
- 🛡️ Microsoft Defender for Endpoint
- 📑 Windows Security Event Logs
- 💻 PowerShell
- 📦 Oracle VirtualBox

### 🔎 Assessment Methodology

Instead of treating compliance as a checklist exercise, I used an evidence-driven process:

```text
Requirement
    ↓
Implementation
    ↓
Evidence
    ↓
Assessment
    ↓
MET / NOT MET
    ↓
Gap
    ↓
Risk
    ↓
Remediation
    ↓
Owner
    ↓
Validation
```

### 📚 Requirements Assessed

Selected NIST SP 800-171 Rev. 2 / CMMC Level 2 requirements included:

#### 🔐 Access Control

- `AC.L2-3.1.1`
- `AC.L2-3.1.5`

#### 🧾 Audit & Accountability

- `AU.L2-3.3.1`
- `AU.L2-3.3.2`

#### 👤 Identification & Authentication

- `IA.L2-3.5.1`
- `IA.L2-3.5.3`

#### 🌐 System & Communications Protection

- `SC.L2-3.13.1`

#### 🛡️ System & Information Integrity

- `SI.L2-3.14.1`
- `SI.L2-3.14.2`
- `SI.L2-3.14.6`

### 📊 Technical-to-Risk Translation

Technical findings were translated into:

- ✅ Control status
- 📎 Evidence
- ⚠️ Security gaps
- 📊 Risk statements
- 🛠️ Remediation actions
- 👤 Ownership
- 🔄 Validation requirements
- 📋 POA&M-style remediation tracking

### 💡 Key Takeaway

The goal was not simply to say that a security control existed.

The goal was to answer:

> **What technical evidence proves that the control is actually implemented and operating as expected?**

---

## 🐧 4. Linux Server Administration, Hardening & Security Validation

📁 **[Explore Week 21 — Linux Administration & Scripting](./Week21_Linux_Administration_%26_Scripting)**

Built an enterprise-style Linux security administration lab using:

- 🖥️ `linux-srv01` — Ubuntu Linux server
- 💻 `kali-admin01` — Kali Linux administrative/testing workstation
- 🔒 Private VirtualBox management network
- 🌐 Separate NAT connectivity for package management and updates

### 🛠️ Areas Covered

- Linux user administration
- Group management
- File and directory permissions
- Administrative privilege management
- Secure remote access
- SSH
- Host firewall configuration
- Network configuration
- Linux service management
- Logging
- Auditing
- Package management
- System updates
- Security hardening
- Bash scripting
- Administrative validation
- Security testing from a separate system

### 🔎 Validation Mindset

The lab follows a simple security principle:

> **A configuration should not be considered complete simply because a command executed successfully. The resulting system behavior should also be tested and validated.**

That means checking the system from both the administrative and security perspective after changes are made.

---

## ⚙️ 5. Python & Security Automation

📁 **[Explore Week 20 — Python & Security Automation](./Week20_Python_%26_Security_Automation)**

This section expands the portfolio from primarily manual workflows into repeatable security automation.

### 🐍 Areas of Focus

- Python
- File handling
- CSV processing
- JSON processing
- Regular expressions
- IOC validation
- API interaction
- Error handling
- Logging
- Structured output
- Security reporting
- Automation workflow design

### 🔍 Indicator Types

Security automation work includes processing indicators such as:

```text
IP Addresses
Domains
URLs
File Hashes
```

The objective is not simply to write code.

The objective is to use programming to improve the **speed, consistency, repeatability, and documentation** of security workflows.

---

## 🪟 6. Windows Event Log Analysis & Incident Response

📁 **[Explore Week 13 — Windows Event Logs & Incident Response](./Week13_Windows_EventLogs_IncidentResponse)**

Performed Windows security investigations using **Windows Event Viewer and Windows Security logs**.

### 🔎 Authentication Events Investigated

| Event ID | Meaning |
|---|---|
| `4624` | Successful logon |
| `4625` | Failed logon |
| `4634` | Account logoff |
| `4672` | Special privileges assigned to a new logon |

I generated controlled failed-authentication activity and investigated the resulting Event ID `4625` events.

The investigation considered:

- ⏱️ Timing
- 🔢 Number of failures
- 👤 User account
- 🔐 Authentication result
- ✅ Surrounding successful logons
- 🧠 Expected versus unexpected behavior
- 🚨 Potential brute-force indicators

The activity was ultimately classified as **expected lab testing** because the surrounding evidence supported that conclusion.

The activity was also mapped to:

> **MITRE ATT&CK T1110 — Brute Force**

### 💡 Key Takeaway

> **Security events are evidence, not conclusions.**

A failed login could represent a user mistake, broken service, password spraying, brute-force behavior, expected testing, or another condition.

Context determines its meaning.

---

## 🌐 7. Network Security, Firewall, IDS/IPS & VPN Validation

📁 **[Explore Week 5 — pfSense](./Week5_pfSense)**  
📁 **[Explore Week 6 — Advanced Firewall & VPN Security](./Week6_Firewall_Advanced_VPN)**

Built and tested network defense environments using **pfSense, Suricata, VPN technology, and packet-level validation**.

### 🛡️ Work Performed

- Firewall rule creation
- Traffic filtering
- ICMP control
- NAT
- Rule priority testing
- Network segmentation
- Suricata IDS monitoring
- Nmap-generated test traffic
- IDS alert validation
- Detection tuning
- VPN configuration
- Encrypted tunnel validation
- Packet capture analysis

A recurring validation workflow was:

```text
Configure Control
      ↓
Generate Traffic
      ↓
Observe Behavior
      ↓
Capture Evidence
      ↓
Verify Result
```

The same principle was applied to firewall, IDS/IPS, and VPN testing.

---

## 🔎 8. Splunk SIEM Investigations

📁 **[Explore Week 9 — Splunk SIEM](./Week9_Splunk_SIEM)**  
📁 **[Explore Week 10 — Splunk Investigations](./Week10_Splunk2)**  
📁 **[Explore Week 12 — Potential Data Exfiltration](./Week12_Potential_Data_Exfiltration)**

Developed progressively deeper Splunk investigation workflows involving:

- Index discovery
- Raw event review
- Field identification
- SPL
- Search refinement
- Event filtering
- Timeline reconstruction
- Endpoint investigation
- Authentication analysis
- Web activity analysis
- Process activity
- Data-transfer analysis
- Alert validation
- Evidence collection

### 🚨 Investigation Scenarios

Projects included analysis of:

- Suspicious PowerShell activity
- Living-off-the-land behavior
- Potentially malicious domain activity
- High-volume outbound transfers
- Potential data exfiltration
- Endpoint software visibility
- Security alert prioritization
- True-positive versus false-positive decisions

The objective was not simply to locate an event.

The objective was to determine:

> **What happened, what evidence supports it, how significant it is, and what should happen next?**

---

# 🧭 Portfolio Progression

This repository intentionally progresses from technical foundations into increasingly integrated security work.

```text
🌐 Networking Fundamentals
          ↓
🔬 Packet & Protocol Analysis
          ↓
🔥 Firewalls / IDS / VPN
          ↓
🪟 Windows / Active Directory
          ↓
🔐 Cryptography / TLS
          ↓
📊 SIEM
          ↓
🔎 Security Investigations
          ↓
🚨 Incident Response
          ↓
🛡️ Endpoint Security / EDR
          ↓
☁️ Microsoft Security & Azure
          ↓
🎯 Detection Engineering
          ↓
🔐 Identity / RBAC Security
          ↓
⚙️ Security Automation
          ↓
🐧 Linux Security Administration
          ↓
📋 Technical Risk & Control Validation
```

| Stage | Focus | Representative Skills |
|---|---|---|
| 🌐 Foundations | Networking | OSI, TCP/IP, subnetting, DNS, DHCP, VLANs |
| 🔬 Traffic Analysis | Packet-level visibility | Wireshark, tcpdump, TCP, UDP, DNS, HTTP, TLS |
| 🔥 Network Defense | Preventive and detective controls | pfSense, Suricata, firewalling, NAT, VPN |
| 👤 Identity | Authentication and authorization | Active Directory, GPO, Entra ID, RBAC |
| 🔐 Data Protection | Cryptography | Hashing, AES, TLS, certificates |
| 📊 SIEM | Centralized telemetry | Splunk, SPL, Microsoft Sentinel |
| 🚨 Incident Response | Investigation | Windows logs, timelines, event correlation |
| 🛡️ Endpoint Security | Endpoint telemetry | Defender XDR, MDE, Elastic Defend |
| 🎯 Detection Engineering | Detection creation | KQL, analytics rules, ATT&CK mapping |
| ☁️ Cloud Security | Cloud identity and activity | Azure, Entra ID, RBAC, Log Analytics |
| ⚙️ Automation | Repeatable workflows | Python, PowerShell, Bash |
| 🐧 Infrastructure Security | Secure administration | Linux, SSH, firewalling, auditing |
| 📋 Risk & Controls | Security assurance | NIST SP 800-171, CMMC, POA&M |

---

# 🗂️ Repository Map

<details>
<summary><strong>🌐 Weeks 1–6 | Networking, Packet Analysis & Network Defense</strong></summary>

<br>

### 🌐 [Week 1 — OSI Model & TCP/IP](./Week1_OSI_TCPIP)

**Focus:** Network communication fundamentals

- OSI model
- TCP/IP model
- Network communication
- Protocol behavior

### 🧮 [Week 2 — Subnetting & Cisco Packet Tracer](./Week2_Subnetting_PacketTracer)

**Focus:** Addressing, segmentation, and connectivity

- IPv4
- Subnet masks
- Network ranges
- Host ranges
- Cisco Packet Tracer
- Connectivity validation

### 🌍 [Week 3 — DNS, HTTP & DHCP](./Week3_DNS_HTTP_DHCP)

**Focus:** Core network services

- DNS
- HTTP
- DHCP
- DHCP DORA
- Client/server communication

### 🔬 [Week 4 — Wireshark Deep Dive](./Week4_WiresharkDeepDive)

**Focus:** Packet-level analysis

- TCP three-way handshake
- TCP versus UDP
- DNS packet analysis
- HTTP analysis
- HTTPS visibility
- Wireshark filters

### 🔥 [Week 5 — pfSense Firewall Security](./Week5_pfSense)

**Focus:** Network defense

- Firewall policies
- Rule creation
- ICMP filtering
- NAT
- Segmentation
- Suricata IDS
- Security validation

### 🔒 [Week 6 — Advanced Firewall & VPN Security](./Week6_Firewall_Advanced_VPN)

**Focus:** Layered network security

- Advanced firewall rules
- Rule priority
- VPN deployment
- IDS/IPS integration
- Encrypted communication
- Packet validation

</details>

---

<details>
<summary><strong>🪟 Weeks 7–8 | Windows Identity & Cryptography</strong></summary>

<br>

### 👥 [Week 7 — Windows Active Directory](./Week7_Windows_Active_Directory)

**Focus:** Enterprise Windows identity

- Active Directory Domain Services
- Organizational Units
- Users and groups
- Group Policy
- Password policy
- Authentication
- Windows administration

### 🔐 [Week 8 — Cryptography Fundamentals](./Week8_CryptographyFundamentals)

**Focus:** Cryptographic protection

- SHA-256
- MD5
- Hashing
- AES
- Certificates
- TLS
- PKI concepts
- Encrypted communications

</details>

---

<details>
<summary><strong>📊 Weeks 9–12 | SIEM & Security Investigation</strong></summary>

<br>

### 📊 [Week 9 — Splunk SIEM](./Week9_Splunk_SIEM)

- Splunk
- Event ingestion
- Index discovery
- SPL
- Security telemetry
- Investigation fundamentals

### 🔎 [Week 10 — Splunk Investigation Workflows](./Week10_Splunk2)

Expanded Splunk security analysis and investigative workflows.

### 🚨 Week 11 — SIEM Investigations & Alert Prioritization

Focused on:

- Alert interpretation
- Evidence validation
- Investigation priority
- Security context
- Decision-making

### 📤 [Week 12 — Potential Data Exfiltration](./Week12_Potential_Data_Exfiltration)

Focused on investigating suspicious outbound activity and determining whether the available telemetry supported an exfiltration hypothesis.

</details>

---

<details>
<summary><strong>🚨 Weeks 13–16 | Incident Response & Endpoint Investigation</strong></summary>

<br>

### 🪟 [Week 13 — Windows Event Logs & Incident Response](./Week13_Windows_EventLogs_IncidentResponse)

- Windows Event Viewer
- Authentication events
- Event ID analysis
- Failed-login investigation
- Event correlation
- Incident response reporting

### 🎯 Week 14 — Incident Response, Detection & Analysis

Expanded investigation workflows through telemetry correlation, detection review, and incident analysis.

### 🛡️ [Week 15 — Endpoint Investigation Workflows](./Week15_Endpoint_Investigation_Workflows)

Focused on endpoint-centered investigation and process-level evidence.

### 🚨 [Week 16 — Incident Response Investigations](./Week16_Incident_Response_Investigations)

Focused on structured triage, evidence collection, correlation, scoping, and security decisions.

</details>

---

<details>
<summary><strong>☁️ Week 17 | Microsoft Defender XDR, Sentinel, Azure & Detection Engineering</strong></summary>

<br>

### ☁️ [Week 17 — Microsoft Security Platform Investigations](./Week17_Defender_XDR_Sentinel_Investigations)

Week 17 includes six major labs:

1. **Microsoft Security Environment Deployment, Defender for Endpoint Onboarding, Sentinel Integration & EDR Investigation**
2. **Microsoft Defender XDR Fundamentals, Incident Investigation & Enterprise Security Operations**
3. **Microsoft Sentinel SIEM, Azure Activity Telemetry, KQL Threat Hunting & Detection Engineering Fundamentals**
4. **Microsoft Entra Failed Login Investigation, KQL Threat Hunting, Sentinel Detection Engineering & Defender Incident Response**
5. **Suspicious PowerShell Endpoint Investigation, Advanced Hunting, Telemetry Correlation & Endpoint Response**
6. **Azure Cloud Security Operations: Identity, RBAC, Azure Activity Monitoring, KQL Threat Hunting, Sentinel Detection Engineering, Least-Privilege Remediation & Security Automation**

This section brings together:

- ☁️ Cloud security
- 🛡️ Endpoint security
- 👤 Identity
- 🔐 RBAC
- 📊 SIEM
- 🚨 EDR
- 🔎 Threat hunting
- 🎯 Detection engineering
- 🧠 Investigation
- ⚙️ Security automation
- ✅ Least privilege

</details>

---

<details>
<summary><strong>🧰 Weeks 18–21 | Troubleshooting, Response, Automation & Linux Security</strong></summary>

<br>

### 🧰 [Week 18 — IT & Network Troubleshooting](./Week18_IT_Network_Troubleshooting)

Focuses on troubleshooting the underlying systems security depends on.

Areas include:

- Networking
- DNS
- Routing
- Services
- Authentication
- Firewall configuration
- Operating system behavior
- Application behavior

### 🚨 Week 19 — Incident Response Triage & Investigation

Focused on:

- Alert triage
- Scope
- Evidence correlation
- Investigation
- Response decisions

### ⚙️ [Week 20 — Python & Security Automation](./Week20_Python_%26_Security_Automation)

Focused on:

- Python
- File handling
- CSV
- JSON
- Regex
- IOC processing
- API interaction
- Logging
- Error handling
- Structured reporting

### 🐧 [Week 21 — Linux Administration & Scripting](./Week21_Linux_Administration_%26_Scripting)

Focused on:

- Linux administration
- Users and groups
- Permissions
- SSH
- Firewalling
- Services
- Logging
- Auditing
- Hardening
- Bash
- Security validation

</details>

---

<details>
<summary><strong>📋 Technical-to-GRC | Risk, Controls & Security Assurance</strong></summary>

<br>

### 📋 [Technical-to-GRC Risk Bridge Projects](./Technical_To_GRC_Risk_Bridge_Projects)

Connects technical cybersecurity work to:

- NIST SP 800-171 Rev. 2
- CMMC Level 2
- Security control implementation
- Evidence validation
- Risk assessment
- Security gaps
- Remediation
- POA&M-style tracking
- Control re-validation

</details>

---

# 🧪 Investigation Methodology

Across the portfolio, I use an evidence-driven investigation process.

```text
┌──────────────────────────────────────┐
│  1. Understand the Environment       │
└──────────────────┬───────────────────┘
                   ↓
┌──────────────────────────────────────┐
│  2. Define the Question / Hypothesis │
└──────────────────┬───────────────────┘
                   ↓
┌──────────────────────────────────────┐
│  3. Identify Relevant Telemetry      │
└──────────────────┬───────────────────┘
                   ↓
┌──────────────────────────────────────┐
│  4. Review Raw Evidence              │
└──────────────────┬───────────────────┘
                   ↓
┌──────────────────────────────────────┐
│  5. Filter & Correlate Activity      │
└──────────────────┬───────────────────┘
                   ↓
┌──────────────────────────────────────┐
│  6. Determine Scope                  │
└──────────────────┬───────────────────┘
                   ↓
┌──────────────────────────────────────┐
│  7. Add Technical Context            │
└──────────────────┬───────────────────┘
                   ↓
┌──────────────────────────────────────┐
│  8. Compare Expected vs. Observed    │
└──────────────────┬───────────────────┘
                   ↓
┌──────────────────────────────────────┐
│  9. Classify the Finding             │
└──────────────────┬───────────────────┘
                   ↓
┌──────────────────────────────────────┐
│ 10. Respond / Recommend Action       │
└──────────────────┬───────────────────┘
                   ↓
┌──────────────────────────────────────┐
│ 11. Validate the Result              │
└──────────────────┬───────────────────┘
                   ↓
┌──────────────────────────────────────┐
│ 12. Document Evidence & Limitations  │
└──────────────────────────────────────┘
```

This process helps keep conclusions tied to evidence instead of assumptions.

---

# 🔎 Evidence & Documentation Standard

A major goal of this portfolio is to make the work **verifiable**.

Depending on the project, evidence may include:

| Evidence Type | Examples |
|---|---|
| 📸 **Visual Evidence** | Screenshots, dashboards, configuration views |
| 📑 **Logs** | Windows Security Events, SIEM events, endpoint telemetry |
| 🌐 **Network Evidence** | Packet captures, firewall logs, IDS alerts |
| 💻 **Endpoint Evidence** | Processes, command lines, file activity, timelines |
| 🔎 **Queries** | KQL, SPL, PowerShell queries |
| 🎯 **Detection Evidence** | Analytics rules, alert logic, thresholds, ATT&CK mappings |
| ⚙️ **Automation** | Python, PowerShell, Bash, APIs |
| ☁️ **Cloud Evidence** | Azure Activity, Entra sign-ins, RBAC changes |
| 📋 **Risk Evidence** | Control assessments, gaps, remediation records |
| 📝 **Documentation** | Investigation reports, conclusions, limitations |

I also try to clearly distinguish between four levels of certainty:

### ✅ Observed

Directly supported by collected evidence.

### 🧠 Inferred

Reasonably supported by several pieces of evidence but not directly proven.

### 🧪 Expected / Test Activity

Behavior intentionally generated in the controlled lab environment.

### ⚠️ Not Verified

Something that could not be fully confirmed with the available telemetry, permissions, licensing, or environment.

> **Cybersecurity analysis should not present assumptions as facts.**

---

# 🛠️ Technical Toolkit

## 📊 SIEM, Security Monitoring & EDR

`Microsoft Sentinel` · `Microsoft Defender XDR` · `Microsoft Defender for Endpoint` · `Microsoft Defender Antivirus` · `Splunk Enterprise` · `Splunk Enterprise Security` · `Elastic Security` · `Elastic Defend` · `Suricata` · `Windows Event Viewer`

## ☁️ Cloud & Identity

`Microsoft Azure` · `Microsoft Entra ID` · `Azure RBAC` · `Azure Activity Logs` · `Log Analytics` · `Active Directory Domain Services` · `Windows Server` · `Group Policy` · `Windows DNS`

## 🌐 Networking & Infrastructure

`pfSense` · `Cisco Packet Tracer` · `Wireshark` · `tcpdump` · `TCP/IP` · `DNS` · `DHCP` · `NAT` · `VLANs` · `VPNs` · `Firewalls` · `IDS/IPS`

## 💻 Operating Systems

`Windows 11` · `Windows 11 Enterprise` · `Windows Server 2022` · `Ubuntu Linux` · `Kali Linux`

## ⚙️ Querying, Scripting & Automation

`KQL` · `SPL` · `PowerShell` · `Python` · `Bash`

## 🧭 Frameworks & Security Methodologies

`MITRE ATT&CK` · `NIST SP 800-171 Rev. 2` · `CMMC Level 2` · `Least Privilege` · `Defense in Depth` · `Incident Response` · `Risk Assessment` · `Security Control Validation` · `POA&M-style Remediation Tracking`

## 📦 Virtualization

`Oracle VirtualBox`

---

# 🧠 Security Capabilities Demonstrated

## 🌐 Network Security

Projects demonstrate understanding of how systems communicate and how defensive controls influence that communication.

This includes:

**TCP/IP · subnetting · VLANs · DNS · DHCP · routing · firewalling · NAT · IDS/IPS · VPNs · packet analysis**

---

## 🛡️ Endpoint Security

Endpoint investigations focus on understanding system behavior through operating system and EDR telemetry.

This includes:

**process execution · parent/child relationships · command lines · files · authentication · device timelines · Defender telemetry · Elastic endpoint telemetry**

---

## 👤 Identity & Access Security

Projects explore how users authenticate, receive authorization, and gain privileges.

This includes:

**Active Directory · Group Policy · Microsoft Entra ID · Azure RBAC · authentication monitoring · failed logins · privileged access · least privilege**

---

## 🚨 Incident Response

Investigation projects focus on answering:

1. **What happened?**
2. **Which systems, users, or processes were involved?**
3. **When did it happen?**
4. **What evidence supports the conclusion?**
5. **What is the scope?**
6. **Is the activity malicious, suspicious, benign, or expected?**
7. **What action should be taken?**
8. **How can remediation be validated?**

---

## 🎯 Detection Engineering

Detection-focused work moves beyond consuming alerts into understanding how detections are created and tested.

This includes:

**detection logic · KQL · SPL · analytics rules · thresholds · lookback periods · ATT&CK mapping · validation · false-positive considerations**

---

## ☁️ Cloud Security

Cloud projects focus on identity, permissions, activity, detection, and least privilege.

This includes:

**Azure · Entra ID · Azure RBAC · Azure Activity · Sign-in Logs · Log Analytics · Sentinel · KQL · cloud detection engineering**

---

## ⚙️ Security Automation

Automation projects focus on making repetitive work more consistent and scalable.

This includes:

**Python · PowerShell · Bash · APIs · JSON · CSV · regex · IOC processing · logging · structured reporting**

---

## 🏗️ Security Engineering

Many projects involve building, configuring, troubleshooting, testing, and validating defensive technologies.

This includes:

**firewalls · IDS/IPS · VPNs · endpoint protection · EDR · SIEM · identity · RBAC · Linux hardening · Windows security · logging · detection rules**

---

## 📋 Technical Risk & Control Validation

The Technical-to-GRC work connects infrastructure and security controls to evidence and risk.

```text
Control Requirement
       ↓
Technical Implementation
       ↓
Evidence
       ↓
Assessment
       ↓
Security Gap
       ↓
Risk
       ↓
Remediation
       ↓
Validation
```

---

# 📊 From Alert to Evidence

An alert is only the beginning of an investigation.

```text
🚨 Alert
   ↓
📑 Raw Event
   ↓
🖥️ Host / 👤 User / ⚙️ Process / 🌐 Network Context
   ↓
🔗 Related Events
   ↓
🕒 Timeline
   ↓
🔭 Scope
   ↓
🧩 Evidence Correlation
   ↓
🏷️ Classification
   ↓
🛠️ Response
   ↓
✅ Validation
```

The same principle applies to security controls:

```text
📋 Requirement
   ↓
🛡️ Technical Control
   ↓
⚙️ Configuration
   ↓
📎 Evidence
   ↓
🔎 Assessment
   ↓
⚠️ Gap
   ↓
📊 Risk
   ↓
🛠️ Remediation
   ↓
✅ Validation
```

In both cases, the objective is the same:

> **Make defensible decisions based on evidence.**

---

# 🧩 Troubleshooting as a Security Skill

Security work depends heavily on understanding the systems underneath the security tools.

Unexpected behavior can result from:

- ⚙️ Unsupported software
- 🌐 Incorrect network configuration
- 📉 Missing telemetry
- 🛑 Disabled services
- 🔐 Authentication problems
- 🔥 Firewall behavior
- 🌍 DNS issues
- 👤 Permission problems
- 💳 Licensing limitations
- 📑 Logging problems
- 🛡️ Misconfigured controls

This matters because not every technical failure represents malicious activity.

The Microsoft Defender for Endpoint onboarding problem in Week 17 is one example.

Instead of stopping at the failed deployment, I investigated the endpoint, identified the platform limitation, rebuilt the system using a supported operating system, validated MDE services and telemetry, and then continued the security investigation.

---

# 🛡️ Security Principles Used Throughout the Portfolio

### 🔎 Evidence Before Assumptions

Security conclusions should be supported by observable evidence.

### 🔐 Least Privilege

Users, administrators, and services should receive only the access required for their purpose.

### 🧱 Defense in Depth

Security should not depend on a single defensive control.

### ✅ Validate the Control

A configuration existing does not automatically mean the control works.

### 🧠 Understand Normal Behavior

Expected system behavior should be understood before activity is classified as suspicious or malicious.

### ⚠️ Document Limitations

If something cannot be confirmed, the limitation should be stated clearly.

### 🔄 Remediate & Re-Validate

Security work should not end when a problem is discovered.

### ⚙️ Build Repeatable Processes

Queries, scripts, documentation, and workflows should make future security work more efficient and consistent.

---

# 🎯 What This Portfolio Demonstrates

The work in this repository increasingly requires me to:

- 🏗️ Build technical environments
- 🧩 Troubleshoot deployment and configuration problems
- 🌐 Understand network and protocol behavior
- 🔐 Configure access and identity controls
- 🛡️ Implement defensive security technologies
- 🧪 Generate controlled security activity
- 📑 Identify useful telemetry
- 🔎 Query and analyze security data
- 🚨 Investigate alerts
- 🔗 Correlate evidence across data sources
- 🖥️ Analyze endpoint behavior
- 👤 Investigate identity activity
- ☁️ Analyze cloud administrative activity
- 🎯 Develop detection logic
- ✅ Validate detections
- ⚙️ Automate repeatable security tasks
- 🐧 Harden Linux systems
- 🪟 Secure Windows environments
- 📋 Assess technical controls
- ⚠️ Identify security gaps
- 📊 Translate technical gaps into risk
- 🛠️ Recommend remediation
- 🔄 Validate security improvements
- 📝 Document conclusions clearly

The objective is continued progression from isolated technical exercises toward **complete, evidence-driven security workflows**.

---

# 🔬 Lab Environment

Projects throughout the repository use isolated or controlled environments containing combinations of:

- 📦 Oracle VirtualBox
- 🪟 Windows 11
- 🪟 Windows 11 Enterprise
- 🖥️ Windows Server 2022
- 🐧 Ubuntu Linux
- 🐉 Kali Linux
- 🔥 pfSense
- ☁️ Microsoft Azure
- 🛡️ Microsoft Defender
- 📊 Microsoft Sentinel
- 🔎 Splunk
- 🛡️ Elastic Security
- 🌐 Cisco Packet Tracer

Security testing, authentication testing, network scanning, EDR detection testing, malware simulations, and similar activity were performed within **authorized and controlled lab environments**.

No project in this repository is intended to demonstrate unauthorized access to real-world systems.

---

# ⚠️ Lab Scope & Documentation Notice

This repository represents hands-on technical cybersecurity lab work.

Some environments are intentionally smaller and more controlled than production enterprise environments.

Where relevant, project documentation identifies limitations involving:

- Licensing
- Available telemetry
- Lab architecture
- Data volume
- User population
- Endpoint count
- Cloud resources
- Testing scope

A successful result inside a lab should not automatically be interpreted as proof that the exact same behavior would occur in every production environment.

The purpose of the portfolio is to demonstrate the **technical process, investigation methodology, security reasoning, validation approach, troubleshooting ability, and quality of documentation behind the work**.

---

# 🚀 Recommended Starting Points

If you are reviewing this repository for the first time, I recommend beginning with these areas:

| Area | Project |
|---|---|
| ☁️ **Microsoft Security & Cloud** | [Week 17 — Defender XDR, Sentinel & Azure](./Week17_Defender_XDR_Sentinel_Investigations) |
| 📋 **Risk & Control Validation** | [Technical-to-GRC Risk Bridge](./Technical_To_GRC_Risk_Bridge_Projects) |
| 🐧 **Linux Security** | [Week 21 — Linux Administration & Scripting](./Week21_Linux_Administration_%26_Scripting) |
| ⚙️ **Security Automation** | [Week 20 — Python & Security Automation](./Week20_Python_%26_Security_Automation) |
| 🚨 **Incident Response** | [Week 13 — Windows Event Logs & IR](./Week13_Windows_EventLogs_IncidentResponse) |
| 🛡️ **Endpoint Investigation** | [Week 15 — Endpoint Investigation Workflows](./Week15_Endpoint_Investigation_Workflows) |
| 🔥 **Network Defense** | [Week 5 — pfSense](./Week5_pfSense) |
| 📊 **SIEM** | [Week 9 — Splunk SIEM](./Week9_Splunk_SIEM) |

---

# 📚 Repository Philosophy

This portfolio is designed to provide more than screenshots or a list of tools.

For each project, I try to answer:

```text
🏗️ What did I build?

🎯 Why did I build it?

⚙️ How does it work?

📎 What evidence did I collect?

🔎 What does the evidence actually prove?

⚠️ What does the evidence NOT prove?

🧠 What did I conclude?

🛠️ What action did I take or recommend?

✅ How did I validate the final result?

📚 What did I learn?
```

That approach makes each project both a technical learning record and evidence of practical cybersecurity work.

---

# 👤 About This Portfolio

This repository is maintained by **Fitzgerald Afari-Minta**.

🐙 **GitHub:** [@iamfitz7](https://github.com/iamfitz7)

I continue to expand this portfolio through hands-on work involving **infrastructure security, endpoint security, cloud security, identity, detection engineering, incident response, automation, Linux administration, and technical risk/control validation**.

---

# 🙏 Thank You for Reviewing My Work

Thank you for taking the time to explore this portfolio.

Each project represents continued hands-on practice across:

**building → troubleshooting → investigating → validating → documenting → improving**

For the strongest representation of my current technical work, I recommend reviewing the **newer projects alongside the foundational labs**, since the repository is intentionally structured to show progression over time.

---

### 🔐 Build it. 🔎 Investigate it. 📎 Prove it. 🛠️ Improve it.
