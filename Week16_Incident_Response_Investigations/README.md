# ☁️ Week 16 — Incident Response Investigations

This section documents realistic incident response investigations involving **data exfiltration**, **Layer 7 DDoS activity**, **insider threat behavior**, **privileged account abuse**, **credential dumping**, **LSASS memory access**, **command-and-control beaconing**, **threat intelligence enrichment**, and evidence correlation across multiple security data sources.

The goal of these projects is to demonstrate how SOC analysts, incident responders, threat hunters, and DFIR teams investigate suspicious activity by validating alerts, determining scope, assessing impact, and making response decisions based on evidence rather than assumptions.

Across these investigations, the focus is not simply on identifying alerts. The focus is on understanding:

- What happened
- How it happened
- Which users, systems, or accounts were involved
- What evidence supports the concern
- Whether the activity was legitimate, suspicious, or malicious
- What additional context is needed
- What response action makes sense
- What limitations exist in the available telemetry

These labs are designed to reflect the type of thinking used during real-world Security Operations Center and Incident Response investigations.

---

## 📁 Labs Included

| Lab | Project |
|---|---|
| Lab 01 | Data Exfiltration Investigation Through Cloud Storage |
| Lab 02 | Layer 7 HTTP Flood / DDoS Investigation |
| Lab 03 | Insider Threat, Privileged Account Abuse, & Data Exfiltration Investigation |
| Lab 04 | Credential Dumping & LSASS Memory Theft Investigation |
| Lab 05 | Command-and-Control Beaconing & Threat Intelligence Investigation |

---

# ☁️ Lab 01 — Data Exfiltration Investigation Through Cloud Storage

## 🛡️ Incident Overview

This lab investigates a potential data exfiltration incident involving unusually large outbound transfers to a cloud storage provider.

The investigation focused on determining:

- Whether outbound data transfer activity actually occurred
- Which user and host were involved
- What destination was contacted
- Whether the transfer was legitimate or suspicious
- What endpoint activity supported the alert
- Whether containment or escalation was required

---

## 🔄 Investigation Workflow

```text
Splunk Alert
      ↓
Proxy Log Analysis
      ↓
Threat Intelligence Validation
      ↓
EDR Investigation
      ↓
Host Artifact Review
      ↓
Containment Decision
```

---

## 🛠️ Skills Demonstrated

- Data exfiltration investigation
- Splunk alert validation
- Proxy log analysis
- Threat intelligence review
- Endpoint telemetry correlation
- Host artifact review
- Incident response decision-making
- Evidence-based escalation

---

## 🧾 Professional Summary

This project strengthened my ability to investigate potential data exfiltration by validating alerts, reviewing outbound activity, analyzing endpoint telemetry, and correlating evidence before making a response decision.

---

# 🌐 Lab 02 — Layer 7 HTTP Flood / DDoS Investigation

## 🛡️ Incident Overview

This lab investigates potential **Layer 7 HTTP flood activity** identified through abnormal web traffic behavior.

The investigation began after monitoring detected a significant increase in HTTP requests targeting a web application environment.

The goal was to determine whether the activity represented:

- Legitimate traffic
- Misconfiguration
- Automated activity
- Layer 7 DDoS behavior

---

## 🔄 Investigation Workflow

```text
Security Finding
        ↓
Traffic Baseline Analysis
        ↓
Requests-Per-Minute Review
        ↓
Source Attribution
        ↓
Distributed Traffic Analysis
        ↓
Endpoint Target Analysis
        ↓
HTTP Error Correlation
        ↓
User-Agent Review
        ↓
Threat Validation
        ↓
Investigation Conclusion
```

---

## 📊 Key Findings

- Significant increase in requests-per-minute
- Multiple source IPs generated elevated traffic
- Requests targeted:
  - `/`
  - `/login`
  - `/api/v1/data`
- Increased HTTP 5xx server errors
- User-Agent indicators included:
  - `curl`
  - `python-requests`
- Evidence supported Layer 7 HTTP flood activity

---

## 🛠️ Skills Demonstrated

- Traffic anomaly detection
- Source IP attribution
- DDoS investigation workflow
- User-Agent analysis
- HTTP error correlation
- Threat validation
- Incident response reasoning

---

## 🧾 Professional Summary

This lab strengthened my understanding of how DDoS investigations are validated using evidence. The main lesson was that traffic volume alone does not prove an attack. A stronger conclusion comes from correlating traffic spikes, distributed sources, targeted endpoints, server errors, and automation indicators.

