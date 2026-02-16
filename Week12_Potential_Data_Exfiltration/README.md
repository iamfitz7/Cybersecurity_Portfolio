# 🛡️ Week 12 — Potential Data Exfiltration (Splunk)

This folder documents a full SOC-style investigation built around a single alert and expanded into a complete case file. The focus is on validating detections, prioritizing risk, enriching data with context, and producing a clear escalation with defensible evidence.

This work reflects how an L1 analyst investigates potential data exfiltration in a real SOC environment.

---

## 🎯 Learning Goals
- Take ownership of an alert from start to finish  
- Validate detection logic using SPL  
- Convert raw log data into readable evidence  
- Prioritize users, hosts, and destinations by risk  
- Enrich alerts with web log context  
- Write clear escalation notes based on evidence  

---

## 🚨 SOC Case File: Potential Data Exfiltration to Cloud Storage
**What I did:**  
Opened an alert in Mission Control and reviewed why it fired. Validated the detection logic by reproducing the SPL search and confirming matching events. Converted outbound byte counts into megabytes to make activity easier to interpret and ranked users, hosts, departments, and destinations to identify the highest-risk behavior.

Pivoted from firewall-style logs into web logs to enrich the investigation with URLs, content types, categories, and user agents. Identified indicators such as scripted transfer behavior and cloud storage destinations. Assessed what was suspicious, what was unknown, and documented a clear escalation with next steps.

**Artifacts added:**  
- Alert overview and context screenshots  
- SPL reproduction and result tables  
- Prioritization views (top users, hosts, destinations)  
- Web log enrichment screenshots  
- Escalation notes and evidence summary  

**Why this matters:**  
SOC analysts must turn alerts into clear stories that justify escalation without making unsupported claims.

---

## 📦 SPL Triage Pack: High-Volume Outbound Transfers
**What I did:**  
Built a reusable set of SPL searches designed to quickly answer common triage questions around outbound data volume. Focused on ranking entities and identifying patterns across users, hosts, departments, destinations, and time windows.

**Artifacts added:**  
- Saved SPL searches  
- Screenshots of ranked results and time-based views  

**Why this matters:**  
Reusable triage workflows help SOC teams move from large datasets to focused investigations faster.

---

## 🛠️ Tools & Technologies
- Splunk (Mission Control, Search & Reporting)  
- SPL  
- Web proxy logs  
- Firewall-style network logs  

---

## 📁 Repository Structure

```text
/
├── soc-case-file/
│   ├── alert-context/
│   ├── enrichment/
│   └── screenshots/
├── triage-pack/
│   ├── spl-searches/
│   └── screenshots/
├── escalation-notes/
│   └── case-summary/
└── README.md
