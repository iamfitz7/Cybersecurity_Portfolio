# Week 17 — Microsoft Defender XDR: Enterprise EDR Investigation Series

> A multi-lab series focused on developing practical experience with **Microsoft Defender XDR**, Microsoft's enterprise Endpoint Detection and Response (EDR) platform. These labs simulate real-world Security Operations Center (SOC) investigations by covering endpoint alerts, process analysis, advanced hunting, device investigations, user investigations, threat intelligence correlation, and incident response workflows.

---

# Overview

Endpoint Detection and Response (EDR) platforms have become one of the most critical technologies within modern Security Operations Centers (SOCs). While SIEM platforms collect and correlate security logs across an environment, EDR solutions provide deep visibility into endpoint activity, allowing analysts to investigate attacks at the device level and respond quickly to malicious behavior.

This week consists of a series of hands-on Microsoft Defender XDR labs designed to build proficiency with Microsoft's enterprise security ecosystem. Throughout these labs, I investigated endpoint alerts, analyzed process trees, reviewed device and user timelines, performed Advanced Hunting using Kusto Query Language (KQL), correlated threat intelligence, and practiced incident response workflows that closely resemble those used by enterprise security teams.

The goal of this lab series is to develop practical experience with Microsoft Defender XDR while strengthening the skills required for Security Operations, Incident Response, Threat Hunting, and Security Engineering roles.

---

# Learning Objectives

By completing this lab series, I will:

- Develop proficiency with Microsoft Defender XDR
- Understand enterprise EDR workflows
- Investigate endpoint security incidents
- Analyze process execution and process trees
- Examine device timelines
- Investigate user activity
- Perform Advanced Hunting with KQL
- Investigate malicious files
- Correlate endpoint telemetry with threat intelligence
- Make containment and remediation decisions
- Document enterprise incident investigations

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
- Malware investigations
- Process analysis
- Device investigations
- User investigations
- File investigations

## Microsoft Defender XDR

- Incident Queue
- Device Inventory
- Device Timeline
- User Timeline
- Advanced Hunting
- Endpoint Alerts
- Process Trees
- Device Isolation
- Live Response
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
Advanced Hunting (KQL)
      │
      ▼
Threat Intelligence Correlation
      │
      ▼
Determine Scope
      │
      ▼
Containment Decision
      │
      ▼
