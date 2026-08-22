# Week 17 — Microsoft Defender XDR & Sentinel Security Operations

> **Focus:** Microsoft Defender XDR • Defender for Endpoint • Microsoft Sentinel • KQL • EDR Investigation • Threat Hunting • Incident Response

## Overview

Week 17 focuses on building and operating a Microsoft-based Security Operations environment using **Microsoft Defender XDR, Microsoft Defender for Endpoint, Microsoft Sentinel, Microsoft Entra ID, Azure Log Analytics, and Kusto Query Language (KQL)**.

Rather than working only with preconfigured security tools, this week begins by building the underlying cloud security environment required to support endpoint detection, telemetry collection, threat hunting, alert investigation, and incident response.

The environment connects a monitored Windows endpoint to Microsoft Defender for Endpoint and Microsoft Defender XDR while integrating Microsoft Sentinel and a dedicated Log Analytics workspace for centralized security operations.

The overall architecture follows this security telemetry path:

```text
Windows 11 Endpoint
        │
        │ Endpoint Security Telemetry
        ▼
Microsoft Defender for Endpoint
        │
        │ EDR Detection & Investigation
        ▼
Microsoft Defender XDR
        │
        │ Alerts / Incidents / Hunting Data
        ▼
Microsoft Sentinel
        │
        ▼
Log Analytics Workspace
        │
        ▼
KQL Hunting & SOC Investigation
```

The labs then move beyond deployment into practical SOC operations, including endpoint onboarding, EDR alert validation, device timeline analysis, process-tree investigation, Advanced Hunting with KQL, incident classification, and secure post-investigation cleanup.

---

# Week 17 Objectives

The primary objectives of Week 17 are to:

- Build a functional Microsoft cloud security lab
- Understand the relationship between SIEM and XDR technologies
- Configure Microsoft Sentinel and Log Analytics
- Configure Microsoft Defender XDR and Defender for Endpoint
- Onboard and validate a Windows endpoint
- Verify endpoint telemetry collection
- Generate controlled security test activity
- Investigate Defender alerts and incidents
- Analyze endpoint process execution and parent-child relationships
- Use device timelines to reconstruct activity
- Hunt endpoint telemetry using KQL
- Correlate endpoint evidence across multiple security views
- Determine incident scope and severity
- Practice SOC alert triage and incident classification
- Evaluate containment and remediation options
- Document investigation findings and evidence
- Perform secure post-investigation cleanup

---

# Security Operations Architecture

## Cloud Security Layer

```text
Microsoft Entra ID
        │
        ├── Identity & Administrative Access
        │
        ▼
Azure Subscription
        │
        ▼
Resource Group
RG-Microsoft-Security-Lab
        │
        ▼
Log Analytics Workspace
LAW-Microsoft-Security-Lab
        │
        ▼
Microsoft Sentinel
```

This portion of the environment provides the identity, cloud-resource, log-management, and SIEM foundation for the lab.

## Endpoint Security Layer

```text
Windows 11 Enterprise VM
        │
        ▼
Microsoft Defender for Endpoint Sensor
        │
        ▼
Endpoint Telemetry
        │
        ▼
Microsoft Defender XDR
        │
        ├── Alerts
        ├── Incidents
        ├── Device Inventory
        ├── Device Timeline
        ├── Process Evidence
        └── Advanced Hunting
```

## Investigation Layer

```text
Security Activity
       │
       ▼
EDR Detection
       │
       ▼
Defender Alert
       │
       ▼
Incident Correlation
       │
       ▼
Device Investigation
       │
       ├── Timeline Analysis
       ├── Process Analysis
       ├── File Evidence
       └── User Context
       │
       ▼
Advanced Hunting (KQL)
       │
       ▼
Scope & Impact Analysis
       │
       ▼
Classification
       │
       ▼
Containment / Remediation Decision
       │
       ▼
Incident Resolution
```

---

# Technologies & Platforms

