# Week 17 — Microsoft Defender XDR & Microsoft Sentinel: Enterprise EDR Investigation

> A hands-on enterprise security lab demonstrating how Microsoft Defender XDR and Microsoft Sentinel work together to detect, investigate, validate, and respond to security incidents. This project follows a real Security Operations Center (SOC) workflow by combining Endpoint Detection and Response (EDR), Security Information and Event Management (SIEM), threat hunting with Kusto Query Language (KQL), and incident response.

---

# Overview

Modern enterprise security teams rarely rely on a single security platform. Instead, they correlate endpoint telemetry, identity data, cloud activity, authentication logs, and security alerts across multiple Microsoft security solutions to build a complete understanding of an incident.

In this lab, I explored Microsoft's integrated security ecosystem by using **Microsoft Defender XDR** as the primary Endpoint Detection and Response (EDR) platform and **Microsoft Sentinel** as the cloud-native Security Information and Event Management (SIEM) platform.

The investigation followed a realistic SOC workflow beginning with an alert in Microsoft Defender XDR, followed by evidence collection, endpoint investigation, user and device analysis, timeline reconstruction, Advanced Hunting with Kusto Query Language (KQL), Sentinel log validation, and incident classification.

Rather than simply reviewing alerts, this project emphasizes how enterprise analysts investigate security incidents by correlating endpoint telemetry, cloud security data, authentication events, and behavioral indicators before determining an appropriate response.

---

# Objectives

Throughout this lab, I aimed to:

- Understand the Microsoft Defender XDR portal
- Navigate Microsoft Sentinel
- Investigate enterprise security incidents
- Perform endpoint investigations
- Analyze user and device context
- Review endpoint timelines
- Investigate process activity
- Perform Advanced Hunting using KQL
- Validate findings using Microsoft Sentinel
- Correlate endpoint and SIEM telemetry
- Practice incident documentation
- Evaluate containment options

---

# Technologies Used

- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Microsoft Sentinel
- Microsoft Entra ID
- Microsoft 365 Defender
- Microsoft Azure
- Windows 11
- Microsoft Edge
- Kusto Query Language (KQL)

---

# Skills Demonstrated

## Endpoint Detection & Response (EDR)

- Endpoint investigations
- Alert triage
- Device investigations
- User investigations
- Process analysis
- File investigations
- Timeline reconstruction
- Threat validation

## SIEM Operations

- Cloud log analysis
- Incident management
- Log correlation
- Incident validation
- Security monitoring
- Security investigations

## Threat Hunting

- Advanced Hunting
- KQL fundamentals
- Failed logon analysis
- PowerShell investigations
- Endpoint telemetry analysis
- Process hunting
- Authentication analysis

## Incident Response

- Alert validation
- Evidence collection
- Investigation
- Scoping
- Classification
- Containment planning
- Documentation

---

# Enterprise Investigation Workflow

```text
Security Alert
      │
      ▼
Microsoft Defender XDR
      │
      ▼
Incident Review
      │
      ▼
Evidence Collection
      │
      ▼
User Investigation
      │
      ▼
Device Investigation
      │
      ▼
Timeline Analysis
      │
      ▼
Advanced Hunting (KQL)
      │
      ▼
Microsoft Sentinel Validation
      │
      ▼
Incident Classification
      │
      ▼
Containment Decision
      │
      ▼
Investigation Notes
```

---

# Lab Activities

## Lab 1 — Microsoft Defender XDR Portal Familiarization

Introduced the Microsoft Defender XDR portal by exploring the security dashboard, navigation menu, incidents, alerts, and investigation capabilities available to enterprise security analysts.

**Key Areas Explored**

- Defender XDR Home
- Security Dashboard
- Navigation Pane
- Incidents & Alerts
- Devices
- Users

---

## Lab 2 — Incident Queue Investigation

Investigated the Defender XDR Incident Queue to understand how security incidents are prioritized, grouped, and assigned for investigation.

**Skills Practiced**

- Incident triage
- Alert prioritization
- Incident severity review
- Initial investigation

---

## Lab 3 — Incident Details Analysis

Performed a detailed review of a security incident by examining associated alerts, evidence, affected users, affected devices, and incident metadata.

**Skills Practiced**

- Alert correlation
- Incident evidence review
- Incident scoping
- Security analysis

---

## Lab 4 — User & Device Context Investigation

Investigated the affected endpoint and associated user account to better understand the context surrounding the security event.

**Skills Practiced**