---

# 🔐 Lab 03 — Insider Threat, Privileged Account Abuse, & Data Exfiltration Investigation

## 🛡️ Incident Overview

This investigation focused on determining whether unusual privileged account activity represented legitimate administrative behavior or potential insider misuse resulting in data theft.

The investigation began after a security alert identified abnormal activity associated with:

```text
corp\admin
```

Initial evidence suggested:

- Multiple system logons
- Elevated privileges
- Encoded PowerShell execution
- Sensitive file access
- Rclone execution
- Possible cloud-based data transfer

---

## 🔄 Investigation Workflow

```text
Security Alert
        ↓
Detection Validation
        ↓
Authentication Review
        ↓
Privilege Assignment Analysis
        ↓
Process Creation Investigation
        ↓
PowerShell Abuse Analysis
        ↓
Sensitive File Access Review
        ↓
Rclone Investigation
        ↓
EDR Correlation
        ↓
Exfiltration Validation
        ↓
Containment Decision
```

---

## 🔍 Key Evidence Reviewed

| Event ID | Description |
|---|---|
| 4624 | Successful Logon |
| 4672 | Special Privileges Assigned |
| 4688 | Process Creation |
| 4663 | Object Access |

Observed processes included:

```text
cmd.exe
powershell.exe
rclone.exe
```

PowerShell execution included suspicious flags:

```text
-NoProfile
-WindowStyle Hidden
-EncodedCommand
```

Sensitive file involved:

```text
Q4_Salary.xlsx
```

Observed process chain:

```text
cmd.exe
      ↓
powershell.exe
      ↓
rclone.exe
```

---

## 📊 Key Findings

- Successful privileged logons occurred
- Administrative privileges were assigned
- Encoded PowerShell execution was observed
- Sensitive file access was identified
- Rclone activity suggested possible cloud transfer
- EDR telemetry supported the process activity
- Evidence supported possible data exfiltration

---

## 🧾 Professional Summary

This project strengthened my ability to investigate privileged account abuse and possible insider threat behavior. The strongest findings involved encoded PowerShell execution, sensitive file access, and Rclone activity associated with possible data transfer.

---

# 🔑 Lab 04 — Credential Dumping & LSASS Memory Theft Investigation

## 🛡️ Incident Overview

This investigation focused on validating suspicious activity associated with Windows credential dumping techniques targeting **LSASS**.

The investigation began after a security alert identified possible credential dumping activity on:

```text
SRV-DC01
```

LSASS is a critical Windows process because it handles sensitive authentication material. If an attacker accesses LSASS memory, they may be able to extract credential-related data such as password hashes, access tokens, and other authentication artifacts.

The objective was to determine whether the activity represented:

- Legitimate administrative or troubleshooting behavior
- Attempted credential dumping
- Successful LSASS memory dumping
- Credential material staging
- External upload or credential exfiltration
- A true positive requiring containment

---

## 🔄 Investigation Workflow

```text
LSASS Credential Dumping Alert
        ↓
Detection Rule Review
        ↓
Splunk Event Correlation
        ↓
Process and Command-Line Analysis
        ↓
Procdump Execution Validation
        ↓
LSASS Dump File Creation Review
        ↓
Curl Upload Investigation
        ↓
Elastic / EDR Alert Validation
        ↓
Service Account Review
        ↓
Containment and Credential Response Decision
```

---

## 🔍 Key Investigation Areas

### 1️⃣ Initial Credential Dumping Alert Review

The investigation began with an alert titled:

```text
Possible Credential Dumping via LSASS
```

The affected host was:

```text
SRV-DC01
```

Because LSASS memory can contain sensitive authentication material, this alert required immediate review.

---

### 2️⃣ Splunk LSASS, Procdump, and Curl Correlation

Splunk evidence connected several important elements:

```text
procdump.exe
lsass.exe
.dmp file
curl.exe
```

This was a major turning point because the evidence suggested more than generic suspicious activity.

---

### 3️⃣ Process and Command-Line Correlation

CrowdStrike-style process evidence showed activity involving:

- `procdump`
- `rundll32`
- `curl`
- suspicious command-line arguments
- hashes
- service account activity

The investigation moved from identifying tool names to understanding how the tools were used together.

---

### 4️⃣ Elastic LSASS Memory Dump Alert Validation

Elastic generated an alert showing:

```text
LSASS Memory Dump Creation
```

This validated that the suspicious activity was not isolated to one telemetry source.

---

### 5️⃣ Process Tree Analysis

Process analyzer evidence showed a suspicious process chain:

```text
cmd.exe
      ↓
procdump64.exe
```

This was important because process lineage helps explain execution.

---

### 6️⃣ Procdump LSASS Dump Argument Review

One of the strongest pieces of evidence showed:

```text
procdump64.exe
```

executing with arguments targeting:

```text
lsass.exe
```

and creating a memory dump file.

This behavior is consistent with credential dumping because LSASS contains sensitive authentication data.

---

### 7️⃣ Curl Upload and Dump File Correlation

Additional evidence showed:

```text
curl.exe
```

executing shortly after LSASS dump activity.

The command-line evidence connected Curl activity to external upload behavior.

The concern became:

```text
Credential Dumping
        +
Possible Credential Exfiltration
```

---

## 📊 Key Findings

The investigation identified:

- Initial LSASS credential dumping alert on `SRV-DC01`
- Detection logic consistent with LSASS memory dumping behavior
- Splunk events connecting:
  - `procdump.exe`
  - `lsass.exe`
  - dump file activity
  - `curl.exe`
- CrowdStrike-style telemetry showing:
  - Procdump activity
  - Rundll32 activity
  - Curl activity
  - command-line evidence
  - hash evidence
  - suspicious account context
- Elastic alert confirming:
  - `LSASS Memory Dump Creation`
- Process analyzer evidence showing:

```text
cmd.exe → procdump64.exe
```

- Procdump arguments targeting:

```text
lsass.exe
```

- Dump file creation evidence
- Curl upload activity to an external destination
- Service account activity tied to suspicious tooling

---

## 🧭 MITRE ATT&CK Concepts Observed

| Technique | Description |
|---|---|
| T1003.001 | OS Credential Dumping: LSASS Memory |
| T1059 | Command and Scripting Interpreter |
| T1105 | Ingress Tool Transfer |
| T1041 | Exfiltration Over C2 Channel |
| T1218.011 | Signed Binary Proxy Execution: Rundll32 |
| T1078 | Valid Accounts |
| T1567 | Exfiltration to Cloud Storage |
| T1552 | Unsecured Credentials |

---

## 🧾 Professional Summary

This project focused on determining whether suspicious LSASS-related activity represented credential dumping and possible credential exfiltration. By reviewing the initial alert, detection logic, Splunk events, CrowdStrike-style telemetry, Elastic alerts, process analyzer evidence, dump file creation, and Curl upload behavior, I was able to build a strong evidence chain.

The strongest findings were the connection between Procdump targeting LSASS, dump file creation, and Curl uploading the file to an external destination. This investigation reinforced that legitimate tools such as Procdump, Rundll32, and Curl can still be abused when used in suspicious sequences.

---

# 📡 Lab 05 — Command-and-Control Beaconing & Threat Intelligence Investigation

## 🛡️ Incident Overview

This investigation focused on validating suspected **command-and-control (C2) beaconing activity** based on repeated outbound network communications to external IP addresses.

The investigation began after a Splunk Enterprise Security notable event identified:

```text
Probable Malware Beaconing Outbound Traffic Identified
```

The objective was to determine whether the observed outbound activity represented:

- Normal business communication
- Repeated but benign external service traffic
- Suspicious beaconing behavior
- Malware communication with external infrastructure
- Activity requiring escalation and deeper endpoint review

C2 beaconing investigations are important because malware often communicates with attacker-controlled infrastructure at regular intervals. These communications may be used to receive commands, download additional payloads, maintain persistence, or signal that a compromised system is active.

---

## 🎯 Investigation Goals

### Alert Validation

- Did the alert represent actual repeated outbound communication?
- Were the connection counts unusually high?
- Did the traffic pattern support a beaconing hypothesis?

### External Destination Review

- Which external IP addresses were contacted?
- How frequently were the destinations contacted?
- What were the first seen and last seen timestamps?
- Were the same destinations contacted repeatedly?

### User and Host Attribution

- Which internal users communicated with the suspicious IPs?
- Which systems were involved?
- Were multiple users or departments affected?

### Threat Intelligence Validation

- Did the external IP addresses have suspicious reputation?
- Were there related files, malware associations, or historical detections?
- Did threat intelligence provide supporting context?

