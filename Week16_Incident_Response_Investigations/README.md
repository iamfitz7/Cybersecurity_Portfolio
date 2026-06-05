# ☁️ Week 16 — Incident Response Investigations

This section of the repository focuses on realistic incident response investigations involving data exfiltration, denial-of-service activity, insider threats, privileged account abuse, endpoint investigations, and evidence correlation across multiple security data sources.

The projects in this week are designed to simulate how SOC analysts, incident responders, threat hunters, and DFIR teams investigate suspicious activity, validate alerts, determine scope, assess impact, and make response decisions based on evidence rather than assumptions.

Throughout these investigations, the focus is not simply on identifying alerts. Instead, the emphasis is placed on understanding:

- What happened
- How it happened
- Who was involved
- What systems were affected
- Whether the activity was legitimate or malicious
- What evidence supports the conclusion
- What response actions make sense

These labs closely mirror the thought process used during real-world incident response investigations.

---

# 📁 Labs Included

| Lab | Project |
|------|----------|
| Lab 01 | Data Exfiltration Investigation Through Cloud Storage |
| Lab 02 | Layer 7 HTTP Flood / DDoS Investigation |
| Lab 03 | Insider Threat, Privileged Account Abuse, & Data Exfiltration Investigation |

---

# ☁️ Lab 01 — Data Exfiltration Investigation Through Cloud Storage

---

## 🛡️ Incident Overview

This project documents the investigation of a potential data exfiltration incident involving cloud storage services.

The investigation began after a security alert identified unusually large outbound data transfers to a cloud-based file sharing platform.

The objective was to determine:

- Whether sensitive data was actually leaving the environment
- Who was responsible for the activity
- What files were involved
- How the transfer occurred
- Whether the behavior was legitimate or suspicious
- What response actions were appropriate

The investigation followed a realistic SOC and Incident Response workflow by correlating evidence across multiple security tools and data sources.

---

## 🎯 Investigation Goals

The investigation focused on answering:

### Validation

- Is the alert legitimate?
- Is outbound traffic actually occurring?

### User Attribution

- Which user initiated the activity?
- Is the activity associated with a known employee?

### Threat Intelligence

- Is the destination known to be malicious?
- Are there indicators of compromise associated with the destination?

### Endpoint Investigation

- What processes were responsible?
- Were suspicious tools involved?

### Host Artifact Review

- What files were transferred?
- Were sensitive files involved?

### Incident Response

- Does the activity require containment?
- What actions should be taken next?

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
- Alert validation
- Proxy log analysis
- Threat intelligence review
- Endpoint investigation
- Host artifact review
- Incident response decision-making
- Evidence correlation
- Security documentation

---

## 🧾 Professional Summary

This project strengthened my understanding of how security teams investigate potential data exfiltration activity by validating alerts, reviewing outbound communications, analyzing endpoint telemetry, and examining host artifacts. The investigation reinforced the importance of correlating multiple evidence sources before making containment decisions.

---

# 🌐 Lab 02 — Layer 7 HTTP Flood / DDoS Investigation

---

## 🛡️ Incident Overview

This project documents a structured investigation into potential Layer 7 HTTP flood activity identified through abnormal web traffic behavior.

The investigation began after monitoring systems detected a significant increase in HTTP requests targeting a web application environment.

Rather than assuming a DDoS attack was occurring, the objective was to validate the activity through:

- Traffic analysis
- Source attribution
- Endpoint targeting review
- Application impact assessment
- User-Agent investigation
- Threat validation

The goal was determining whether the activity represented:

- Legitimate traffic
- Misconfiguration
- Automated activity
- Layer 7 DDoS behavior

---

## 🎯 Investigation Goals

The investigation focused on determining:

- Whether traffic exceeded normal baselines
- Whether traffic originated from one source or many
- What resources were being targeted
- Whether the application was impacted
- Whether traffic appeared automated
- Whether evidence supported a DDoS conclusion

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
  - curl
  - python-requests
- Evidence supported Layer 7 HTTP flood activity

---

## 🧠 Investigation Thinking

One major lesson from this project was learning that:

> A traffic spike alone does not prove a DDoS attack.