| Technology | Purpose |
|---|---|
| **Microsoft Defender XDR** | Centralized detection, investigation, incident correlation, and response |
| **Microsoft Defender for Endpoint** | Endpoint Detection and Response (EDR) and endpoint telemetry |
| **Microsoft Sentinel** | Cloud-native SIEM and security operations platform |
| **Microsoft Entra ID** | Identity, tenant, user, and administrative access management |
| **Azure Log Analytics** | Centralized log storage and query workspace |
| **Microsoft Azure** | Cloud infrastructure supporting Sentinel and Log Analytics |
| **Windows 11 Enterprise** | Monitored enterprise endpoint |
| **Kusto Query Language (KQL)** | Threat hunting and security telemetry analysis |
| **PowerShell** | Endpoint administration, validation, and controlled test activity |
| **Oracle VirtualBox** | Virtualized endpoint lab environment |

---

# Core Skills Demonstrated

## Microsoft Defender XDR

- Incident queue investigation
- Alert triage
- Alert evidence review
- Incident correlation
- Device inventory analysis
- Device investigation
- Device timeline analysis
- Process-tree analysis
- File and process investigation
- User and device context analysis
- Advanced Hunting
- Incident classification
- Investigation documentation

## Endpoint Detection & Response

- Windows endpoint onboarding
- Defender sensor validation
- Endpoint telemetry verification
- EDR alert validation
- Process execution analysis
- Parent-child process analysis
- Command-line investigation
- Endpoint timeline reconstruction
- Detection-source analysis
- Endpoint scoping
- Response decision-making

## Microsoft Sentinel / SIEM

- Sentinel workspace deployment
- Log Analytics configuration
- Defender XDR integration
- Security telemetry validation
- Incident review
- Alert correlation
- KQL-based investigation
- SIEM/XDR workflow analysis

## Threat Hunting

- Advanced Hunting
- KQL query construction
- `DeviceProcessEvents` analysis
- Time-based filtering
- Device-specific filtering
- Process-name filtering
- Command-line filtering
- Field projection
- Chronological event analysis
- Endpoint telemetry correlation

## Incident Response

- Detection
- Triage
- Validation
- Investigation
- Scoping
- Evidence collection
- Classification
- Containment evaluation
- Remediation
- Secure cleanup
- Incident resolution
- Documentation

## Cloud & Identity Administration

- Microsoft Entra tenant configuration
- Organizational security account configuration
- Azure resource management
- Resource-group deployment
- Log Analytics workspace deployment
- Microsoft Sentinel onboarding
- Defender licensing validation
- Security service provisioning

## Troubleshooting

- Azure tenant and identity troubleshooting
- Microsoft Defender licensing validation
- Endpoint onboarding troubleshooting
- Defender SENSE service troubleshooting
- Windows edition compatibility validation
- Endpoint service verification
- Telemetry-flow validation
- Cloud security integration troubleshooting

---

# Lab 01 — Microsoft Security Environment Setup

## Objective

Build the Microsoft security infrastructure required for later Defender XDR and Sentinel investigations.

This lab establishes the complete environment rather than assuming that the SIEM and EDR platforms are already configured.

The implementation includes:

1. Preparing the Windows endpoint
2. Establishing the Microsoft Entra environment
3. Configuring Azure resources
4. Creating a dedicated Log Analytics workspace
5. Enabling Microsoft Sentinel
6. Configuring Microsoft Defender for Endpoint
7. Onboarding a Windows 11 Enterprise endpoint
8. Verifying Defender services
9. Confirming the endpoint in Device Inventory
10. Generating controlled detection activity
11. Investigating the resulting EDR alert
12. Hunting the underlying endpoint telemetry with KQL
13. Reviewing incident context
14. Classifying the activity as authorized security testing
15. Removing temporary test infrastructure

---

# Lab Environment

## Azure Resources

```text
Azure Subscription
└── RG-Microsoft-Security-Lab
    └── LAW-Microsoft-Security-Lab
        └── Microsoft Sentinel
```

A dedicated resource group and Log Analytics workspace were used to keep the security lab logically separated from unrelated Azure resources.

---

# Endpoint Configuration

