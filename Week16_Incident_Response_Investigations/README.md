# ☁️ Week 16 — Incident Response Investigations

This section documents realistic incident response investigations involving **data exfiltration**, **Layer 7 DDoS activity**, **insider threat behavior**, **privileged account abuse**, **credential dumping**, **LSASS memory access**, and evidence correlation across multiple security data sources.

The goal of these projects is to demonstrate how SOC analysts, incident responders, threat hunters, and DFIR teams investigate suspicious activity by validating alerts, determining scope, assessing impact, and making response decisions based on evidence rather than assumptions.

---

## 📁 Labs Included

| Lab | Project |
|---|---|
| Lab 01 | Data Exfiltration Investigation Through Cloud Storage |
| Lab 02 | Layer 7 HTTP Flood / DDoS Investigation |
| Lab 03 | Insider Threat, Privileged Account Abuse, & Data Exfiltration Investigation |
| Lab 04 | Credential Dumping & LSASS Memory Theft Investigation |

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

## 🚨 Why This Activity Was Concerning

No single event proved malicious activity by itself.

However, the sequence created a stronger concern:

```text
Privileged Logon
        ↓
Administrative Privileges Assigned
        ↓
Encoded PowerShell Execution
        ↓
Sensitive File Access
        ↓
Rclone Activity
        ↓
Potential Data Transfer
```

---

## 🛠️ Skills Demonstrated

- Insider threat investigation
- Privileged account monitoring
- Windows Event Log analysis
- PowerShell abuse analysis
- Rclone investigation
- Sensitive file access review
- EDR correlation
- Data exfiltration validation
- Containment decision-making

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

## 🎯 Investigation Goals

### Alert Validation

- Did the alert accurately represent suspicious LSASS activity?
- Was LSASS actually targeted?
- Did the detection rule provide useful context?

### Process Investigation

- Which process targeted LSASS?
- What parent process launched the suspicious activity?
- What command-line arguments were used?
- What account was associated with the activity?

### Credential Dumping Validation

- Was Procdump used?
- Was `lsass.exe` referenced directly?
- Was a `.dmp` file created?
- Where was the dump file written?

### External Upload Investigation

- Was the dump file moved or uploaded?
- Was `curl.exe` involved?
- Was there communication to an external storage destination?

### Endpoint Correlation

- Did Elastic or CrowdStrike-style telemetry confirm the activity?
- Did process tree evidence support the SIEM findings?
- Were tools such as Rundll32, Curl, or Procdump observed together?

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

## 🛠️ Tools & Data Sources Used

| Tool / Data Source | Purpose |
|---|---|
| Splunk | SIEM correlation and event review |
| Splunk Detection Rule Logic | Understanding why the alert triggered |
| Elastic Security | LSASS dump alert validation and endpoint review |
| Elastic Discover | Searching dump file and command-line evidence |
| CrowdStrike-style Telemetry | Process, command-line, hash, and account correlation |
| Process Analyzer | Parent-child process relationship review |
| Windows Endpoint Telemetry | Process execution and dump file evidence |
| Command-Line Analysis | Validating Procdump and Curl behavior |
| Service Account Review | Determining account risk and possible impact |

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

The first question was:

> Did a process actually target LSASS and create a memory dump?

---

### 2️⃣ Detection Rule Logic Review

The detection rule was reviewed to understand why the alert fired.

The rule focused on indicators such as:

- LSASS process access
- Dump file creation
- Memory dumping behavior
- Suspicious process execution
- Known credential dumping tool patterns

This helped define what evidence needed to be validated next.

---

### 3️⃣ Splunk LSASS, Procdump, and Curl Correlation

Splunk evidence connected several important elements:

```text
procdump.exe
lsass.exe
.dmp file
curl.exe
```

This was a major turning point because the evidence suggested more than generic suspicious activity.

The presence of Procdump alone may not confirm credential theft. However, Procdump used in relation to LSASS and dump file creation is highly suspicious.

---

### 4️⃣ Process and Command-Line Correlation

CrowdStrike-style process evidence showed activity involving:

- `procdump`
- `rundll32`
- `curl`
- suspicious command-line arguments
- hashes
- service account activity

The investigation moved from identifying tool names to understanding how the tools were used together.

The key question became:

> What did each tool do in the sequence?

---

### 5️⃣ Elastic LSASS Memory Dump Alert Validation

