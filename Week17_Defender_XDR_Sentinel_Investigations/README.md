# Microsoft Defender XDR: End-to-End Enterprise EDR Investigation Lab

> A comprehensive Microsoft Defender XDR home lab demonstrating enterprise endpoint detection and response (EDR) workflows, advanced investigations, threat hunting, and incident response using Microsoft Defender XDR, Microsoft Defender for Endpoint, Microsoft Sentinel, and Kusto Query Language (KQL).

---

# Overview

Modern Security Operations Centers (SOCs) rely heavily on Endpoint Detection and Response (EDR) platforms to detect, investigate, contain, and respond to cyber threats. Unlike traditional SIEM platforms that primarily aggregate logs, EDR solutions provide rich endpoint telemetry, process visibility, behavioral analytics, and response capabilities.

This lab focuses on building practical experience with **Microsoft Defender XDR**, one of the industry's leading enterprise EDR platforms. Throughout this project, I investigated endpoint alerts, analyzed process trees, hunted for suspicious activity using KQL, reviewed device and user timelines, correlated threat intelligence, and documented incident response decisions.

The objective was to simulate how Security Analysts investigate and respond to endpoint threats within an enterprise environment.

---

# Objectives

- Understand the Microsoft Defender XDR interface
- Investigate endpoint security alerts
- Analyze process trees and process lineage
- Examine device timelines
- Investigate user activity
- Perform Advanced Hunting using KQL
- Investigate suspicious files
- Analyze malware behavior
- Review persistence mechanisms
- Investigate credential theft activity
- Correlate endpoint telemetry with threat intelligence
- Make containment and remediation decisions
- Document an enterprise incident investigation

---

# Technologies Used

- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Microsoft Sentinel
- Microsoft Entra ID
- Microsoft 365 Defender
- Kusto Query Language (KQL)
- Windows 11
- Microsoft Security Portal

---

# Skills Demonstrated

## Endpoint Detection and Response (EDR)

- Endpoint investigations
- Alert triage
- Incident response
- Threat hunting
- Behavioral analysis
- Process analysis
- Malware investigation
- Device investigations
- User investigations
- Evidence collection

---

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
- Live Response
- Threat Intelligence Correlation

---

## Threat Hunting

- KQL queries
- Endpoint telemetry analysis
- Process creation events
- Network connection events
- File creation events
- Registry modification events
- Persistence hunting
- PowerShell hunting
- Suspicious authentication hunting

---

## Incident Response

- Detection
- Validation
- Investigation
- Scoping
- Containment
- Eradication
- Recovery
- Lessons Learned

---

# Enterprise Investigation Workflow

```text
Alert Generated
       │
       ▼
Incident Queue Review
       │
       ▼
Alert Validation
       │
       ▼
Device Timeline Analysis
       │
       ▼
User Timeline Analysis
       │
       ▼
Process Tree Investigation
       │
       ▼
File Investigation
       │
       ▼
Threat Intelligence Correlation
       │
       ▼
Advanced Hunting (KQL)
       │
       ▼
Determine Scope
       │
       ▼
Containment Decision
       │
       ▼
Remediation
       │
       ▼
Incident Report
```

---

# Lab Scenarios

## Scenario 1 – Suspicious PowerShell Investigation

### Objectives

- Investigate suspicious PowerShell activity
- Identify encoded commands
- Analyze parent processes
- Review command-line arguments
- Determine attacker intent

### Investigation Tasks

- Review endpoint alert
- Examine process tree
- Analyze PowerShell command line
- Investigate child processes
- Review network connections
- Determine persistence attempts

### Skills Practiced

- PowerShell analysis
- Living-off-the-Land (LOLBins)
- Command-line analysis
- Process lineage
- Threat validation

---

## Scenario 2 – Malware Execution Investigation

### Objectives

- Investigate malicious executable activity
- Analyze execution timeline
- Review Defender detections
- Identify impacted devices

### Investigation Tasks

- Review malware alert
- Analyze file hash
- Review VirusTotal reputation
- Examine execution history
- Analyze child processes
- Review persistence artifacts

### Skills Practiced

- Malware triage
- File investigation
- Threat intelligence
- Device compromise analysis

---

## Scenario 3 – Credential Theft Investigation

### Objectives

- Investigate credential access activity
- Detect suspicious authentication
- Review lateral movement indicators

### Investigation Tasks

- Review login events
- Analyze authentication logs
- Investigate LSASS access
- Review suspicious PowerShell
- Analyze remote connections

### Skills Practiced

- Credential theft detection
- Authentication investigations
- Lateral movement analysis

---

## Scenario 4 – Persistence Investigation

### Objectives

- Detect persistence mechanisms
- Identify attacker survival techniques

### Investigation Tasks

- Registry analysis
- Scheduled task investigation
- Startup folder review
- Service creation analysis
- Autorun investigation

### Skills Practiced

- Persistence analysis
- Windows internals
- Registry investigations

