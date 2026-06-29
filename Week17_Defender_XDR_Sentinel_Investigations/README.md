# Week 17 — Microsoft Defender XDR: Deep EDR Investigation Lab

## Lab Overview

Week 17 focuses on developing practical experience with **Microsoft Defender XDR**, Microsoft's enterprise Endpoint Detection and Response (EDR) platform. This lab emphasizes the investigative workflow used by modern Security Operations Centers (SOCs), including endpoint alert analysis, device investigations, user investigations, process analysis, advanced hunting, and incident response.

Unlike traditional SIEM investigations that primarily rely on log analysis, Microsoft Defender XDR provides rich endpoint telemetry that enables analysts to investigate malicious activity directly on affected devices, understand attacker behavior, determine the scope of an incident, and make informed containment decisions.

Throughout this week, I explored how enterprise analysts investigate security incidents from the initial alert through containment and remediation while using Microsoft Defender XDR, Microsoft Defender for Endpoint, Microsoft Sentinel, and Kusto Query Language (KQL).

---

# Learning Objectives

- Understand the Microsoft Defender XDR portal
- Investigate enterprise endpoint alerts
- Analyze device timelines
- Analyze user timelines
- Perform Advanced Hunting with KQL
- Investigate suspicious processes
- Analyze process trees
- Investigate suspicious files
- Review endpoint evidence
- Correlate threat intelligence
- Perform endpoint incident investigations
- Make containment and remediation decisions

---

# Technologies Used

- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Microsoft Sentinel
- Microsoft Entra ID
- Microsoft 365 Defender
- Windows 11
- Kusto Query Language (KQL)

---

# Skills Demonstrated

## Endpoint Detection & Response (EDR)

- Endpoint investigations
- Alert triage
- Incident response
- Threat hunting
- Malware analysis
- Process investigations
- Device investigations
- User investigations

## Microsoft Defender XDR

- Incident Queue
- Device Inventory
- Device Timeline
- User Timeline
- Advanced Hunting
- Endpoint Alerts
- Process Trees
- Device Isolation
- File Investigation
- Threat Intelligence Integration

## Incident Response

- Detection
- Validation
- Investigation
- Scoping
- Containment
- Remediation
- Documentation

---

# Enterprise Investigation Workflow

```text
Security Alert
      │
      ▼
Incident Queue
      │
      ▼
Alert Validation
      │
      ▼
Device Investigation
      │
      ▼
User Investigation
      │
      ▼
Process Tree Analysis
      │
      ▼
Threat Hunting (KQL)
      │
````