A Windows 11 Enterprise virtual machine was configured as the monitored enterprise endpoint.

The endpoint was prepared with:

- Internet connectivity to Microsoft cloud services
- Microsoft Defender Antivirus
- Microsoft Defender for Endpoint
- Defender EDR sensor functionality
- Endpoint telemetry reporting
- Microsoft Defender XDR connectivity

After onboarding, the device became visible through Microsoft Defender's device inventory and could be investigated using Defender's endpoint security capabilities.

---

# Endpoint Onboarding & Troubleshooting

One of the most valuable portions of this lab involved troubleshooting endpoint onboarding rather than simply following a successful deployment path.

During the initial onboarding attempt, the Microsoft Defender for Endpoint **SENSE service was unavailable**, preventing the endpoint from completing the expected onboarding workflow.

Troubleshooting included:

```powershell
sc query sense
```

Windows edition validation:

```powershell
DISM /Online /Get-CurrentEdition
```

Defender for Endpoint capability inspection:

```powershell
DISM.EXE /Online /Get-CapabilityInfo /CapabilityName:Microsoft.Windows.Sense.Client~~~~
```

This troubleshooting process demonstrated an important operational skill:

> Security engineering requires validating dependencies, licensing, operating-system support, service state, and telemetry—not simply rerunning failed installation commands.

After resolving the endpoint requirements, Defender services were successfully validated and the endpoint was able to report security telemetry.

---

# Controlled Detection Test

After the endpoint was successfully onboarded, controlled Microsoft Defender security test activity was generated to validate the detection pipeline.

The purpose was not to simulate an uncontrolled compromise, but to verify that:

```text
Endpoint Activity
      ↓
Defender for Endpoint Sensor
      ↓
Defender Cloud Analytics
      ↓
EDR Detection
      ↓
Security Alert
      ↓
Defender XDR Investigation
```

The resulting investigation included a **Suspicious PowerShell command line** alert with EDR telemetry associated with the monitored endpoint.

This confirmed that the environment could detect endpoint activity and surface it to the analyst through Microsoft Defender XDR.

---

# Alert Investigation

The alert investigation examined:

- Alert title
- Severity
- Detection source
- Affected endpoint
- Associated user
- Timestamp
- Process execution
- Command-line activity
- Supporting endpoint evidence
- Related incident context

Rather than treating the alert itself as proof of compromise, the investigation pivoted into the underlying endpoint telemetry.

---

# Process Tree Analysis

Process relationships were examined to understand how the detected activity executed on the endpoint.

The observed process chain included:

```text
userinit.exe
    │
    ▼
explorer.exe
    │
    ▼
cmd.exe
    │
    ▼
powershell.exe
```

Analyzing the process tree provides context that cannot be obtained from an alert title alone.

The process lineage helped establish:

- Which process initiated the activity
- The user execution context
- How PowerShell was launched
- Whether the execution chain matched the controlled test
- Whether unexpected parent or child processes were present

---

# Device Timeline Investigation

The Defender device timeline was used to reconstruct activity around the detection.

Timeline analysis focused on:

- Process execution
- PowerShell activity
- File activity
- Detection events
- Alert associations
- User context
- Event timestamps

This allowed the investigation to move from:

```text
"What alert fired?"
```

to:

```text
"What actually happened on the endpoint?"
```

That distinction is fundamental to EDR investigation.

---

# Advanced Hunting with KQL

Advanced Hunting was used to independently search the endpoint telemetry supporting the detection.

Example investigation query:

```kusto
DeviceProcessEvents
| where Timestamp > ago(2h)
| where DeviceName =~ "<LAB-ENDPOINT>"
| where FileName in~ ("powershell.exe", "invoice.exe")
    or ProcessCommandLine contains "test-MDATP-test"
| project
    Timestamp,
    DeviceName,
    AccountName,
    FileName,
    ProcessCommandLine,
    InitiatingProcessFileName,
    InitiatingProcessCommandLine
| order by Timestamp desc
```

## Query Purpose

The query:

