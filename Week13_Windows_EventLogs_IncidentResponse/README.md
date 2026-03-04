# 🛡️ Week 13 — Windows Event Logs & Incident Response Investigation

This folder documents hands-on work analyzing Windows Event Logs and applying basic incident response investigation steps. The focus is on identifying suspicious activity, correlating logs, and producing a clear incident report using evidence from system telemetry.

The work shows how system logs are used to detect events such as failed logins, scanning behavior, and other activity that may indicate compromise.

---

## 🎯 Learning Goals
- Understand the purpose of major Windows event log categories  
- Identify suspicious activity using event logs  
- Correlate multiple logs to build investigation context  
- Practice basic incident response workflow  
- Document findings in a structured incident report  

---

## 📜 Windows Event Log Exploration
**What I did:**  
Reviewed major Windows Event Log categories and examined how different system events appear in each log source.

**Artifacts added:**  
- Screenshots of Security logs  
- Screenshots of System logs  
- Screenshots of Application logs  

**Why this matters:**  
Understanding where events are recorded helps investigators locate useful evidence faster.

---

## 🔐 Failed Login Detection
**What I did:**  
Generated multiple failed login attempts in a controlled environment and reviewed the resulting log entries.

**Artifacts added:**  
- Screenshots showing failed authentication events  
- Annotated timestamps, user accounts, and source details  

**Why this matters:**  
Repeated login failures are often early indicators of brute-force activity or account misuse.

---

## 🌐 Port Scan Activity Detection
**What I did:**  
Generated network scanning activity and observed how the system recorded the behavior within Windows logs.

**Artifacts added:**  
- Screenshots showing scanning-related log events  

**Why this matters:**  
Port scans often occur during early reconnaissance phases of attacks.

---

## 🔗 Log Correlation and Event Analysis
**What I did:**  
Combined multiple log sources to analyze activity patterns and identify relationships between system events.

**Artifacts added:**  
- Screenshots showing correlated log views  
- Notes explaining how the events relate to each other  

**Why this matters:**  
Real investigations require combining multiple data sources to understand what actually happened.

---

## 📄 Incident Response Investigation
**What I did:**  
Followed a structured incident response workflow to analyze activity observed in the logs and document findings. Created a report summarizing the investigation, evidence reviewed, and conclusions.

**Artifacts added:**  
- Incident response report draft  
- Investigation notes and screenshots  

**Why this matters:**  
Clear documentation allows teams to understand incidents quickly and respond appropriately.

---

## 🛠️ Tools & Technologies
- Windows Event Viewer  
- Splunk (log analysis and correlation)  
- Virtual machines  

---

## 📁 Repository Structure

```text
/
├── log-analysis/
│   ├── event-categories/
│   └── screenshots/
├── failed-login-investigation/
│   ├── authentication-events/
│   └── screenshots/
├── scan-detection/
│   ├── network-scan-events/
│   └── screenshots/
├── incident-response/
│   ├── investigation-notes/
│   └── ir-report/
└── README.md
