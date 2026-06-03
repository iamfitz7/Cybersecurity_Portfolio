# ☁️ Data Exfiltration Investigation Through Cloud Storage

## 📖 Incident Overview

This project documents the investigation of a potential data exfiltration incident involving cloud storage services. The investigation began after a security alert identified unusually large outbound data transfers to a cloud-based file sharing platform.

The objective was to determine:

- Whether sensitive data was actually leaving the environment
- Who was responsible for the activity
- What files were involved
- How the transfer occurred
- Whether the behavior was legitimate or suspicious
- What response actions were appropriate

This investigation follows a realistic Security Operations Center (SOC) and Incident Response (IR) workflow by correlating evidence across multiple security tools and data sources.

---

# 🎯 Investigation Goals

The investigation focused on answering the following questions:

### Validation

- Is the alert legitimate?
- Is outbound traffic actually occurring?

### User Attribution

- Which user initiated the activity?
- Is the activity associated with a known employee?

### Threat Intelligence

- Is the destination known to be malicious?
- Are there any indicators of compromise associated with the destination?

### Endpoint Investigation

- What processes were responsible for the activity?
- Were suspicious tools or executables involved?

### Host Artifact Review

- What files were accessed or transferred?
- Are the files business-related or sensitive?

### Incident Response

- Does the activity require containment?
- What actions should be taken next?

---

# 🔄 Investigation Workflow

The investigation followed the workflow below:

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

This workflow mirrors how many SOC and Incident Response teams validate and investigate potential exfiltration events in production environments.

---

# 🛠 Tools Used

| Tool | Purpose |
|--------|---------|
| Splunk | Alert validation and log analysis |
| Proxy Logs | Outbound connection review |
| Threat Intelligence Platforms | Reputation validation |
| Elastic EDR | Endpoint investigation |
| Windows Host Artifacts | File activity review |
| Incident Response Methodology | Containment decision making |

---

# 🔎 Investigation Process

## 1. Splunk Alert Validation

The investigation began with a Splunk security alert indicating unusual outbound traffic to a cloud storage provider.

The alert was reviewed to determine:

- Source host
- Source user
- Destination service
- Data transfer volume
- Alert severity

The objective was to validate that the alert represented real activity rather than a false positive.

---

## 2. Proxy Log Analysis

Proxy logs were analyzed to identify:

- Upload destinations
- Data transfer size
- User activity
- Connection timing
- Frequency of uploads

This step helped establish whether data was actually being transferred outside the organization.

---

## 3. Threat Intelligence Review

Threat intelligence sources were consulted to determine:

- Domain reputation
- Historical malicious activity
- Known abuse reports
- Security vendor classifications

The goal was to determine whether the destination itself represented a known threat.

---

## 4. Endpoint Investigation

Elastic EDR telemetry was reviewed to identify:

- Responsible processes
- Parent-child process relationships
- User context
- Execution history
- Associated alerts

The investigation focused on understanding how the upload activity originated from the endpoint.

---

## 5. Host Artifact Review

Host artifacts were examined to determine:

- Files involved
- File locations
- File ownership
- Data sensitivity
- Recent file activity

This step provided visibility into exactly what content may have been transferred.

---

## 6. Containment Decision

Based on all available evidence, a determination was made regarding:

- Potential business impact
- Data exposure risk
- User intent
- Need for host isolation
- Escalation requirements

Appropriate response actions were then documented.

---

# 📂 Repository Structure

```text
Week16_Incident_Response_Investigations/
│
├── README.md
├── Data_Exfiltration_Incident_Case_File.md
├── Technical_Investigation_Writeup.md
│
└── screenshots/
    ├── 01_Splunk_Alert.png
    ├── 02_Proxy_Log_Analysis.png
    ├── 03_Threat_Intelligence_Review.png
    ├── 04_EDR_Investigation.png
    ├── 05_Host_Artifact_Review.png
    └── 06_Containment_Decision.png
```

---

# 📸 Investigation Evidence

## Alert Validation

Evidence showing the initial Splunk security alert.

---

## Proxy Analysis

Evidence showing outbound cloud storage activity.

---

## Threat Intelligence Validation

Evidence showing reputation analysis of the destination.

---

## Endpoint Investigation

Evidence showing process execution and endpoint telemetry.

---

## Host Artifact Review

Evidence showing files associated with the activity.

---

## Containment Decision

Evidence supporting the final response determination.

---

# 🧠 Key Skills Demonstrated

- Security Alert Validation
- Incident Response Workflow
- Splunk Investigation
- Proxy Log Analysis
- Threat Intelligence Analysis
- Endpoint Detection and Response (EDR)
- Host Artifact Investigation
- Data Exfiltration Detection
- Evidence Correlation
- Security Documentation
- SOC Operations
- Incident Escalation Decision Making

---

# 📚 Learning Outcomes

Through this investigation I strengthened my ability to:

- Validate potential security incidents
- Correlate evidence across multiple tools
- Investigate potential data exfiltration activity
- Analyze endpoint telemetry
- Review host-level artifacts
- Evaluate business impact
- Make containment recommendations
- Document investigations using professional SOC methodologies

---

# ⚠️ Disclaimer

This project was completed in a controlled educational and cybersecurity training environment. All logs, alerts, indicators, and investigation artifacts were used strictly for learning and professional development purposes.

---

# 👨‍💻 Author

**Fitzgerald Afari-Minta**

Cybersecurity | SOC Operations | Incident Response | Threat Investigation