The investigation focused on validating:

- Source distribution
- Targeted resources
- Application impact
- Automation indicators

before reaching a conclusion.

---

## 🛠️ Skills Demonstrated

- Traffic anomaly detection
- Requests-per-minute analysis
- Source attribution
- DDoS investigation workflows
- User-Agent analysis
- Application impact assessment
- Threat validation
- Incident response reasoning

---

## 🧾 Professional Summary

This project strengthened my understanding of how security teams validate potential denial-of-service activity using evidence rather than assumptions. By reviewing traffic volume, source distribution, targeted resources, application impact, and automation indicators, I developed a stronger understanding of Layer 7 DDoS investigation workflows.

---

# 🔐 Lab 03 — Insider Threat, Privileged Account Abuse, & Data Exfiltration Investigation

---

## 🛡️ Incident Overview

This investigation focused on determining whether unusual privileged account activity represented legitimate administrative behavior or potential insider misuse resulting in data theft.

The investigation began after a security alert identified abnormal activity associated with the privileged account:

```text
corp\admin
```

Initial evidence suggested that the account was:

- Logging into multiple systems
- Receiving elevated privileges
- Executing encoded PowerShell commands
- Accessing sensitive files
- Launching Rclone
- Potentially transferring organizational data externally

The objective was not simply determining whether the account logged in successfully.

Instead, the investigation focused on understanding:

- What actions occurred after authentication
- Whether privileges were abused
- Whether sensitive files were accessed
- Whether data left the organization
- Whether containment was necessary

This project closely mirrors real insider threat and privileged account abuse investigations performed by SOC and Incident Response teams.

---

## 🎯 Investigation Goals

The investigation focused on answering several critical questions:

### Authentication Review

- Who logged in?
- Where did they log in from?
- Which systems were accessed?

### Privilege Validation

- Were elevated privileges assigned?
- What level of access existed?

### Process Investigation

- What processes executed?
- Were suspicious commands used?

### Sensitive File Access

- Which files were accessed?
- Did the account interact with confidential information?

### Data Exfiltration Validation

- Was data transferred externally?
- Which tools were used?
- What evidence supports exfiltration?

### Incident Response

- Does the activity require escalation?
- Should containment occur?

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

## 🔍 Key Investigation Areas

### 1️⃣ Authentication Investigation

The investigation began by reviewing:

| Event ID | Description |
|-----------|-------------|
| 4624 | Successful Logon |

The objective was determining:

- Which systems were accessed
- Which accounts were involved
- Whether authentication behavior appeared unusual

Observed activity occurred across multiple systems, increasing investigative concern.

---

### 2️⃣ Privileged Access Validation

The investigation reviewed:

| Event ID | Description |
|-----------|-------------|
| 4672 | Special Privileges Assigned |

This confirmed that the account possessed elevated administrative privileges.

This finding was important because privileged accounts can access sensitive resources that standard users cannot.

---

### 3️⃣ Process Creation Investigation

The investigation reviewed:

| Event ID | Description |
|-----------|-------------|
| 4688 | Process Creation |

Processes observed included:

```text
cmd.exe
powershell.exe
rclone.exe
```

This shifted the investigation from authentication activity to behavioral analysis.

---

### 4️⃣ Encoded PowerShell Analysis

One of the strongest findings involved PowerShell execution using:

```text
-NoProfile
-WindowStyle Hidden
-EncodedCommand
```

Encoded PowerShell commands are commonly associated with:

- Defense evasion
- Obfuscation
- Hidden execution
- Reduced visibility

The presence of encoded commands significantly increased investigative concern.

---

### 5️⃣ Sensitive File Access Investigation

The investigation reviewed:

| Event ID | Description |
|-----------|-------------|
| 4663 | Object Access |

Evidence confirmed access to:

```text
Q4_Salary.xlsx
```

This represented potentially sensitive organizational information.

The file access activity became especially important when combined with later findings.

---

### 6️⃣ Rclone Activity Investigation

The investigation identified execution of:

```text
rclone.exe
```

Rclone is a legitimate cloud synchronization tool.

However, attackers frequently abuse it for:

