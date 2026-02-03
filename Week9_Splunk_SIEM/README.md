# 🛡️ Week 9 — SIEM Fundamentals with Splunk

This folder documents hands-on SIEM work focused on understanding where security data lives, how to search it correctly, and how alerts are handled in a real SOC-style workflow. The emphasis is on accuracy, data awareness, and proper investigation habits.

---

## 🎯 Learning Goals
- Identify and validate available log sources in a SIEM  
- Understand how indexes map to real security telemetry  
- Write accurate search queries using correct logic  
- Recognize parsed fields and why they matter  
- Follow proper alert handling and workflow discipline  

---

## ⚙️ SIEM Setup and Access
**What I did:**  
Installed and accessed the SIEM platform to confirm it was running and ready for log analysis.

**Artifacts added:**  
- Screenshot showing the SIEM interface running

**Why this matters:**  
A SIEM must be accessible and stable before any investigation or monitoring can happen.

---

## 🗂️ Index and Log Source Discovery
**What I did:**  
Reviewed available indexes and identified which ones contain security-relevant data. Explored log sources to understand what type of events each index represents.

**Artifacts added:**  
- Screenshot of available indexes  
- Screenshot of raw and parsed events from a security index

**Why this matters:**  
Knowing where data lives prevents wasted time and missed evidence during investigations.

---

## 🔍 SPL Fundamentals and Search Logic
**What I did:**  
Ran search queries using different logic types and field-based filtering. Compared typed searches with GUI-based filtering to validate results.

**Artifacts added:**  
- Screenshots showing search logic differences  
- Screenshots of filtered results

**Why this matters:**  
Incorrect logic can lead to false conclusions and missed security events.

---

## 🧩 Log Parsing and Field Awareness
**What I did:**  
Reviewed raw logs and parsed fields to understand how event data is structured. Used fields like usernames and hostnames to filter results accurately.

**Artifacts added:**  
- Screenshot showing raw vs parsed event views  
- Screenshot of field-based filtering

**Why this matters:**  
Understanding fields and schemas explains why some searches work and others fail.

---

## 🚨 Mission Control Workflow and Alert Handling
**What I did:**  
Reviewed alerts in the SIEM workflow interface, assigned ownership, updated alert status, and inspected the related detection logic.

**Artifacts added:**  
- Screenshot of alert overview  
- Screenshot showing alert status changes  
- Screenshot of detection rule and search results

**Why this matters:**  
Following proper workflow ensures alerts are handled consistently and metrics remain accurate.

---

## 📁 Repository Structure
Week_9_SIEM_Splunk/
├── configs/ # SIEM and log-related configuration files
├── searches/ # Saved search queries and logic examples
├── alerts/ # Alert details and workflow references
├── analysis/ # Notes on log sources and field observations
└── screenshots/ # Visual proof of searches, alerts, and logs