Elastic generated an alert showing:

```text
LSASS Memory Dump Creation
```

This validated that the suspicious activity was not isolated to one telemetry source.

Elastic supported the same investigation direction observed in Splunk:

- LSASS was targeted
- Memory dump behavior occurred
- Endpoint telemetry supported the alert

---

### 6️⃣ Process Tree Analysis

Process analyzer evidence showed a suspicious process chain:

```text
cmd.exe
      ↓
procdump64.exe
```

This was important because process lineage helps explain execution.

A suspicious binary becomes more meaningful when the process tree shows:

- how it launched
- what parent process created it
- what arguments were used
- whether the execution context was expected

---

### 7️⃣ Procdump LSASS Dump Argument Review

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

The key issue was not whether Procdump existed on the system. The issue was what Procdump was used to dump.

---

### 8️⃣ Curl Upload and Dump File Correlation

Additional evidence showed:

```text
curl.exe
```

executing shortly after LSASS dump activity.

The command-line evidence connected Curl activity to external upload behavior.

This changed the severity of the investigation because the concern was no longer only local credential dumping.

The concern became:

```text
Credential Dumping
        +
Possible Credential Exfiltration
```

---

### 9️⃣ External Upload Evidence

Curl activity showed upload behavior to an external suspicious cloud destination.

This was one of the most important findings because credential dumping becomes significantly more dangerous when dumped credential material leaves the environment.

---

### 🔟 Elastic Discover Dump File Search

Elastic Discover was used to search for:

```text
.dmp
```

activity and command-line evidence.

This helped validate dump file creation and provided another source of endpoint-level evidence.

Searching for dump file patterns is useful because attackers often stage credential material before compressing, moving, or uploading it.

---

### 1️⃣1️⃣ Service Account Suspicious Tool Correlation

The investigation identified service account activity associated with suspicious tools and behaviors.

Observed account context included:

```text
svc_adm@corp.local
```

Additional related account context included:

```text
corp\admin
```

Service account involvement increased risk because service accounts often have persistent access, elevated privileges, or access across multiple systems.

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
- Elastic Discover evidence for `.dmp` and command-line activity
- Service account activity tied to suspicious tooling

---

## 🚨 Why This Activity Was High Risk

This activity was high risk because the evidence showed a suspicious chain:

```text
LSASS Alert
      ↓
Procdump Execution
      ↓
LSASS Targeted
      ↓
Dump File Created
      ↓
Curl Upload Activity
      ↓
External Destination
      ↓
EDR Validation
```

Credential dumping is dangerous because it can expose authentication material that attackers may use for:

- lateral movement
- privilege escalation
- account takeover
- domain compromise
- persistence
- additional data theft

The presence of external upload behavior made the investigation more serious because it suggested possible credential exfiltration, not only local dumping.

---

## 🧠 Investigation Thinking

One of the most important lessons from this investigation was that legitimate tools can still be abused.

Tools observed included:

```text
Procdump
Rundll32
Curl
```

None of these are automatically malicious by name alone.

The stronger investigation questions were:

- What launched the tool?
- What arguments were used?
- What process was targeted?
- Was a file created?
- Where was the file written?
- Was the file uploaded?
- What account was involved?
- Did another telemetry source confirm the behavior?

The investigation became stronger as multiple independent data points supported the same conclusion.

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

## 🛠️ Recommended Response Actions

Based on the available evidence, recommended actions include:

- Isolate `SRV-DC01` from the network
- Preserve logs and endpoint evidence
- Preserve the timeline before containment changes system state
- Disable or reset the affected service account
- Review recent logons from involved accounts
- Rotate privileged credentials
- Review Tier-0 and administrative account exposure
- Block the external upload destination
- Hunt for similar Procdump, Rundll32, and Curl behavior across other hosts
- Search for `.dmp` files and LSASS access patterns across the environment
- Review lateral movement indicators
- Validate whether the dump file was successfully exfiltrated
- Document the complete evidence chain for escalation

---

## 📸 Screenshot Evidence

