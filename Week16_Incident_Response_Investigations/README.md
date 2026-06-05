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

# 🌐 Lab 02 — Layer 7 HTTP Flood / DDoS Investigation

---

## 🛡️ Incident Overview

This project documents a structured investigation into potential **Layer 7 HTTP flood activity** identified through abnormal web traffic patterns.

The investigation began after monitoring systems detected a significant increase in HTTP requests targeting a web application environment. Rather than assuming a denial-of-service attack was occurring, the objective was to validate the activity through evidence collection, traffic analysis, source attribution, endpoint targeting review, application impact assessment, and threat validation.

The goal of the investigation was to determine:

- Whether traffic behavior was actually abnormal
- Whether requests originated from a single source or multiple systems
- Which application resources were being targeted
- Whether the activity appeared automated
- Whether application performance was affected
- Whether evidence supported a Layer 7 DDoS hypothesis
- What response actions would be appropriate

This project mirrors a realistic SOC and Incident Response workflow used to investigate potential web application attacks.

---

## 🎯 Investigation Goals

The investigation focused on answering several key questions:

### Traffic Validation

- Is the increase in requests legitimate?
- Does activity exceed the normal baseline?

### Source Attribution

- Which systems are generating requests?
- Is the activity distributed?

### Endpoint Targeting

- Which application resources are being accessed?
- Are login pages or APIs being targeted?

### Application Impact

- Is the web application showing signs of stress?
- Are HTTP errors increasing?

### Automation Analysis

- Does User-Agent data indicate automation?
- Are tools such as curl or python-requests involved?

### Threat Validation

- Does threat intelligence support the investigation?
- Does available evidence support a DDoS conclusion?

---

## 🔄 Investigation Workflow

```text
Initial Security Finding
          ↓
Traffic Baseline Analysis
          ↓
Requests-Per-Minute Review
          ↓
Source IP Attribution
          ↓
Distributed Traffic Analysis
          ↓
Endpoint Targeting Investigation
          ↓
HTTP Error Correlation
          ↓
User-Agent Analysis
          ↓
Threat Intelligence Validation
          ↓
Evidence Correlation & Conclusion
```

This workflow reflects how many SOC and Incident Response teams validate potential denial-of-service events in production environments.

---

## 🛠️ Tools & Data Sources Used

| Tool / Data Source | Purpose |
|--------------------|----------|
| Web Server Logs | Request analysis |
| HTTP Request Data | Traffic volume review |
| Source IP Analysis | Attribution and distribution review |
| Endpoint Request Analysis | Resource targeting investigation |
| User-Agent Analysis | Automation detection |
| Threat Intelligence Platforms | Indicator validation |
| Incident Response Methodology | Investigation and decision making |

---

## 🔍 Investigation Process

### 1. Initial Security Finding Review

The investigation began with a security alert indicating potentially suspicious web traffic activity.

The alert served as a starting point but was not treated as proof of malicious activity.

The objective was to validate the alert using supporting evidence.

---

### 2. Traffic Baseline Analysis

Requests-per-minute data was reviewed to determine whether traffic levels exceeded normal operating patterns.

The analysis identified a significant spike above baseline activity.

This established that abnormal behavior was occurring and justified deeper investigation.

---

### 3. Source IP Attribution

Source analysis was performed to identify which systems generated the elevated traffic.

Multiple external IP addresses were observed contributing significant request volumes.

Examples included:

- `203.0.113.10`
- `203.0.113.5`
- `203.0.113.8`

This suggested the activity was not isolated to a single host.

---

### 4. Distributed Traffic Analysis

Further analysis showed multiple systems generating similar request volumes.

This finding was important because it shifted the investigation from:

```text
Possible Single-Source DoS
```

to:

```text
Potential Distributed Denial-of-Service Activity
```

The distributed nature of the traffic strengthened the DDoS hypothesis.

---

### 5. Endpoint Targeting Investigation

The investigation identified repeated requests against:

- `/`
- `/login`
- `/api/v1/data`

These resources are commonly targeted because they require application-side processing and can consume server resources under high request volumes.

Understanding what was being targeted helped determine attacker intent and potential impact.

---

### 6. HTTP Error Correlation

HTTP status code analysis identified elevated server-side error activity.

The investigation observed:

- Increased HTTP 5xx responses
- Elevated application error rates
- Signs of operational stress

This provided evidence that the traffic was affecting application performance.

---

### 7. User-Agent Analysis

User-Agent review identified indicators associated with automated tooling.

Observed values included:

- `curl`
- `python-requests`
- `Mozilla`

This helped distinguish likely human activity from scripted request generation.