### Impact Assessment

- How many users, hosts, or departments were potentially affected?
- Was the activity isolated or widespread?
- Did the available evidence justify escalation?

---

## 🔄 Investigation Workflow

```text
C2 Beaconing Alert
        ↓
Beaconing Detection Query Review
        ↓
High-Frequency Connection Analysis
        ↓
Suspicious External IP Identification
        ↓
User Attribution
        ↓
Multi-User Communication Analysis
        ↓
Threat Intelligence IP Reputation Review
        ↓
File and Malware Correlation
        ↓
Blast Radius Assessment
        ↓
Investigation Conclusion
```

---

## 🛠️ Tools & Data Sources Used

| Tool / Data Source | Purpose |
|---|---|
| Splunk Enterprise Security | Notable event review and alert validation |
| Splunk Search | High-frequency outbound connection analysis |
| Network Logs | External destination and connection count review |
| User Attribution Data | Mapping suspicious traffic to internal users |
| Threat Intelligence Platform | IP reputation and malware association review |
| Community Intelligence | Historical context and external reputation validation |
| Incident Response Methodology | Impact assessment and response decision-making |

---

## 🔍 Key Investigation Areas

### 1️⃣ Initial C2 Beaconing Alert Review

The investigation began with the notable event:

```text
Probable Malware Beaconing Outbound Traffic Identified
```

This alert represented a potential sign of malware communicating outbound to external infrastructure.

However, the alert was not treated as confirmed compromise. It was treated as a hypothesis requiring validation.

The first investigative question was:

> Are internal systems repeatedly communicating with suspicious external destinations in a pattern consistent with beaconing?

---

### 2️⃣ Beaconing High-Frequency Connection Analysis

A Splunk search was used to identify outbound communications with:

- External destination IPs
- High connection counts
- Repeated communication patterns
- First seen timestamps
- Last seen timestamps

This helped determine whether the outbound connections were isolated events or recurring activity.

Repeated communication to the same destination is more meaningful than a single connection because beaconing often relies on periodic check-ins.

---

### 3️⃣ Suspicious External IP Connection Summary

The investigation reviewed top external IP addresses and summarized:

- Destination IP
- Number of connections
- First observed time
- Last observed time
- Communication frequency

This helped prioritize which external destinations required further investigation.

At this stage, the investigation focused on identifying the most suspicious infrastructure rather than reviewing every network event individually.

---

### 4️⃣ User Attribution for Suspicious IPs

After identifying suspicious external IPs, the investigation shifted to attribution.

The goal was to determine:

- Which internal users communicated with the IPs
- Which systems were involved
- Whether activity was isolated to one user or seen across multiple users

This step was important because an IP address by itself does not provide enough context.

Attribution helped transform the investigation from:

```text
Suspicious External IP
```

to:

```text
Suspicious External IP + Internal User + Internal Host
```

That context is much more useful for incident response.

---

### 5️⃣ Multi-User Beaconing Communication Analysis

Further analysis showed communication patterns involving:

- Multiple users
- Same destination IPs
- Repeated connection counts

This was important because repeated communication across multiple users or systems may indicate a broader issue.

The investigation focused on whether the activity represented:

- one compromised host
- multiple affected systems
- shared business application traffic
- possible malware beaconing across several endpoints

This helped support blast radius analysis.

---

### 6️⃣ Threat Intelligence IP Reputation Review

Threat intelligence research was performed against the suspicious external IP addresses.

The goal was to determine whether the infrastructure had:

- malicious reputation
- community reports
- historical detections
- known malware associations
- suspicious hosting relationships

Threat intelligence was used as supporting evidence, not as the only deciding factor.

A clean reputation score alone does not prove safety, and a suspicious reputation score should still be validated against internal telemetry.

---

### 7️⃣ Threat Intelligence File and Malware Correlation

One of the strongest parts of the investigation involved reviewing threat intelligence relationships.

The analysis included:

- connected files
- malware associations
- historical detections
- related indicators
- infrastructure relationships

This helped determine whether the suspicious IPs were associated with known malicious activity or malware-related infrastructure.

This stage increased the depth of the investigation because it moved beyond “IP reputation” and into relationship-based threat intelligence analysis.

---

### 8️⃣ Blast Radius User and Host Impact Assessment

The investigation then reviewed impact across:

- users
- hostnames
- departments
- external IP relationships

This helped determine whether the activity was isolated or potentially broader.