Incident Report
```

---

# Lab Series

## Lab 1 — Microsoft Defender XDR Portal Overview

### Objectives

- Explore the Microsoft Defender XDR portal
- Understand the security dashboard
- Navigate the Incident Queue
- Review Devices, Users, and Assets
- Familiarize yourself with investigation tools

**Topics Covered**

- Microsoft Defender XDR Portal
- Security Dashboard
- Incident Queue
- Devices
- Users
- Assets
- Security Alerts

---

## Lab 2 — Endpoint Alert Investigation

### Objectives

- Investigate endpoint security alerts
- Validate detections
- Determine alert severity
- Review evidence collected by Defender

**Topics Covered**

- Endpoint Alerts
- Alert Details
- Alert Evidence
- Alert Correlation
- Incident Prioritization

---

## Lab 3 — Device Timeline Investigation

### Objectives

- Analyze endpoint telemetry
- Review device activity
- Build an attack timeline
- Identify suspicious behavior

**Topics Covered**

- Device Timeline
- Process Events
- File Events
- Network Events
- Registry Activity
- Logon Events

---

## Lab 4 — User Timeline Investigation

### Objectives

- Investigate user behavior
- Review authentication activity
- Analyze account usage
- Identify suspicious sign-in activity

**Topics Covered**

- User Timeline
- Authentication Events
- Account Activity
- Privilege Changes
- Identity Investigation

---

## Lab 5 — Process Tree Investigation

### Objectives

- Analyze parent-child process relationships
- Identify malicious execution chains
- Detect Living-off-the-Land techniques (LOLBins)

**Topics Covered**

- Process Trees
- Parent Processes
- Child Processes
- Command-Line Analysis
- LOLBins
- Process Lineage

---

## Lab 6 — Suspicious PowerShell Investigation

### Objectives

- Investigate suspicious PowerShell execution
- Analyze encoded commands
- Determine attacker intent

**Topics Covered**

- PowerShell
- Encoded Commands
- Script Execution
- Command-Line Analysis
- Behavioral Detection

---

## Lab 7 — Malware Execution Investigation

### Objectives

- Investigate malicious file execution
- Analyze malware behavior
- Review endpoint detections

**Topics Covered**

- Malware Alerts
- File Reputation
- Hash Analysis
- Execution History
- Threat Intelligence

---

## Lab 8 — Credential Theft Investigation

### Objectives

- Investigate credential access techniques
- Review suspicious authentication activity
- Detect lateral movement indicators

**Topics Covered**

- Credential Access
- Authentication Logs
- LSASS Investigation
- Remote Access
- Lateral Movement

---

## Lab 9 — Persistence Investigation

### Objectives

- Detect persistence mechanisms
- Investigate attacker survival techniques

**Topics Covered**

- Registry Persistence
- Scheduled Tasks
- Startup Programs
- Services
- Autoruns

---

## Lab 10 — Advanced Hunting with KQL

### Objectives

- Hunt for suspicious activity across the environment
- Build custom KQL queries
- Investigate endpoint telemetry

**Topics Covered**

- KQL Fundamentals
- DeviceEvents
- DeviceProcessEvents
- DeviceFileEvents
- DeviceNetworkEvents
- DeviceRegistryEvents
- DeviceLogonEvents

---

## Lab 11 — Threat Intelligence Investigation

### Objectives

- Correlate endpoint evidence with threat intelligence
- Validate indicators of compromise (IOCs)
- Investigate known malicious artifacts

**Topics Covered**

- Threat Intelligence
- File Hashes
- Indicators of Compromise (IOCs)
- Reputation Analysis
- Threat Correlation

---

## Lab 12 — Device Isolation & Incident Response

### Objectives

- Evaluate containment options
- Determine when device isolation is appropriate
- Practice incident response decision-making

**Topics Covered**

- Device Isolation
- Containment
- Remediation
- Recovery
- Incident Documentation

---

## Lab 13 — End-to-End Microsoft Defender XDR Investigation

### Objectives

Conduct a complete enterprise investigation from the initial security alert through containment and documentation.

**Topics Covered**

- Incident Queue
- Alert Investigation
- Device Timeline
- User Timeline
- Process Trees
- Advanced Hunting
- Threat Intelligence
- Scope Determination
- Containment
- Incident Reporting

---

# Key Concepts Learned

- Enterprise EDR operations
- Endpoint telemetry analysis
- Behavioral threat detection
- Process tree analysis
- Threat hunting with KQL
- Malware investigations
- Credential theft investigations
- Persistence analysis
- Threat intelligence integration
- Enterprise incident response workflows

---

# Portfolio Value

This repository demonstrates hands-on experience with Microsoft's enterprise Endpoint Detection and Response platform through a structured series of investigative labs that simulate real-world Security Operations Center workflows.

The completed lab series showcases practical skills in:

- Microsoft Defender XDR
- Endpoint Detection & Response (EDR)
- Threat Hunting
- Incident Response
- Advanced Hunting with KQL
- Process Analysis
- Device Investigations
- User Investigations
- Threat Intelligence Correlation
- Enterprise Security Operations

---

# Repository Structure

```
Week-17-Microsoft-Defender-XDR/
│
├── Lab-01-Portal-Overview/
├── Lab-02-Endpoint-Alert-Investigation/
├── Lab-03-Device-Timeline-Investigation/
├── Lab-04-User-Timeline-Investigation/
├── Lab-05-Process-Tree-Investigation/
├── Lab-06-Suspicious-PowerShell-Investigation/
├── Lab-07-Malware-Execution-Investigation/
├── Lab-08-Credential-Theft-Investigation/
├── Lab-09-Persistence-Investigation/
├── Lab-10-Advanced-Hunting-KQL/
├── Lab-11-Threat-Intelligence-Investigation/
├── Lab-12-Device-Isolation-Incident-Response/
├── Lab-13-End-to-End-Defender-XDR-Investigation/
├── Screenshots/
├── KQL-Queries/
├── Investigation-Notes/
├── Incident-Reports/
└── README.md
```


