# 🛡️ Week 11 — SIEM Investigations & Alert Prioritization (Splunk)

This folder documents hands-on SOC-style investigations focused on alert triage, data prioritization, and evidence-based decision-making. The work shows how large volumes of security logs are turned into clear, defensible conclusions without overstating risk.

---

## 🎯 Learning Goals
- Identify and prioritize suspicious activity from large datasets  
- Build SPL queries that surface high-risk behavior  
- Conduct structured investigations using endpoint and web logs  
- Scope impact and assess exposure without assumptions  
- Make clear escalation, closure, or tuning decisions  

---

## 📊 High-Volume Transfer Detection and Prioritization
**What I did:**  
Analyzed outbound transfer data to identify unusually high data volumes. Converted raw byte counts into readable values, applied thresholds, and ranked users, hosts, departments, and destinations to focus on the highest-risk activity.

**Artifacts added:**  
- Screenshots of SPL queries and results  
- Tables showing top users, hosts, and destinations  

**Why this matters:**  
SOC analysts must quickly reduce large datasets into a short, prioritized list without labeling activity as confirmed exfiltration.

---

## 🧪 Vulnerable Software Execution Investigation (Notepad++)
**What I did:**  
Conducted an end-to-end investigation using process execution logs to identify vulnerable software versions, determine affected hosts, and assess execution behavior. Reviewed frequency, blast radius, parent processes, and execution paths to determine if activity was benign or suspicious.

**Artifacts added:**  
- Screenshots of process execution queries  
- Host frequency and scope results  
- Evidence of parent process and execution paths  

**Why this matters:**  
Understanding scope and execution context prevents false positives and supports accurate risk-based conclusions.

---

## 🔎 Alert Triage and Log Enrichment
**What I did:**  
Triaged alerts by validating detection logic and reviewing core details such as user, host, time window, and activity type. Pivoted from alert data into related web logs to enrich context using URLs, categories, content types, and user agents.

**Artifacts added:**  
- Alert detail screenshots  
- Enriched log views and filtered results  

**Why this matters:**  
Effective enrichment turns alerts into investigations and helps analysts decide next steps confidently.

---

## ⚖️ SOC Decision-Making and Escalation
**What I did:**  
Reviewed collected evidence and documented clear outcomes for each case, including escalation, closure, or tuning recommendations. Defined what information should be handed off to higher tiers and what should not.

**Artifacts added:**  
- Decision summaries  
- Escalation notes and evidence snapshots  

**Why this matters:**  
Clear decisions and clean handoffs keep SOC operations efficient and credible.

---

## 🛠️ Tools & Technologies
- Splunk (Search & Reporting, Mission Control)  
- SPL  
- Sysmon telemetry  
- Web proxy logs  
- OSINT sources  

---

## 📁 Repository Structure

```text
/
├── volume-detection/
│   ├── spl-workflows/
│   └── screenshots/
├── vulnerable-software/
│   ├── notepadpp-analysis/
│   └── screenshots/
├── alert-enrichment/
│   ├── web-log-pivots/
│   └── screenshots/
├── decision-notes/
│   ├── escalation-summaries/
│   └── screenshots/
└── README.md