Blast radius assessment is important because the response decision changes depending on scope.

A single affected host may require endpoint containment and user review.

Multiple affected hosts or users may require wider hunting, network blocking, and incident escalation.

---

## 📊 Key Findings

The investigation identified:

- A Splunk ES notable event for probable malware beaconing
- Repeated outbound communications to suspicious external IP addresses
- High-frequency connection patterns
- First seen and last seen timestamps supporting recurring activity
- External IPs mapped back to internal users
- Multiple users communicating with the same destination IPs
- Threat intelligence context for suspicious infrastructure
- File and malware relationships connected to external indicators
- Blast radius evidence involving users, hosts, departments, and external destinations

---

## 🚨 Why This Activity Was Concerning

This activity was concerning because several findings aligned:

```text
Beaconing Alert
      ↓
Repeated Outbound Connections
      ↓
Suspicious External IPs
      ↓
Internal User Attribution
      ↓
Multi-User Communication Patterns
      ↓
Threat Intelligence Correlation
      ↓
Blast Radius Review
```

Repeated outbound communications alone do not prove malware.

However, repeated communications combined with suspicious infrastructure, user attribution, threat intelligence relationships, and impact assessment create a stronger case for further investigation.

---

## 🧠 Investigation Thinking

One of the biggest lessons from this lab was that C2 beaconing investigations require context.

A weaker investigation might stop at:

> “This IP looks suspicious.”

A stronger investigation asks:

- How often was the IP contacted?
- Which users contacted it?
- Which hosts were involved?
- Was the communication repeated?
- Was the same destination contacted by multiple users?
- Does threat intelligence show malware relationships?
- Is the behavior isolated or widespread?
- What additional endpoint evidence is needed?

This lab reinforced that an external IP address alone rarely tells the full story. The value comes from connecting network behavior, attribution, threat intelligence, and impact.

---

## ⚠️ Risks, Trade-Offs, and Limitations

One risk in beaconing investigations is overreacting to repeated outbound connections.

Many legitimate applications communicate regularly with external services, including:

- cloud platforms
- software update services
- authentication providers
- telemetry services
- business applications

Another risk is relying too heavily on threat intelligence reputation scores. Newly created attacker infrastructure may not have enough historical reputation data to appear malicious.

This investigation was based on available lab evidence. In a real environment, additional data sources would likely be reviewed, including:

- DNS logs
- firewall logs
- proxy logs
- EDR process telemetry
- packet captures
- endpoint timelines
- malware sandbox results

---

## 🧭 MITRE ATT&CK Concepts Observed

| Technique | Description |
|---|---|
| T1071 | Application Layer Protocol |
| T1102 | Web Service |
| T1571 | Non-Standard Port |
| T1095 | Non-Application Layer Protocol |
| T1041 | Exfiltration Over C2 Channel |
| T1059 | Command and Scripting Interpreter |
| T1583 | Acquire Infrastructure |
| T1595 | Active Scanning |

---

## 🛠️ Recommended Response Actions

Based on the evidence available, recommended next steps include:

- Continue monitoring affected users and hosts
- Review endpoint telemetry for suspicious processes
- Correlate with DNS and proxy logs
- Block confirmed malicious IPs where operationally safe
- Enrich indicators using multiple threat intelligence sources
- Review whether the same IPs appear across other environments
- Hunt for related file hashes and malware indicators
- Determine whether outbound traffic matches known business services
- Escalate if endpoint telemetry supports compromise
- Document all findings in a formal case file

---

## 📸 Screenshot Evidence

| Screenshot | Description |
|---|---|
| `Week16_Lab5_01_C2_Beaconing_Detection_Alert.png` | Initial Splunk notable event showing probable malware beaconing outbound traffic |
| `Week16_Lab5_02_Beaconing_High_Frequency_Connection_Analysis.png` | Splunk query identifying external destination IPs, high connection counts, and repeated outbound communications |
| `Week16_Lab5_03_Suspicious_External_IP_Connection_Summary.png` | Summary of suspicious external IPs, connection counts, first seen, and last seen values |
| `Week16_Lab5_04_User_Attribution_For_Suspicious_IPs.png` | Mapping of suspicious external IP addresses to internal users |
| `Week16_Lab5_05_Multi_User_Beaconing_Communication_Analysis.png` | Multi-user communication analysis showing shared destination IPs and connection counts |
| `Week16_Lab5_06_Threat_Intelligence_IP_Reputation_Review.png` | Threat intelligence reputation review for suspicious external IP infrastructure |
| `Week16_Lab5_07_Threat_Intelligence_File_And_Malware_Correlation.png` | Threat intelligence relationships showing connected files, detections, and malware associations |
| `Week16_Lab5_08_Blast_Radius_User_And_Host_Impact_Assessment.png` | Impact assessment showing departments, hostnames, users, and external IP relationships |