- Google Drive uploads
- Dropbox uploads
- OneDrive uploads
- Cloud-based data exfiltration

Additional evidence showed Rclone activity referencing:

```text
Q4_Salary.xlsx
```

This dramatically increased confidence that data transfer activity may have occurred.

---

### 7️⃣ EDR Correlation

Elastic EDR telemetry was used to validate:

- Process execution
- Command-line activity
- Parent-child relationships
- Timeline evidence

Observed process chain:

```text
cmd.exe
      ↓
powershell.exe
      ↓
rclone.exe
```

This helped explain how the activity occurred rather than simply showing that it occurred.

---

## 📊 Key Findings

The investigation identified:

✅ Successful privileged logons

✅ Administrative privilege assignment

✅ Encoded PowerShell execution

✅ Sensitive file access activity

✅ Rclone execution

✅ Potential cloud-based file transfer

✅ EDR confirmation of process activity

✅ Evidence supporting possible data exfiltration

---

## 🚨 Why This Activity Was Concerning

No single event proved malicious activity.

However, the investigation revealed a sequence of related actions:

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

Individually, some actions could be legitimate.

Together, they created a much stronger indication of privileged account misuse and possible insider threat behavior.

---

## 🧠 Investigation Thinking

One major lesson from this project was learning that:

> Authentication events rarely tell the full story.

At first, the investigation appeared to involve login activity.

As more evidence was reviewed, the focus shifted toward:

- Process behavior
- File access
- Command execution
- Data movement

This reinforced the importance of investigating what happens after access is obtained.

---

## 🛠️ Recommended Containment Actions

Based on the evidence available:

- Isolate affected systems
- Reset privileged credentials
- Block unauthorized Rclone usage
- Review additional file access activity
- Investigate blast radius
- Monitor privileged accounts
- Review cloud storage activity

---

## 📸 Featured Investigation Evidence

The strongest screenshots from this project include:

- Initial Insider Threat Finding
- Privileged Logon Investigation
- Special Privileges Assigned
- Encoded PowerShell Execution
- Salary File Exfiltration Command
- File Access Validation
- Process Timeline Investigation
- Final Exfiltration Evidence

Together these screenshots tell the complete investigation story:

```text
Alert
 ↓
Authentication
 ↓
Privilege Assignment
 ↓
PowerShell Abuse
 ↓
Sensitive File Access
 ↓
Rclone Activity
 ↓
Data Exfiltration Evidence
 ↓
EDR Confirmation
```

---

## 🛠️ Skills Demonstrated

- Insider threat investigation
- Privileged account monitoring
- Windows Event Log analysis
- Authentication review
- Event ID interpretation
- PowerShell investigation
- Rclone analysis
- Data exfiltration detection
- Process tree analysis
- EDR correlation
- Blast radius thinking
- Incident response reasoning
- Containment decision-making
- Security documentation

---

## 📚 Learning Outcomes

Through this investigation I strengthened my ability to:

- Investigate privileged account activity
- Interpret Event IDs 4624, 4672, 4688, and 4663
- Analyze PowerShell abuse indicators
- Review process execution behavior
- Investigate sensitive file access
- Correlate SIEM and EDR evidence
- Validate potential data exfiltration activity
- Build evidence-based conclusions
- Document investigations using professional SOC workflows

---

## 🧾 Professional Summary

This project focused on determining whether unusual privileged account activity represented legitimate administration or potential data theft. By reviewing authentication activity, privilege assignments, process execution, file access events, PowerShell behavior, and Rclone activity, I was able to build a clearer understanding of what occurred. The strongest findings involved encoded PowerShell execution, access to sensitive salary data, and cloud transfer activity associated with Rclone. This investigation strengthened my understanding of insider threat investigations, privileged account abuse detection, data exfiltration analysis, and evidence-based incident response decision-making.

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

# ⚠️ Disclaimer

These projects was completed in a controlled educational and cybersecurity training environment. All logs, alerts, indicators, and investigation artifacts were used strictly for learning and professional development purposes.

---

# 👨‍💻 Author

**Fitzgerald Afari-Minta**

Cybersecurity | SOC Operations | Incident Response | Threat Investigation