The presence of automation indicators significantly increased confidence in the investigation findings.

---

### 8. Threat Intelligence Validation

Threat intelligence enrichment was performed to validate indicators identified during the investigation.

This helped provide additional context regarding observed activity and strengthened the overall evidence set.

---

## 📊 Key Findings

The investigation identified:

- Significant traffic volume increases above baseline activity
- Multiple source IP addresses generating elevated request volumes
- Distributed traffic patterns consistent with DDoS behavior
- Repeated targeting of:
  - `/`
  - `/login`
  - `/api/v1/data`
- Increased HTTP 5xx server errors
- User-Agent indicators associated with automated tools
- Evidence supporting Layer 7 HTTP flood activity

---

## 🧠 Investigation Thinking

One of the most important lessons from this project was learning that:

> High traffic volume alone does not prove a DDoS attack.

A weaker investigation might conclude:

> "Traffic spiked, therefore it must be DDoS."

A stronger investigation asks:

- Is traffic actually abnormal?
- Is the activity distributed?
- What resources are being targeted?
- Is the application impacted?
- Is automation involved?
- Do multiple evidence sources support the conclusion?

This project reinforced the importance of validating evidence before reaching conclusions.

---

## 🚨 Why This Was More Than a Traffic Spike

Several independent findings supported the DDoS hypothesis:

- Requests significantly exceeded baseline levels
- Traffic originated from multiple systems
- Critical application resources were targeted
- HTTP error rates increased
- Automated User-Agent indicators were observed
- Threat intelligence supported investigative findings

Together, these indicators created a stronger evidence-based case than traffic volume alone.

---

## 📸 Featured Investigation Evidence

### Traffic Anomaly Detection

Evidence showing a significant increase in requests-per-minute above normal baseline activity.

**Screenshot:**  
`Week16_Lab2_02_Traffic_Anomaly_Detection.png`

---

### Distributed Traffic Evidence

Evidence showing multiple source IP addresses generating elevated request volumes.

**Screenshot:**  
`Week16_Lab2_04_Distributed_Traffic_Evidence.png`

---

### Automated Traffic Confirmation

Evidence showing User-Agent indicators associated with automated tools including:

- curl
- python-requests

**Screenshot:**  
`Week16_Lab2_09_Automated_Traffic_Confirmation.png`

---

## 🛠️ Skills Demonstrated

- Traffic anomaly detection
- Requests-per-minute analysis
- Source IP attribution
- Distributed traffic analysis
- Endpoint targeting review
- HTTP error correlation
- User-Agent analysis
- Threat intelligence enrichment
- Evidence correlation
- Detection validation
- Incident response workflow execution
- Security documentation

---

## 📚 Learning Outcomes

Through this investigation I strengthened my ability to:

- Validate potential DDoS activity
- Analyze web traffic patterns
- Investigate application-layer attacks
- Correlate multiple evidence sources
- Distinguish automation from legitimate user activity
- Assess application impact
- Build evidence-based conclusions
- Document investigations using professional SOC methodologies

---

## ⚠️ Common Beginner Mistake

One common mistake is assuming that high traffic volume automatically means a DDoS attack is occurring.

In reality, traffic spikes may be caused by:

- legitimate user demand
- marketing campaigns
- automated updates
- business operations
- scheduled tasks

Effective investigations require validating:

- source distribution
- targeted resources
- application impact
- automation indicators

before reaching conclusions.

---

## 🔧 Recommended Improvements

Potential improvements include:

- Automated baseline monitoring
- Improved anomaly detection thresholds
- Enhanced web traffic visibility
- Stronger rate-limiting controls
- WAF tuning and optimization
- Improved alert correlation workflows

---

## 🧾 Professional Summary

This project focused on investigating potential Layer 7 HTTP flood activity through structured traffic analysis and evidence validation.

The investigation began with abnormal web traffic activity and expanded into source attribution, endpoint targeting analysis, HTTP error review, User-Agent investigation, and threat intelligence validation.

The most valuable lesson from this project was learning that effective incident response depends on validating evidence rather than trusting alert names alone. By correlating traffic volume, source distribution, targeted resources, application impact, and automation indicators, I was able to build a stronger understanding of how Layer 7 DDoS investigations are performed in real-world environments.


# ⚠️ Disclaimer

This project was completed in a controlled educational and cybersecurity training environment. All logs, alerts, indicators, and investigation artifacts were used strictly for learning and professional development purposes.

---

# 👨‍💻 Author

**Fitzgerald Afari-Minta**

Cybersecurity | SOC Operations | Incident Response | Threat Investigation