---

## Scenario 5 – Device Compromise Investigation

### Objectives

Perform a complete endpoint investigation from initial alert through containment.

### Investigation Tasks

- Review incident
- Analyze alerts
- Investigate device timeline
- Investigate user timeline
- Analyze process trees
- Hunt for related activity
- Determine blast radius
- Recommend containment

---

## Scenario 6 – End-to-End Defender XDR Investigation

This project combines every previous investigation into one complete enterprise case study.

The investigation follows the same workflow used by modern Security Operations Centers.

### Workflow

- Initial alert review
- Incident validation
- Scope determination
- Device investigation
- User investigation
- Process analysis
- Threat hunting
- Containment decision
- Recovery
- Incident documentation

---

# Advanced Hunting (KQL)

Throughout this project I performed Advanced Hunting using Kusto Query Language (KQL) to search enterprise telemetry for suspicious activity.

Example hunting categories included:

- Suspicious PowerShell execution
- Encoded PowerShell commands
- Process creation events
- Network connections
- File creation
- Registry changes
- User logins
- Device events
- Malware detections
- Persistence indicators
- Scheduled tasks
- Service creation

---

# Key Defender XDR Components Explored

## Incident Queue

- Incident prioritization
- Alert correlation
- Severity assessment
- Incident ownership

---

## Device Timeline

Reviewed endpoint telemetry including:

- Process execution
- Network connections
- Registry changes
- File creation
- User logins
- Defender detections

---

## User Timeline

Investigated:

- Authentication activity
- Device access
- Account usage
- Privilege changes
- Suspicious behavior

---

## Process Trees

Analyzed:

- Parent-child relationships
- Suspicious spawning
- Process lineage
- LOLBins
- Malicious execution chains

---

## File Investigation

Examined:

- SHA-256 hashes
- File reputation
- Digital signatures
- Detection history
- Execution history

---

## Threat Intelligence

Correlated endpoint evidence with:

- Known malicious hashes
- Indicators of Compromise (IOCs)
- Threat intelligence feeds
- Microsoft threat intelligence

---

## Device Isolation

Evaluated when endpoint isolation would be appropriate to prevent lateral movement while preserving forensic evidence.

---

# Key Concepts Learned

- Enterprise EDR operations
- Endpoint telemetry analysis
- Incident prioritization
- Threat hunting methodology
- Behavioral detection
- Process analysis
- Endpoint containment
- Threat intelligence integration
- Enterprise investigation workflow
- Security operations best practices

---

# Project Outcomes

Through this lab I developed hands-on experience using Microsoft Defender XDR to investigate enterprise endpoint incidents from initial alert through containment.

This project strengthened my understanding of:

- Enterprise Security Operations
- Endpoint Detection and Response (EDR)
- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Microsoft Sentinel integration
- Advanced Hunting with KQL
- Process tree investigations
- Device and user timeline analysis
- Malware investigations
- Threat intelligence correlation
- Incident response workflows

---

# Future Improvements

Future enhancements to this lab include:

- Automated response playbooks
- Microsoft Sentinel analytics rules
- SOAR automation
- Attack simulation
- MITRE ATT&CK mapping
- Live Response investigations
- Threat Intelligence enrichment
- Advanced KQL detection engineering
- Custom detection rules
- Multi-device incident investigations

---

# MITRE ATT&CK Coverage

This lab demonstrates investigations across numerous MITRE ATT&CK techniques including:

- Initial Access
- Execution
- Persistence
- Privilege Escalation
- Defense Evasion
- Credential Access
- Discovery
- Lateral Movement
- Collection
- Command and Control
- Impact

---

# Portfolio Value

This project demonstrates practical experience with an enterprise-grade EDR platform and mirrors investigative workflows performed by Security Operations Center (SOC) analysts.

The repository showcases the ability to:

- Investigate endpoint alerts
- Analyze process execution
- Hunt threats using KQL
- Correlate endpoint telemetry
- Perform enterprise incident response
- Document investigations professionally
- Apply security operations methodologies
- Work with Microsoft Defender XDR in realistic enterprise scenarios

---

# Repository Structure

```
Microsoft-Defender-XDR-Lab/
│
├── README.md
├── Screenshots/
├── Investigation-Notes/
├── KQL-Queries/
├── Case-Studies/
├── Incident-Reports/
├── Threat-Hunting/
├── MITRE-Mapping/
└── Evidence/
```

---

# Author

**Fitzgerald Afari-Minta**

Information Systems Student | Cybersecurity

Focused on:

- Security Operations (SOC)
- Incident Response
- Threat Hunting
- Endpoint Detection & Response (EDR)
- Microsoft Defender XDR
- Microsoft Sentinel
- SIEM Engineering
- Cloud Security
- Security Engineering

---

*This project was created for educational purposes to develop practical skills in enterprise endpoint detection, threat hunting, and incident response using Microsoft Defender XDR.*
````