- Device investigations
- User investigations
- Identity analysis
- Endpoint context review

---

## Lab 5 — Device Timeline Investigation

Reviewed endpoint telemetry through the Device Timeline to reconstruct system activity and identify suspicious processes, authentication events, and network activity.

**Skills Practiced**

- Timeline analysis
- Process investigations
- Endpoint telemetry
- Behavioral analysis

---

## Lab 6 — Advanced Hunting: Failed Logon Analysis

Performed Advanced Hunting using Kusto Query Language (KQL) to identify failed authentication attempts across the environment and detect abnormal logon patterns.

**Skills Practiced**

- KQL fundamentals
- Authentication investigations
- Failed logon hunting
- Security analytics

---

## Lab 7 — Advanced Hunting: PowerShell Investigation

Investigated suspicious PowerShell execution using KQL by searching for encoded commands, download activity, execution policy bypasses, and other indicators commonly associated with malicious PowerShell usage.

**Skills Practiced**

- PowerShell investigations
- Command-line analysis
- Threat hunting
- Behavioral detection

---

## Lab 8 — Microsoft Sentinel Incident Investigation

Examined Microsoft Sentinel's Incident Queue to compare SIEM-generated incidents with Defender XDR investigations and understand how both platforms complement one another.

**Skills Practiced**

- SIEM investigations
- Incident management
- Cloud security monitoring
- Log correlation

---

## Lab 9 — Microsoft Sentinel Log Validation

Validated investigation findings by querying Sentinel logs using KQL to review security incidents, incident status, severity, ownership, and provider information.

**Skills Practiced**

- Sentinel Logs
- KQL
- Cloud log analysis
- Incident validation

---

## Lab 10 — Endpoint Containment Evaluation

Reviewed the available endpoint response actions within Microsoft Defender XDR and evaluated when containment actions such as device isolation, antivirus scans, investigation package collection, or Live Response would be appropriate.

**Skills Practiced**

- Incident response
- Containment planning
- Endpoint response
- Decision-making

---

## Lab 11 — Investigation Documentation

Documented the investigation by summarizing the evidence collected, endpoint observations, user activity, KQL findings, and containment recommendations in a professional incident report.

**Skills Practiced**

- Incident documentation
- Technical writing
- Security reporting
- Investigation summaries

---

# KQL Queries

During this project, Kusto Query Language (KQL) was used to investigate security events across Microsoft Defender XDR and Microsoft Sentinel.

### Queries Included

- Failed Logon Investigation
- Suspicious PowerShell Investigation
- Security Incident Validation

Future additions will include:

- Process creation hunting
- Network connection hunting
- Registry modification hunting
- Persistence detection
- Privileged account monitoring
- Device event correlation

---

# Repository Structure

```text
Week17_Defender_XDR_Sentinel_EDR_Investigations/
│
├── README.md
│
├── Screenshots/
│
├── Writeup/
│
├── Case_Study/
│
├── KQL_Queries/
│
└── Notes/
```

---

# Key Concepts Learned

- Enterprise Endpoint Detection & Response (EDR)
- Microsoft Defender XDR investigations
- Microsoft Sentinel investigations
- Security incident triage
- Device investigations
- User investigations
- Timeline reconstruction
- Advanced Hunting with KQL
- SIEM and EDR correlation
- Incident classification
- Containment decision-making
- Enterprise investigation workflows

---

# Portfolio Value

This project demonstrates practical experience using Microsoft's enterprise security ecosystem to investigate security incidents from initial detection through validation and response.

The lab highlights skills commonly expected of Security Operations Center (SOC) analysts, Incident Responders, Threat Hunters, and Security Engineers by showcasing the ability to:

- Investigate Microsoft Defender XDR incidents
- Perform endpoint investigations
- Analyze user and device activity
- Conduct Advanced Hunting with KQL
- Validate findings using Microsoft Sentinel
- Correlate SIEM and EDR telemetry
- Document professional incident investigations
- Apply structured enterprise incident response workflows

---

# Future Enhancements

Future iterations of this project will expand into additional Microsoft security capabilities, including:

- Analytics Rules
- Custom Detection Rules
- Watchlists
- Data Connectors
- User and Entity Behavior Analytics (UEBA)
- MITRE ATT&CK Mapping
- Automated Response Playbooks
- Microsoft Sentinel Workbooks
- Threat Intelligence Integration
- Multi-stage Threat Hunting
- End-to-End Cloud Incident Response
````