- Searches endpoint process telemetry using `DeviceProcessEvents`
- Restricts results to the investigation time window
- Filters activity to the affected endpoint
- Searches for processes associated with the controlled detection test
- Returns command-line and initiating-process context
- Orders events chronologically for timeline reconstruction

The hunt returned multiple relevant endpoint events associated with the controlled test activity.

This independently validated that the alert was supported by underlying endpoint telemetry.

---

# Evidence Correlation

The investigation correlated multiple evidence sources rather than relying on a single security alert.

```text
Defender Alert
      │
      ├──────────────┐
      ▼              ▼
Process Tree     Device Timeline
      │              │
      └──────┬───────┘
             ▼
      Advanced Hunting
             │
             ▼
       KQL Telemetry
             │
             ▼
      Incident Context
             │
             ▼
      Analyst Decision
```

This workflow mirrors an important SOC principle:

> An alert is the beginning of an investigation—not the conclusion.

---

# Investigation Findings

The investigation established that:

- The Windows 11 Enterprise endpoint successfully reported security telemetry.
- Microsoft Defender for Endpoint detected the controlled activity.
- Microsoft Defender XDR generated an EDR alert.
- The alert was associated with the correct endpoint and user context.
- The process tree showed the execution chain leading to PowerShell.
- Device telemetry preserved the underlying endpoint activity.
- Advanced Hunting returned multiple relevant process events.
- KQL provided independent validation of the endpoint evidence.
- The observed behavior was consistent with the authorized Microsoft Defender security test.
- No evidence indicated an actual unauthorized compromise.

---

# Incident Classification

Because the activity was intentionally generated as part of an authorized security validation exercise, the investigation was documented accordingly.

```text
Detection Result: Valid Detection
Activity Type: Authorized Security Testing
Compromise: No
Analyst Disposition: Expected / Security Testing
```

This distinction is important.

A security detection can be technically valid even when the underlying behavior was intentionally generated.

Correctly documenting the event preserves the effectiveness of the detection while preventing authorized testing from being incorrectly recorded as a genuine compromise.

---

# Secure Cleanup

Temporary components used to support the controlled test were removed after evidence collection.

Cleanup included:

- Stopping the temporary IIS web service
- Removing temporary test files
- Removing the temporary test directory
- Disabling the temporary IIS role
- Verifying that TCP port 80 was no longer listening

Example verification:

```powershell
Test-NetConnection 127.0.0.1 -Port 80
```

Expected post-cleanup result:

```text
TcpTestSucceeded : False
```

Cleanup is part of the investigation lifecycle because temporary testing infrastructure should not remain exposed after the exercise is complete.

---

# SOC Investigation Methodology

The methodology practiced throughout this lab can be summarized as:

```text
1. Detect
      ↓
2. Validate
      ↓
3. Establish Context
      ↓
4. Investigate Endpoint
      ↓
5. Analyze Process Lineage
      ↓
6. Review Timeline
      ↓
7. Hunt Telemetry with KQL
      ↓
8. Correlate Evidence
      ↓
9. Determine Scope
      ↓
10. Classify Activity
      ↓
11. Evaluate Response
      ↓
12. Remediate / Clean Up
      ↓
13. Document & Resolve
```

---

# SIEM vs. XDR — Practical Takeaway

This lab also demonstrated the complementary roles of SIEM and XDR.

## Microsoft Sentinel — SIEM

Provides centralized capabilities for:

- Security data collection
- Log analysis
- Cross-source correlation
- KQL investigations
- Incident management
- Threat detection
- Security operations workflows

## Microsoft Defender XDR — XDR

Provides deep security context across protected Microsoft security domains, including:

- Endpoint telemetry
- EDR alerts
- Device investigations
- Process relationships
- Incident correlation
- Advanced Hunting
- Response actions

Together, these technologies allow an analyst to move between high-level security monitoring and detailed endpoint investigation.

---

# Key Takeaways

### 1. Building the environment is part of security engineering

A functioning EDR/SIEM environment depends on identity, licensing, endpoints, cloud resources, sensors, connectors, and telemetry pipelines all working correctly.