---

## 📸 Recommended LinkedIn Screenshots

The strongest screenshots for LinkedIn are:

1. `Week16_Lab5_01_C2_Beaconing_Detection_Alert.png`  
   Shows the initial C2 beaconing detection alert.

2. `Week16_Lab5_03_Suspicious_External_IP_Connection_Summary.png`  
   Shows the repeated outbound communication evidence.

3. `Week16_Lab5_07_Threat_Intelligence_File_And_Malware_Correlation.png`  
   Shows threat intelligence enrichment and malware relationship analysis.

These three screenshots tell the public-facing investigation story:

```text
Detection Alert
      ↓
Repeated External Connections
      ↓
Threat Intelligence Correlation
      ↓
C2 Beaconing Investigation
```

---

## 📁 Supporting Files

Recommended file names for this lab:

```text
Cybersecurity-Technical-Analysis-C2-Beaconing-And-Threat-Intelligence-Investigation.md
SOC-Case-Study-C2-Beaconing-And-Threat-Intelligence-Investigation.md
```

---

## ✅ Case Determination

| Field | Value |
|---|---|
| Case Type | Probable Malware Beaconing / C2 Investigation |
| Primary Concern | Repeated outbound communication to suspicious external IPs |
| Supporting Evidence | High-frequency connections, user attribution, threat intelligence relationships |
| Risk Level | Medium to High |
| Status | Requires continued investigation and endpoint validation |
| Recommended Action | Monitor, enrich, correlate with endpoint telemetry, escalate if compromise indicators are confirmed |

---

## 📚 Learning Outcomes

Through this investigation I strengthened my ability to:

- Validate suspected C2 beaconing alerts
- Analyze high-frequency outbound network connections
- Identify suspicious external IP destinations
- Attribute network activity to internal users and systems
- Use threat intelligence to enrich indicators
- Review file and malware relationships
- Assess user, host, and department impact
- Distinguish repeated business traffic from suspicious beaconing patterns
- Build evidence-based conclusions
- Document network-based investigations professionally

---

## 🧾 Professional Summary

This project focused on investigating suspected command-and-control beaconing activity through outbound network analysis and threat intelligence correlation. The investigation began with a Splunk notable event and expanded into high-frequency connection review, suspicious external IP analysis, user attribution, multi-user communication analysis, IP reputation review, malware relationship research, and blast radius assessment.

The strongest lesson from this project was that repeated outbound communication is not enough by itself to confirm compromise. A stronger investigation comes from connecting communication frequency, external destination context, internal attribution, threat intelligence relationships, and impact assessment.

This lab strengthened my understanding of C2 beaconing investigations, network traffic analysis, threat intelligence enrichment, user attribution, blast radius review, and incident response decision-making.

---

# 🧾 Week 16 Summary

Week 16 focused heavily on incident response investigations and evidence correlation.

Across these projects, I practiced:

- Alert validation
- Threat intelligence review
- Data exfiltration investigations
- Layer 7 DDoS investigations
- Insider threat investigations
- Privileged account abuse analysis
- Credential dumping investigations
- LSASS memory access review
- Command-and-control beaconing investigations
- Procdump abuse analysis
- Curl upload investigation
- PowerShell investigation
- Rclone activity review
- EDR process analysis
- Authentication review
- User attribution
- External IP reputation analysis
- Evidence correlation
- Blast radius assessment
- Containment reasoning
- Security documentation

The biggest lesson from this week is:

> An alert is not the answer.  
> It is the starting point of the investigation.

Strong investigations are built by validating evidence, connecting findings across multiple data sources, understanding what actually matters, and making reasonable decisions based on the information available.


# ⚠️ Disclaimer

These projects was completed in a controlled educational and cybersecurity training environment. All logs, alerts, indicators, and investigation artifacts were used strictly for learning and professional development purposes.

---

# 👨‍💻 Author

**Fitzgerald Afari-Minta**

Cybersecurity | SOC Operations | Incident Response | Threat Investigation