| Screenshot | Description |
|---|---|
| `Week16_Lab4_01_LSASS_Credential_Dumping_Alert.png` | Initial LSASS credential dumping alert on `SRV-DC01` |
| `Week16_Lab4_02_Detection_Rule_Logic_And_Description.png` | Detection rule logic explaining LSASS dumping indicators |
| `Week16_Lab4_03_Splunk_LSASS_Procdump_Curl_Evidence.png` | Splunk correlation showing `procdump.exe`, `lsass.exe`, dump file activity, and `curl.exe` |
| `Week16_Lab4_04_CrowdStrike_Process_And_Commandline_Correlation.png` | CrowdStrike-style process evidence with Procdump, Rundll32, Curl, command lines, hashes, and suspicious activity |
| `Week16_Lab4_05_Elastic_LSASS_Memory_Dump_Alert.png` | Elastic alert showing LSASS memory dump creation |
| `Week16_Lab4_06_Process_Analyzer_Procdump_Execution.png` | Process analyzer evidence showing `cmd.exe` leading to `procdump64.exe` |
| `Week16_Lab4_07_Process_Tree_System_And_Smss_Context.png` | Wider process tree context around system processes |
| `Week16_Lab4_08_Procdump64_LSASS_Dump_Arguments.png` | `procdump64.exe` arguments targeting `lsass.exe` and creating a dump file |
| `Week16_Lab4_09_Curl_Upload_And_Procdump_Dump_Correlation.png` | Curl upload activity correlated with Procdump dump creation |
| `Week16_Lab4_10_Curl_External_Upload_Evidence.png` | Curl upload to external suspicious cloud destination |
| `Week16_Lab4_11_Elastic_Discover_LSASS_Dump_Search.png` | Elastic Discover search for `.dmp` and command-line evidence |
| `Week16_Lab4_12_Service_Account_Suspicious_Tool_Correlation.png` | Service account activity tied to Curl, Rundll32, Procdump, LSASS dump behavior, and hashes |

---

## 📁 Supporting Files

```text
Cybersecurity-Technical-Analysis-Credential-Dumping-LSASS-Investigation.md
Security-Operations-Case-File-LSASS-Credential-Dumping-Investigation.md
```

---

## ✅ Case Determination

| Field | Value |
|---|---|
| Case Status | True Positive |
| Risk Level | High |
| Primary Concern | LSASS credential dumping |
| Secondary Concern | External upload / possible credential exfiltration |
| Affected Host | `SRV-DC01` |
| Account Context | `svc_adm@corp.local`, `corp\admin` |
| Recommended Action | Escalate, contain, preserve evidence, rotate credentials |

---

## 📚 Learning Outcomes

Through this investigation I strengthened my ability to:

- Validate credential dumping alerts
- Investigate LSASS memory access
- Analyze Procdump abuse
- Review suspicious command-line arguments
- Correlate SIEM and EDR telemetry
- Identify dump file creation
- Investigate Curl-based upload behavior
- Review service account risk
- Build an evidence chain from multiple data sources
- Make evidence-based containment decisions
- Document credential theft investigations professionally

---

## 🧾 Professional Summary

This project focused on determining whether suspicious LSASS-related activity represented credential dumping and possible credential exfiltration. By reviewing the initial alert, detection logic, Splunk events, CrowdStrike-style telemetry, Elastic alerts, process analyzer evidence, dump file creation, and Curl upload behavior, I was able to build a strong evidence chain.

The strongest findings were the connection between Procdump targeting LSASS, dump file creation, and Curl uploading the file to an external destination. This investigation reinforced that legitimate tools such as Procdump, Rundll32, and Curl can still be abused when used in suspicious sequences.

This lab strengthened my understanding of credential dumping investigations, LSASS security, Windows authentication risk, service account exposure, SIEM/EDR correlation, and containment decision-making.

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
- Procdump abuse analysis
- Curl upload investigation
- PowerShell investigation
- Rclone activity review
- EDR process analysis
- Authentication review
- Evidence correlation
- Containment reasoning
- Security documentation

The biggest lesson from this week is:

> An alert is not the answer.  
> It is the starting point of the investigation.

Strong investigations are built by validating evidence, connecting findings across multiple data sources, understanding what actually matters, and making reasonable decisions based on the information available.
````


Strong investigations are built by validating evidence, connecting findings across multiple data sources, understanding what actually matters, and making reasonable decisions based on the information available.

# ⚠️ Disclaimer

These projects was completed in a controlled educational and cybersecurity training environment. All logs, alerts, indicators, and investigation artifacts were used strictly for learning and professional development purposes.

---

# 👨‍💻 Author

**Fitzgerald Afari-Minta**

Cybersecurity | SOC Operations | Incident Response | Threat Investigation