### 2. Alerts require validation

An alert indicates suspicious or security-relevant activity. It does not automatically prove malicious intent or compromise.

### 3. Process lineage provides critical context

Understanding parent-child process relationships can reveal how suspicious activity began and how execution progressed.

### 4. Endpoint telemetry provides deeper visibility

EDR enables analysts to investigate the behavior behind an alert rather than relying only on summarized detection information.

### 5. KQL enables independent validation

Advanced Hunting allows analysts to query raw security telemetry and validate or challenge conclusions suggested by alerts.

### 6. SIEM and XDR are complementary

Microsoft Sentinel provides broad SIEM visibility while Defender XDR provides rich detection and investigation context.

### 7. Troubleshooting is a security skill

Diagnosing endpoint onboarding, licensing, services, permissions, and telemetry issues is directly relevant to real security operations and engineering work.

### 8. Incident classification matters

Valid security detections generated through authorized testing should be documented accurately rather than simply labeled false positives.

### 9. Cleanup is part of the lifecycle

Temporary services and testing artifacts should be removed once they are no longer required.

---

# Evidence Collected

Portfolio evidence for this lab includes screenshots documenting:

- Azure security environment configuration
- Microsoft Entra tenant configuration
- Azure resource group
- Log Analytics workspace deployment
- Microsoft Sentinel enablement
- Microsoft Defender XDR configuration
- Defender for Endpoint onboarding
- Windows endpoint configuration
- Defender service validation
- Device inventory
- Controlled detection test
- EDR alert investigation
- Process-tree evidence
- Device timeline telemetry
- Advanced Hunting query and results
- Incident classification
- Post-investigation cleanup

Sensitive identifiers, credentials, subscription information, tenant information, and private account details are excluded or redacted from public evidence.

---

# MITRE ATT&CK-Relevant Investigation Concepts

Although the activity in this lab was authorized security testing rather than a real intrusion, the investigation exercised analysis techniques relevant to behaviors commonly encountered during real incidents, particularly:

- **Command and Scripting Interpreter**
- **PowerShell**
- **Process Execution**
- **User Execution**
- **Endpoint telemetry analysis**

MITRE ATT&CK mappings should always be based on the behavior actually observed rather than assigned solely because an alert fired.

---

# Professional Skills Demonstrated

This lab demonstrates practical experience in:

**Security Operations**
- SOC alert triage
- Incident investigation
- Evidence correlation
- Incident classification
- Investigation documentation

**Endpoint Security**
- Microsoft Defender for Endpoint
- EDR telemetry analysis
- Device investigation
- Process analysis
- Endpoint onboarding

**Threat Hunting**
- Microsoft Defender Advanced Hunting
- Kusto Query Language
- Process telemetry investigation
- Command-line analysis
- Timeline reconstruction

**Cloud Security**
- Microsoft Azure
- Microsoft Entra ID
- Microsoft Sentinel
- Log Analytics
- Cloud security architecture

**Incident Response**
- Detection validation
- Investigation
- Scoping
- Response evaluation
- Remediation
- Secure cleanup

**Security Engineering**
- Security platform deployment
- Endpoint integration
- Service validation
- Licensing and dependency troubleshooting
- Telemetry-pipeline validation

---

# Week 17 Outcome

By completing this environment and investigation workflow, I developed hands-on experience with the technologies and investigative processes used in Microsoft-focused Security Operations environments.

The week progressed from:

```text
Building the Security Environment
            ↓
Onboarding an Enterprise Endpoint
            ↓
Validating EDR Telemetry
            ↓
Generating Controlled Security Activity
            ↓
Investigating the Detection
            ↓
Analyzing Endpoint Evidence
            ↓
Hunting with KQL
            ↓
Correlating SIEM + XDR Context
            ↓
Classifying the Incident
            ↓
Performing Secure Cleanup
```

The result is a functional Microsoft security lab that can support additional exercises involving **endpoint detection, threat hunting, incident investigation, KQL analytics, Microsoft Sentinel, and Defender XDR**.
