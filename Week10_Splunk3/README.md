# 🛡️ Week 10 — SIEM Alert Triage & SOC Decision-Making (Splunk)

This folder documents hands-on SIEM alert triage and investigation work using Splunk. The focus is on reviewing alerts, analyzing behavior, validating evidence with OSINT, and making clear decisions on whether to escalate, close, or tune alerts. The work reflects real responsibilities handled by SOC L1 analysts.

---

## 🎯 Learning Goals
- Perform alert triage using a structured workflow  
- Review evidence tied to SIEM detections  
- Analyze command-line and network behavior  
- Use OSINT to support investigation decisions  
- Decide when to escalate, close, or recommend tuning  

---

## 🚨 Suspicious PowerShell LOLBAS Alert Investigation
**What I did:**  
Reviewed a PowerShell alert in the SIEM, identified the affected host and user, examined the detection rule, and analyzed command-line activity. Extracted URLs from results and used OSINT sources to support an escalation decision.

**Artifacts added:**  
- Screenshots of alert details and detection rule  
- Search result screenshots showing command-line output  
- OSINT lookup screenshots

**Why this matters:**  
Understanding command-line behavior is critical for detecting abuse of built-in tools that attackers commonly use.

---

## 🌐 Malicious Domain Access Analysis (Proxy Logs + OSINT)
**What I did:**  
Investigated proxy-related alerts, identified affected users, extracted accessed domains, and classified them based on reputation using multiple OSINT sources.

**Artifacts added:**  
- Screenshots of proxy alert data  
- Domain list and classification screenshots  
- OSINT results screenshots

**Why this matters:**  
Proxy alerts generate high volume, and analysts must separate real threats from noise.

---

## ⚖️ SOC Decision-Making and Alert Outcomes
**What I did:**  
Reviewed investigation results and documented clear decisions on which alerts required escalation, which could be closed, and which needed tuning. Defined what information should be passed to higher tiers.

**Artifacts added:**  
- Decision summary screenshots  
- Notes showing escalation and tuning rationale

**Why this matters:**  
Good decisions keep SOC workflows efficient and ensure serious incidents get the right attention.

---

## 📁 Repository Structure

```text
/
├── alert-triage/
│   ├── powershell-lolbas/
│   └── screenshots/
├── proxy-domain-analysis/
│   ├── zscaler-logs/
│   └── screenshots/
├── osint/
│   ├── domain-reputation/
│   └── screenshots/
├── decision-making/
│   ├── escalation-notes/
│   └── screenshots/
└── README.md
