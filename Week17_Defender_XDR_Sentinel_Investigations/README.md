# Week 17 — Microsoft Defender XDR & Sentinel Security Operations

> **Focus:** Microsoft Defender XDR • Microsoft Defender for Endpoint • Microsoft Sentinel • Microsoft Entra ID • Azure Log Analytics • Kusto Query Language (KQL) • Endpoint Detection and Response (EDR) • Security Information and Event Management (SIEM) • Extended Detection and Response (XDR) • Threat Hunting • Incident Investigation • Incident Response

---

## Overview

Week 17 focuses on building, validating, and operating a Microsoft-based Security Operations environment using **Microsoft Defender XDR, Microsoft Defender for Endpoint, Microsoft Sentinel, Microsoft Entra ID, Azure Log Analytics, and Kusto Query Language (KQL)**.

Rather than beginning with an already configured security platform, the week starts by building the cloud and endpoint infrastructure required to support security monitoring. The environment is then used to generate controlled security activity, validate endpoint telemetry, investigate Microsoft Defender detections, hunt security data with KQL, and practice the investigation workflow used within Microsoft Defender XDR.

Week 17 consists of two complementary labs:

### Lab 01 — Microsoft Security Environment Setup & EDR Investigation

Lab 01 focuses on building the underlying Microsoft security environment. This includes configuring Azure resources, creating a Log Analytics workspace, enabling Microsoft Sentinel, configuring Microsoft Defender for Endpoint, onboarding a Windows 11 Enterprise endpoint, validating Defender services and telemetry, generating controlled security activity, investigating the resulting EDR detection, hunting the supporting endpoint telemetry with KQL, classifying the activity, and securely cleaning up temporary test infrastructure.

### Lab 02 — Microsoft Defender XDR Fundamentals

Lab 02 expands from the investigation of a single controlled detection into the broader Microsoft Defender XDR Security Operations workflow. The lab focuses on the Defender XDR dashboard, device inventory, incident queue, incident correlation, alert investigation, evidence and entity analysis, device investigation, Device Timeline analysis, process lineage, user investigation, User Timeline analysis, incident lifecycle management, and endpoint response capabilities.

Together, the labs progress from **building the security platform** to **operating the platform as a security analyst**.

```text
Build Microsoft Security Environment
                │
                ▼
Configure Sentinel & Log Analytics
                │
                ▼
Configure Defender for Endpoint
                │
                ▼
Onboard Windows Endpoint
                │
                ▼
Validate Endpoint Telemetry
                │
                ▼
Generate Controlled Security Activity
                │
                ▼
Investigate Defender Detection
                │
                ▼
Hunt Supporting Telemetry with KQL
                │
                ▼
Understand XDR Incident Correlation
                │
                ▼
Investigate Alerts & Evidence
                │
                ▼
Pivot Across Devices & Users
                │
                ▼
Reconstruct Activity with Timelines
                │
                ▼
Analyze Process Lineage
                │
                ▼
Determine Scope & Verdict
                │
                ▼
Evaluate Response Options
                │
                ▼
Classify, Document & Resolve
```

---

# Week 17 Objectives

The primary objectives of Week 17 were to:

- Build a functional Microsoft cloud security lab
- Understand the relationship between SIEM, EDR, and XDR technologies
- Configure Microsoft Sentinel and Azure Log Analytics
- Configure Microsoft Defender XDR and Microsoft Defender for Endpoint
- Establish the Microsoft Entra ID identity environment supporting the lab
- Onboard and validate a Windows 11 Enterprise endpoint
- Verify Microsoft Defender for Endpoint sensor functionality
- Verify endpoint security telemetry collection
- Troubleshoot endpoint onboarding and service dependencies
- Generate controlled security test activity
- Validate the endpoint detection pipeline
- Investigate Microsoft Defender alerts
- Investigate Microsoft Defender XDR incidents
- Understand the difference between an alert and an incident
- Navigate the Microsoft Defender XDR Security Operations interface
- Review and prioritize incidents from the incident queue
- Analyze incident attack stories
- Investigate evidence and security-relevant entities
- Pivot between incidents, alerts, devices, users, processes, and other evidence
- Investigate affected endpoints through device entity pages
- Reconstruct endpoint behavior using Device Timeline
- Analyze process execution and parent-child process relationships
- Understand process lineage and process trees
- Investigate identity context through user entity pages
- Review chronological identity activity using User Timeline
- Use Microsoft Defender Advanced Hunting
- Use KQL to query endpoint telemetry
- Correlate evidence across multiple security views
- Determine incident context and scope
- Practice SOC alert triage
- Practice incident classification
- Understand incident lifecycle management
- Evaluate endpoint containment and response capabilities
- Document investigation findings
- Collect portfolio-quality investigation evidence
- Perform secure post-investigation cleanup

---

# Security Operations Architecture

## Cloud Security Layer

```text
Microsoft Entra ID
        │
        ├── Identity
        ├── Users
        └── Administrative Access
        │
        ▼
Microsoft Azure Subscription
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

This portion of the environment provides the identity, cloud-resource, centralized logging, and SIEM foundation supporting the security lab.

---

## Endpoint Security Layer

```text
Windows 11 Enterprise VM
        │
        ▼
Microsoft Defender for Endpoint
        │
        ▼
Endpoint Security Telemetry
        │
        ▼
Microsoft Defender XDR
        │
        ├── Alerts
        ├── Incidents
        ├── Evidence
        ├── Entities
        ├── Device Inventory
        ├── Device Timeline
        ├── Process Evidence
        ├── User Entities
        ├── Advanced Hunting
        └── Response Actions
```

The endpoint layer provides the detailed host telemetry required to investigate processes, users, files, command lines, detections, and other security-relevant activity.

---

## Investigation Layer

```text
Security Activity
       │
       ▼
Endpoint Telemetry
       │
       ▼
Detection Logic
       │
       ▼
Security Alert
       │
       ▼
Incident Correlation
       │
       ▼
Incident Queue
       │
       ▼
Analyst Triage
       │
       ├───────────────┬────────────────┐
       ▼               ▼                ▼
     Alerts         Evidence          Assets
                       │
              ┌────────┴────────┐
              ▼                 ▼
            Device             User
              │                 │
              ▼                 ▼
      Device Timeline      User Timeline
              │
              ▼
       Process Lineage
              │
              ▼
      Advanced Hunting
              │
              ▼
       KQL Telemetry
              │
              ▼
      Evidence Correlation
              │
              ▼
       Scope & Verdict
              │
              ▼
       Response Decision
              │
              ▼
 Classification / Resolution
```

---

# Technologies & Platforms

| Technology | Purpose |
|---|---|
| **Microsoft Defender XDR** | Unified detection, incident correlation, investigation, entity analysis, and response |
| **Microsoft Defender for Endpoint** | Endpoint Detection and Response, endpoint telemetry, device investigation, and endpoint response |
| **Microsoft Sentinel** | Cloud-native Security Information and Event Management and security operations platform |
| **Microsoft Entra ID** | Identity, tenant, user, and administrative access management |
| **Azure Log Analytics** | Centralized security log storage and query workspace |
| **Microsoft Azure** | Cloud infrastructure supporting Microsoft Sentinel and Log Analytics |
| **Windows 11 Enterprise** | Monitored enterprise endpoint used for endpoint security investigation |
| **Kusto Query Language (KQL)** | Security telemetry querying, threat hunting, filtering, and investigation |
| **Microsoft Defender Advanced Hunting** | Query interface for investigating Defender security telemetry |
| **PowerShell** | Endpoint administration, validation, troubleshooting, and controlled test activity |
| **Oracle VirtualBox** | Virtualization platform supporting the Windows lab endpoint |

---

# Core Skills Demonstrated

## Microsoft Defender XDR

- Defender XDR portal navigation
- Security Operations dashboard review
- Incident queue analysis
- Incident prioritization
- Alert triage
- Alert investigation
- Alert and incident differentiation
- Incident correlation analysis
- Attack-story investigation
- Evidence review
- Entity analysis
- Device investigation
- User investigation
- Device Timeline analysis
- User Timeline analysis
- Process lineage analysis
- Process-tree analysis
- Advanced Hunting
- Incident lifecycle management
- Incident classification
- Response-action evaluation
- Investigation documentation

## Endpoint Detection & Response

- Windows endpoint onboarding
- Microsoft Defender for Endpoint configuration
- Defender sensor validation
- Endpoint telemetry verification
- EDR alert validation
- Process execution analysis
- Parent-child process analysis
- Command-line investigation
- Process lineage analysis
- Endpoint timeline reconstruction
- Detection-source analysis
- Endpoint scoping
- Response decision-making
- Device isolation awareness
- Antivirus scan awareness
- Investigation-package awareness
- Live Response awareness

## Microsoft Sentinel / SIEM

- Microsoft Sentinel workspace deployment
- Azure Log Analytics configuration
- Defender XDR integration
- Security telemetry validation
- Incident review
- Alert correlation
- KQL-based investigation
- SIEM and XDR workflow analysis

## Threat Hunting

- Microsoft Defender Advanced Hunting
- KQL query construction
- `DeviceProcessEvents` analysis
- Time-based filtering
- Device-specific filtering
- Process-name filtering
- Command-line filtering
- Field projection
- Chronological event analysis
- Endpoint telemetry correlation
- Hypothesis-driven investigation
- Evidence validation

## Incident Response

- Detection
- Triage
- Validation
- Context development
- Investigation
- Evidence collection
- Scoping
- Classification
- Containment evaluation
- Remediation evaluation
- Secure cleanup
- Incident resolution
- Documentation

## Cloud & Identity Administration

- Microsoft Entra ID tenant configuration
- Organizational security account configuration
- Microsoft Azure resource management
- Resource-group deployment
- Log Analytics workspace deployment
- Microsoft Sentinel onboarding
- Defender licensing validation
- Security-service provisioning
- User and identity investigation

## Troubleshooting

- Azure tenant troubleshooting
- Microsoft Entra ID troubleshooting
- Microsoft Defender licensing validation
- Endpoint onboarding troubleshooting
- Defender SENSE service troubleshooting
- Windows edition compatibility validation
- Endpoint service verification
- Defender capability validation
- Telemetry-flow validation
- Cloud security integration troubleshooting

---

# Lab 01 — Microsoft Security Environment Setup & EDR Investigation

## Objective

Build the Microsoft security infrastructure required for later Defender XDR and Sentinel investigations and validate the environment through a controlled endpoint-security exercise.

This lab establishes the security environment rather than assuming the SIEM and EDR platforms are already configured.

The implementation included:

1. Preparing the Windows endpoint
2. Establishing the Microsoft Entra ID environment
3. Configuring Microsoft Azure resources
4. Creating a dedicated Azure resource group
5. Creating a dedicated Log Analytics workspace
6. Enabling Microsoft Sentinel
7. Configuring Microsoft Defender XDR
8. Configuring Microsoft Defender for Endpoint
9. Onboarding a Windows 11 Enterprise endpoint
10. Verifying Microsoft Defender services
11. Confirming the endpoint in Device Inventory
12. Generating controlled security test activity
13. Investigating the resulting EDR alert
14. Reviewing endpoint process execution
15. Analyzing process lineage
16. Reviewing the Device Timeline
17. Hunting the underlying endpoint telemetry with KQL
18. Correlating multiple evidence sources
19. Reviewing incident context
20. Classifying the activity as authorized security testing
21. Removing temporary test infrastructure
22. Verifying successful cleanup

---

# Lab 01 Environment

## Azure Resources

```text
Azure Subscription
└── RG-Microsoft-Security-Lab
    └── LAW-Microsoft-Security-Lab
        └── Microsoft Sentinel
```

A dedicated resource group and Log Analytics workspace were used to keep the security lab logically separated from unrelated Azure resources.

The environment created a foundation for collecting, analyzing, and correlating security telemetry throughout later exercises.

---

# Endpoint Configuration

A **Windows 11 Enterprise virtual machine** was configured as the monitored enterprise endpoint.

The endpoint was prepared with:

- Internet connectivity to Microsoft cloud services
- Microsoft Defender Antivirus
- Microsoft Defender for Endpoint
- Defender EDR sensor functionality
- Endpoint telemetry reporting
- Microsoft Defender XDR connectivity

After successful onboarding, the endpoint became visible through Microsoft Defender Device Inventory and could be investigated using Defender's endpoint-security capabilities.

The endpoint represented the host being monitored by the Security Operations environment.

```text
Windows 11 Enterprise
        │
        ▼
Defender for Endpoint Sensor
        │
        ▼
Endpoint Telemetry
        │
        ▼
Microsoft Defender XDR
```

---

# Endpoint Onboarding & Troubleshooting

One of the most valuable portions of Lab 01 involved troubleshooting endpoint onboarding rather than simply following a successful deployment path.

During the initial onboarding attempt, the Microsoft Defender for Endpoint **SENSE service was unavailable**, preventing the endpoint from completing the expected onboarding workflow.

Troubleshooting included checking the service:

```powershell
sc query sense
```

Windows edition validation:

```powershell
DISM /Online /Get-CurrentEdition
```

Microsoft Defender for Endpoint capability inspection:

```powershell
DISM.EXE /Online /Get-CapabilityInfo /CapabilityName:Microsoft.Windows.Sense.Client~~~~
```

The troubleshooting process required validating several layers of the endpoint configuration rather than assuming the onboarding package itself was the source of the problem.

The investigation reinforced an important security-engineering principle:

> Security platforms depend on underlying services, operating-system capabilities, licensing, connectivity, permissions, and telemetry pipelines. Effective troubleshooting requires validating those dependencies rather than repeatedly rerunning a failed deployment command.

After resolving the endpoint requirements, Defender services were successfully validated and the endpoint was able to report security telemetry.

---

# Telemetry Validation

Successful endpoint onboarding alone was not treated as sufficient evidence that the security environment was functioning correctly.

The next requirement was validating the telemetry pipeline.

```text
Windows Endpoint
       │
       ▼
Defender Sensor
       │
       ▼
Microsoft Cloud
       │
       ▼
Defender for Endpoint
       │
       ▼
Microsoft Defender XDR
       │
       ▼
Analyst Visibility
```

The endpoint becoming visible within Defender Device Inventory confirmed that the device had successfully established communication with the Microsoft security platform.

This was important because security tooling provides little operational value if the underlying telemetry is missing, delayed, or incorrectly configured.

---

# Controlled Detection Test

After the endpoint was successfully onboarded, controlled Microsoft Defender security test activity was generated to validate the detection pipeline.

The purpose was not to simulate an uncontrolled compromise.

The objective was to confirm that security-relevant endpoint activity could successfully travel through the complete detection pipeline:

```text
Controlled Endpoint Activity
           │
           ▼
Defender for Endpoint Sensor
           │
           ▼
Endpoint Telemetry
           │
           ▼
Defender Cloud Analytics
           │
           ▼
EDR Detection
           │
           ▼
Microsoft Defender XDR Alert
           │
           ▼
Analyst Investigation
```

The resulting investigation included a **Suspicious PowerShell command line** alert associated with the monitored endpoint.

The detection demonstrated that:

- Endpoint telemetry was being collected
- Defender for Endpoint was communicating with Microsoft cloud services
- Detection logic was evaluating the telemetry
- Relevant activity could generate a security alert
- The alert could be investigated through Microsoft Defender XDR

---

# Alert Investigation

The resulting alert was investigated through Microsoft Defender XDR.

The investigation examined:

- Alert title
- Alert severity
- Detection source
- Affected endpoint
- Associated user
- Detection timestamp
- Process execution
- Command-line activity
- Supporting endpoint evidence
- Related incident context

Rather than treating the alert itself as proof of compromise, the investigation pivoted into the underlying endpoint telemetry.

This distinction is critical.

```text
Alert
  │
  ▼
Investigation
  │
  ▼
Evidence
  │
  ▼
Context
  │
  ▼
Analyst Verdict
```

An alert represents a reason to investigate.

It does not automatically represent the final conclusion.

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

Analyzing the process tree provided context that could not be obtained from the alert title alone.

The process lineage helped establish:

- Which process initiated the activity
- The user execution context
- How PowerShell was launched
- What occurred before PowerShell execution
- Whether the execution chain matched the controlled test
- Whether unexpected parent processes were present
- Whether unexpected child processes were present

This demonstrated why process names should not be interpreted without surrounding execution context.

For example:

```text
powershell.exe
```

by itself does not establish malicious activity.

PowerShell is a legitimate administrative tool.

The analyst must determine:

```text
Who launched it?
        │
        ▼
What launched it?
        │
        ▼
What command executed?
        │
        ▼
What happened afterward?
```

---

# Device Timeline Investigation

The Microsoft Defender Device Timeline was used to reconstruct activity around the detection.

Timeline analysis focused on:

- Process execution
- PowerShell activity
- File activity
- Detection events
- Alert associations
- User context
- Event timestamps

The Device Timeline allowed the investigation to move from:

```text
"What alert fired?"
```

to:

```text
"What actually happened on the endpoint?"
```

The timeline can help answer questions such as:

```text
What happened before the suspicious process executed?

What happened immediately afterward?

Which user was active?

Were additional processes created?

Were files created or modified?

Did network activity occur?

Were additional detections generated?
```

That distinction between reviewing an alert and reconstructing endpoint behavior is fundamental to EDR investigation.

---

# Advanced Hunting with KQL

Microsoft Defender Advanced Hunting was used to independently search the endpoint telemetry supporting the detection.

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
- Restricts results to the relevant investigation time window
- Filters activity to the affected endpoint
- Searches for processes associated with the controlled detection test
- Returns process command-line information
- Returns initiating-process information
- Provides account context
- Orders events chronologically for investigation

The hunt returned relevant endpoint events associated with the controlled test activity.

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
       Analyst Verdict
```

This workflow reinforces an important SOC principle:

> **An alert is the beginning of an investigation—not the conclusion.**

---

# Lab 01 Investigation Findings

The investigation established that:

- The Windows 11 Enterprise endpoint successfully reported security telemetry.
- Microsoft Defender for Endpoint successfully monitored the endpoint.
- Microsoft Defender for Endpoint detected the controlled activity.
- Microsoft Defender XDR generated an EDR alert.
- The alert was associated with the expected endpoint.
- The alert contained relevant user context.
- Process telemetry preserved the underlying activity.
- The process tree showed the execution chain leading to PowerShell.
- Device Timeline data provided chronological context around the activity.
- Microsoft Defender Advanced Hunting returned relevant process events.
- KQL provided independent validation of the endpoint evidence.
- The observed behavior was consistent with the authorized Microsoft Defender security test.
- No evidence identified an unauthorized compromise within the scope of the exercise.

---

# Lab 01 Incident Classification

Because the activity was intentionally generated as part of an authorized security-validation exercise, the investigation was documented accordingly.

```text
Detection Result: Valid Detection
Activity Type: Authorized Security Testing
Compromise: No
Analyst Disposition: Expected / Security Testing
```

This distinction is important.

A security detection can be technically valid even when the underlying behavior was intentionally generated.

A detection should therefore not automatically be classified as a false positive simply because the activity was authorized.

The detection correctly identified the behavior.

The analyst's responsibility was to determine the context and intent behind that behavior.

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

Cleanup is part of the investigation and testing lifecycle because temporary infrastructure should not remain unnecessarily exposed after an exercise has been completed.

---

# Lab 01 Investigation Methodology

The methodology practiced throughout Lab 01 can be summarized as:

```text
1. Build
      ↓
2. Configure
      ↓
3. Onboard
      ↓
4. Validate
      ↓
5. Generate Controlled Activity
      ↓
6. Detect
      ↓
7. Investigate
      ↓
8. Analyze Process Lineage
      ↓
9. Review Timeline
      ↓
10. Hunt with KQL
      ↓
11. Correlate Evidence
      ↓
12. Determine Scope
      ↓
13. Classify Activity
      ↓
14. Clean Up
      ↓
15. Document
```

---

# Lab 02 — Microsoft Defender XDR Fundamentals

## Objective

Develop practical familiarity with the **Microsoft Defender XDR investigation environment** and understand how a SOC analyst moves from high-level security information into the incidents, alerts, evidence, devices, users, timelines, processes, and response capabilities associated with a security case.

Where Lab 01 focused on:

> **Can I build and validate the Microsoft security environment?**

Lab 02 focused on:

> **Can I navigate and investigate within that environment like a security analyst?**

The lab followed a structured investigation path:

```text
Microsoft Defender XDR Dashboard
              │
              ▼
        Device Inventory
              │
              ▼
         Incident Queue
              │
              ▼
           Incident
              │
              ▼
        Attack Story
              │
              ▼
            Alert
              │
              ▼
      Evidence / Entities
          ↙         ↘
       Device       User
          │           │
          ▼           ▼
 Device Timeline  User Timeline
          │
          ▼
   Process Lineage
          │
          ▼
    Analyst Verdict
          │
          ▼
   Response Options
```

---

# Understanding Microsoft Defender XDR

Microsoft Defender XDR is an Extended Detection and Response platform designed to provide unified investigation across supported Microsoft security workloads and security-relevant entities.

The central concept is not simply antivirus protection.

The important capability is **cross-domain correlation**.

Microsoft Defender XDR organizes investigation information into related security objects:

```text
Microsoft Defender XDR
        │
        ├── Incidents
        │      │
        │      ├── Alerts
        │      ├── Evidence
        │      ├── Devices
        │      ├── Users
        │      └── Investigation Context
        │
        ├── Device Inventory
        │      │
        │      └── Device Timeline
        │
        ├── User Entities
        │      │
        │      └── User Timeline
        │
        ├── Advanced Hunting
        │
        └── Response Actions
```

This allows an analyst to pivot between different parts of an investigation without treating each security signal as an isolated event.

---

# Alert vs. Incident

One of the most important concepts reinforced during Lab 02 was the distinction between an **alert** and an **incident**.

## Alert

An alert represents a specific security detection.

Conceptually:

```text
Observed Telemetry
       │
       ▼
Detection Logic
       │
       ▼
Security Alert
```

An alert indicates that observed behavior matched security detection logic strongly enough to require analyst attention.

An alert can contain information such as:

- Detection title
- Severity
- Detection source
- Affected device
- Affected user
- Process
- File
- IP address
- Timestamp
- Supporting evidence
- MITRE ATT&CK context

However:

> **An alert does not automatically establish malicious intent or compromise.**

The alert tells the analyst what was detected.

The analyst must determine what the activity means.

---

## Incident

An incident represents the broader correlated security case.

An incident can contain multiple related alerts and associated security entities.

```text
Alert A ─────┐
             │
Alert B ─────┼────► Incident
             │
Alert C ─────┘
```

The incident may connect:

- Alerts
- Devices
- Users
- Processes
- Files
- IP addresses
- Evidence
- Investigation information
- Response activity

The incident therefore provides the larger attack story surrounding one or more detections.

This changes the analyst's perspective from:

```text
"What does this individual alert say?"
```

to:

```text
"What security activity is occurring across the environment,
and how are these signals related?"
```

---

# Microsoft Defender XDR Dashboard

The Microsoft Defender XDR dashboard was reviewed as the high-level operational entry point into the security environment.

The dashboard provides visibility into areas such as:

- Security incidents
- Alerts
- Protected assets
- Device inventory
- Hunting capabilities
- Security posture
- Investigation functions
- Response functions

The dashboard serves as an operational overview.

It is not where the complete investigation occurs.

The analyst must pivot from high-level security information into the underlying incidents, alerts, entities, and telemetry.

```text
Dashboard
    │
    ▼
Security Signal
    │
    ▼
Investigation
    │
    ▼
Underlying Evidence
```

---

# Device Inventory

Microsoft Defender Device Inventory was reviewed to understand how onboarded endpoints appear within the Defender environment.

Device Inventory provides a centralized view of monitored endpoint assets.

An analyst can use this area to identify and pivot into devices requiring investigation.

A device entity can provide information such as:

- Device name
- Operating system
- Risk level
- Exposure level
- Onboarding state
- Sensor health
- Logged-on users
- Associated alerts
- Associated incidents
- Software
- Vulnerabilities
- Security recommendations
- Device Timeline
- Response actions

Device Inventory therefore connects asset visibility with investigation.

```text
Device Inventory
       │
       ▼
Select Endpoint
       │
       ▼
Device Entity Page
       │
       ├── Overview
       ├── Timeline
       ├── Alerts
       ├── Incidents
       ├── Software
       └── Response Actions
```

---

# Incident Queue Analysis

The centralized **Incident Queue** was used to examine security cases requiring analyst attention.

Key incident attributes reviewed included:

- Incident name
- Severity
- Status
- Classification
- Assigned analyst
- Alert count
- Affected assets
- Service sources
- Creation time
- Last activity

The incident queue represents one of the primary locations where SOC analysts begin prioritizing security investigations.

However, severity alone should not determine investigative priority.

An analyst may also consider:

```text
Severity
   +
Asset Criticality
   +
Number of Alerts
   +
Affected Users
   +
Attack Stage
   +
Recency
   +
Ongoing Activity
   +
Detection Confidence
   +
Existing Investigation State
   =
Investigation Priority
```

This reinforced the difference between simply sorting alerts and performing actual SOC triage.

---

# Incident Investigation

An incident was opened to examine the broader correlated security story.

Investigation areas included:

- Incident summary
- Attack story
- Alerts
- Assets
- Evidence and response
- Investigation information
- Timeline
- Classification
- Status

The key concept was **correlation**.

Instead of treating security detections as independent:

```text
Alert 1

Alert 2

Alert 3
```

Defender XDR can organize related security information into:

```text
          Incident
             │
      ┌──────┼──────┐
      ▼      ▼      ▼
   Alert 1 Alert 2 Alert 3
      │      │      │
      └──────┼──────┘
             ▼
      Related Evidence
             │
      ┌──────┴──────┐
      ▼             ▼
    Device          User
```

This provides substantially more investigative context than examining each detection in isolation.

---

# Attack Story Analysis

The incident Attack Story was reviewed to understand how Microsoft Defender XDR visually connects security activity and affected entities.

The attack story helps an analyst understand relationships between:

- Alerts
- Devices
- Users
- Processes
- Files
- Other security entities

The objective was not simply to view a diagram.

The objective was to understand the relationships represented by that diagram.

```text
Security Activity
       │
       ▼
Alert
       │
       ▼
Affected Entity
       │
       ▼
Related Evidence
       │
       ▼
Additional Activity
```

Attack-story analysis supports the broader XDR goal of connecting security signals into an understandable investigation narrative.

---

# Alert Analysis

The investigation then moved from the incident level into an individual alert.

Alert review focused on:

- Detection title
- Severity
- Status
- Detection source
- Category
- Detection timestamp
- Affected endpoint
- Affected user
- Processes
- Files
- IP addresses
- Supporting evidence
- Related entities
- MITRE ATT&CK context when applicable

A repeatable analyst questioning model was used:

```text
1. What was detected?

2. When did it happen?

3. Which device was involved?

4. Which user was involved?

5. What process or file triggered the detection?

6. What launched the process?

7. What command line executed?

8. Did related network activity occur?

9. What other entities are connected?

10. Was the behavior prevented or only detected?

11. What evidence supports the alert?

12. What happened immediately before the alert?

13. What happened immediately afterward?

14. Is the behavior isolated?

15. What should I investigate next?
```

This shifts analysis away from simply trusting an alert title and toward evidence-driven investigation.

---

# Detection Does Not Equal Compromise

A major concept reinforced throughout Week 17 was:

> **A detection is not automatically a confirmed compromise.**

The detection establishes that observed behavior matched security logic.

The analyst must determine the meaning of that behavior.

Possible investigative outcomes may include:

```text
True Positive — Malicious

True Positive — Authorized / Expected Activity

False Positive

Benign / Informational

Insufficient Evidence
```

The exact terminology may vary between platforms and organizations.

The underlying principle remains the same:

```text
Detection
    │
    ▼
Investigation
    │
    ▼
Evidence
    │
    ▼
Context
    │
    ▼
Verdict
```

The verdict should be based on the evidence rather than the alert title alone.

---

# Evidence & Entity Investigation

The investigation expanded from the alert into associated evidence and security-relevant entities.

Entities may include:

- Devices
- Users
- Processes
- Files
- IP addresses
- URLs
- Domains
- Applications
- Mailboxes
- Cloud resources

The investigation process can therefore move through multiple related objects:

```text
Alert
  │
  ▼
Device
  │
  ▼
Process
  │
  ▼
File
  │
  ▼
User
  │
  ▼
IP / Network Context
```

This process of **pivoting between related entities** is one of the core investigative capabilities of XDR.

The analyst is not limited to the object that originally generated the alert.

Each entity can provide another investigative direction.

---

# Device Investigation

The affected endpoint was investigated through its Microsoft Defender device entity page.

The device view provides endpoint context that can include:

- Device hostname
- Operating system
- Risk level
- Exposure information
- Sensor state
- Onboarding state
- Logged-on users
- Associated incidents
- Associated alerts
- Security recommendations
- Software inventory
- Vulnerability information
- Device Timeline
- Available response actions

This changes the investigative question from:

```text
"What happened in this alert?"
```

to:

```text
"What else happened on this endpoint?"
```

That broader question is essential for determining scope.

For example, an individual PowerShell detection may represent one process execution.

The device investigation may reveal whether that same endpoint also experienced:

- Additional suspicious processes
- File creation
- Network communication
- Registry changes
- Additional alerts
- Other user activity
- Related security events

---

# Device Timeline Analysis

The **Device Timeline** was used to examine endpoint activity chronologically.

The Device Timeline represents a security-focused history of activity observed on the endpoint.

Potential endpoint telemetry can include:

- Process creation
- Process termination
- File creation
- File modification
- Network connections
- Registry changes
- User logons
- Service activity
- PowerShell execution
- Defender detections
- Scheduled tasks
- Remote activity
- Other endpoint events

Timeline analysis helps answer:

```text
What happened before the detection?

What happened during the detection?

What happened immediately afterward?

Which user was active?

Which process executed?

What launched that process?

What command line was used?

Were files created?

Were files modified?

Did network communication occur?

Did additional processes execute?

Were additional alerts generated?
```

The Device Timeline therefore helps reconstruct endpoint behavior instead of relying exclusively on summarized alert information.

---

# Hypothesis-Driven Timeline Filtering

Rather than manually reviewing every available endpoint event, timeline filtering was approached according to an investigation hypothesis.

Example:

```text
Hypothesis:
A suspicious process executed.

Best Starting Evidence:
Process events.
```

Another example:

```text
Hypothesis:
A compromised process communicated externally.

Best Starting Evidence:
Process events + network activity.
```

Another example:

```text
Hypothesis:
Suspicious activity modified persistence-related configuration.

Best Starting Evidence:
Process events + registry activity.
```

This reinforces an important investigative principle:

> **Filter telemetry according to the question you are trying to answer.**

Security investigation should be driven by hypotheses and evidence rather than random navigation through available data.

---

# Process Event Investigation

Individual process-related timeline events were reviewed for fields such as:

- Process name
- Process command line
- Executable path
- Parent process
- User
- Process ID
- File hash
- Timestamp

Three pieces of information are especially valuable during initial process analysis.

## Process

```text
What executed?
```

Example:

```text
powershell.exe
```

## Command Line

```text
How exactly was the process executed?
```

The command line can reveal:

- Parameters
- Scripts
- URLs
- File paths
- Encoded content
- Execution-policy changes
- Other behavioral context

## Parent Process

```text
What launched the process?
```

This introduces the concept of **process lineage**.

---

# Process Lineage Analysis

A process such as:

```text
powershell.exe
```

cannot be accurately interpreted from the executable name alone.

PowerShell is a legitimate Windows administration tool.

The surrounding process relationships provide context.

For example:

```text
explorer.exe
    │
    ▼
cmd.exe
    │
    ▼
powershell.exe
```

may have very different implications from:

```text
winword.exe
    │
    ▼
powershell.exe
```

The second execution chain may deserve additional investigation because an Office application unexpectedly launching PowerShell can represent suspicious behavior depending on the surrounding context.

Process lineage helps answer:

```text
What launched this process?

What launched the parent?

Which user context was involved?

What command executed?

What did the process launch afterward?
```

---

# Process Tree Analysis

Where process lineage information was available, the investigation examined parent-child relationships conceptually represented as:

```text
Parent Process
      │
      ▼
Current Process
      │
      ▼
Child Process
```

Process trees can help determine:

- How execution began
- Which process initiated suspicious activity
- Whether a user-launched application was involved
- Whether scripting interpreters were chained together
- Whether unexpected child processes appeared
- Whether execution matched normal user behavior
- Whether execution matched the controlled test
- Whether the process chain supports or contradicts the alert

Process trees are especially valuable when investigating tools such as:

- `powershell.exe`
- `cmd.exe`
- `wscript.exe`
- `cscript.exe`
- `mshta.exe`
- Browsers
- Office applications
- Windows system utilities

Many legitimate Windows binaries can also be abused.

The surrounding process relationships help determine the significance of their execution.

---

# Device Alert History

Associated incidents and alerts for the endpoint were reviewed to provide historical context.

This helps answer:

```text
Is this an isolated event?
```

or:

```text
Has this device generated other related security activity?
```

Historical device context can substantially change the interpretation of an individual alert.

For example:

```text
Single Alert
     │
     ▼
Possibly Isolated Activity
```

may be interpreted differently from:

```text
Alert
  +
Related Alert
  +
Additional Suspicious Process
  +
Related Network Activity
     │
     ▼
Potentially Broader Incident
```

---

# Identity Investigation

The investigation also expanded from endpoint evidence into **user context**.

A user represents another security-relevant entity that can be investigated.

```text
Incident
    │
    ▼
Alert
    │
    ├────────────► Device
    │
    └────────────► User
```

Where device investigation asks:

```text
"What happened on this endpoint?"
```

identity investigation asks:

```text
"What happened with this account?"
```

This distinction becomes increasingly important when investigating:

- Compromised credentials
- Suspicious authentication
- Account takeover
- Lateral movement
- Privilege misuse
- Cloud activity
- Unusual logons
- Identity-based attacks

---

# User Entity Overview

The user entity view was reviewed to understand how identity context is incorporated into a Microsoft Defender XDR investigation.

Depending on the available security data and integrations, a user entity can provide context related to:

- User identity
- Associated devices
- Security alerts
- Authentication activity
- Related incidents
- Identity risk
- Cloud activity
- Other security events

This allows the analyst to pivot from endpoint activity into the identity associated with that activity.

```text
Suspicious Endpoint Activity
            │
            ▼
        User Account
            │
            ▼
   Identity Investigation
            │
            ▼
Additional User Activity
```

---

# User Timeline

The User Timeline provides chronological identity-related context when relevant telemetry is available.

Identity investigation may involve reviewing:

- Security alerts affecting the user
- Authentication activity
- Device logons
- Identity events
- Directory activity
- Cloud application activity
- Other security events associated with the account

The analytical questions become:

```text
Where was this account used?

When was it used?

Which devices were involved?

What security events occurred around the same time?

Were there unusual authentication patterns?

Does the identity activity support the endpoint evidence?

Does the identity activity contradict the endpoint evidence?

Is the account involved in activity on additional devices?
```

This demonstrates why XDR investigations benefit from correlation across multiple security domains rather than endpoint evidence alone.

---

# Device and User Correlation

One of the strongest concepts introduced through Lab 02 was the ability to investigate the same security event from multiple entity perspectives.

```text
              Incident
                 │
                 ▼
               Alert
                 │
         ┌───────┴───────┐
         ▼               ▼
       Device           User
         │               │
         ▼               ▼
Device Timeline      User Timeline
         │               │
         └───────┬───────┘
                 ▼
         Correlated Context
                 │
                 ▼
          Analyst Verdict
```

The device perspective answers:

> **What happened on the endpoint?**

The identity perspective answers:

> **What happened with the account?**

Combining both perspectives provides stronger investigative context.

---

# Incident Lifecycle Management

Lab 02 also examined the operational lifecycle of a Microsoft Defender incident.

```text
Telemetry
    │
    ▼
Detection
    │
    ▼
Alert
    │
    ▼
Incident Correlation
    │
    ▼
Triage
    │
    ▼
Investigation
    │
    ▼
Classification
    │
    ▼
Response
    │
    ▼
Resolution
```

Incident status is therefore not simply a technical field.

It represents **case management**.

## New

The incident has entered the queue and requires review.

## In Progress

The incident is actively being investigated or handled.

## Resolved

The investigation and required response work have been completed.

This introduces an important SOC concept:

> **Security Operations requires managing investigations as cases—not simply acknowledging alerts.**

---

# Analyst Classification

After evidence has been reviewed, the analyst must determine what conclusion is supported by the investigation.

A conceptual decision model is:

```text
Detection
    │
    ▼
Evidence
    │
    ▼
Context
    │
    ▼
Scope
    │
    ▼
Analyst Assessment
    │
    ├── Malicious
    ├── Authorized / Expected
    ├── False Positive
    ├── Benign
    └── Insufficient Evidence
```

The important principle is that the classification should reflect the evidence.

The alert title should not determine the verdict before the investigation has been completed.

---

# Response Actions

The Defender device investigation workflow was also used to review endpoint response capabilities.

Depending on licensing, permissions, configuration, and incident requirements, endpoint response capabilities can include actions such as:

- Device isolation
- Antivirus scanning
- Investigation-package collection
- Automated investigation
- Application restriction
- Live Response

These capabilities were evaluated as **response options**.

They should not automatically be executed simply because an alert exists.

The analyst should first determine:

```text
What happened?
      │
      ▼
What is affected?
      │
      ▼
How confident is the evidence?
      │
      ▼
Is the activity malicious?
      │
      ▼
Is containment necessary?
      │
      ▼
What is the safest appropriate response?
```

Containment without sufficient context can disrupt legitimate business operations.

Delayed containment during a genuine compromise can allow additional attacker activity.

Effective incident response therefore requires both technical capability and analyst judgment.

---

# Microsoft Defender XDR Analyst Workflow

The complete Lab 02 investigation methodology can be summarized as:

```text
1. Review Defender XDR Dashboard
            │
            ▼
2. Review Device Inventory
            │
            ▼
3. Review Incident Queue
            │
            ▼
4. Prioritize Incident
            │
            ▼
5. Open Incident
            │
            ▼
6. Review Incident Summary
            │
            ▼
7. Review Attack Story
            │
            ▼
8. Examine Correlated Alerts
            │
            ▼
9. Investigate Individual Alert
            │
            ▼
10. Review Evidence & Entities
            │
            ▼
11. Pivot to Device
            │
            ▼
12. Review Device Overview
            │
            ▼
13. Review Device Timeline
            │
            ▼
14. Analyze Process Events
            │
            ▼
15. Analyze Process Lineage
            │
            ▼
16. Review Device Alert History
            │
            ▼
17. Pivot to User Context
            │
            ▼
18. Review User Entity
            │
            ▼
19. Review User Timeline
            │
            ▼
20. Correlate Device & Identity Evidence
            │
            ▼
21. Determine Scope
            │
            ▼
22. Develop Analyst Verdict
            │
            ▼
23. Evaluate Response Options
            │
            ▼
24. Classify / Resolve
            │
            ▼
25. Document Findings
```

---

# Analyst Investigation Framework

Across both Week 17 labs, the investigation process can be summarized through six analytical questions.

## 1. Detection — What happened?

Identify the security behavior that generated analyst attention.

Questions include:

- What detection fired?
- What behavior triggered it?
- When did it occur?
- What security product generated the detection?

---

## 2. Entity — Who or what was involved?

Identify affected security entities.

These may include:

- Devices
- Users
- Processes
- Files
- IP addresses
- URLs
- Domains
- Applications
- Cloud resources

---

## 3. Context — What happened around it?

Review the surrounding evidence.

This can include:

- Process lineage
- Command lines
- Device Timeline
- User Timeline
- Authentication activity
- File activity
- Network activity
- Registry activity
- Related alerts

---

## 4. Scope — What else was affected?

Determine whether the activity is isolated or part of broader behavior.

Questions include:

- Are additional devices affected?
- Are additional users involved?
- Are there related alerts?
- Did the process create additional files?
- Did network communication occur?
- Did similar activity occur elsewhere?

---

## 5. Verdict — What does the evidence support?

Determine the most defensible classification.

Possible outcomes include:

- Malicious
- Authorized / expected
- False positive
- Benign
- Insufficient evidence

---

## 6. Response — What should happen next?

Determine the appropriate action based on the evidence.

Possible actions include:

- Continue investigating
- Collect additional evidence
- Escalate
- Monitor
- Isolate the endpoint
- Run an antivirus scan
- Collect an investigation package
- Use Live Response
- Remediate
- Close the incident

The complete model is:

```text
DETECTION
    │
    ▼
ENTITY
    │
    ▼
CONTEXT
    │
    ▼
SCOPE
    │
    ▼
VERDICT
    │
    ▼
RESPONSE
```

---

# Lab 01 vs. Lab 02

| Lab 01 — Environment & EDR Investigation | Lab 02 — Defender XDR Fundamentals |
|---|---|
| Built the Microsoft security environment | Operated the Defender XDR investigation environment |
| Configured Microsoft Azure resources | Navigated the Defender XDR dashboard |
| Created a dedicated resource group | Reviewed Device Inventory |
| Created a Log Analytics workspace | Reviewed the centralized Incident Queue |
| Enabled Microsoft Sentinel | Studied incident prioritization |
| Configured Defender for Endpoint | Investigated incident correlation |
| Onboarded a Windows endpoint | Distinguished incidents from alerts |
| Troubleshot Defender services | Reviewed the incident Attack Story |
| Validated endpoint telemetry | Investigated individual alerts |
| Generated controlled security activity | Investigated evidence and entities |
| Investigated an EDR detection | Pivoted into affected devices |
| Analyzed process lineage | Reconstructed activity with Device Timeline |
| Used Device Timeline | Expanded process-lineage analysis |
| Hunted telemetry with KQL | Reviewed device alert history |
| Used Advanced Hunting | Investigated user and identity context |
| Correlated endpoint evidence | Reviewed User Timeline |
| Classified authorized testing | Studied incident lifecycle management |
| Performed secure cleanup | Evaluated endpoint response capabilities |

Together:

```text
Lab 01
"Can I build and validate the security environment?"
                    │
                    ▼
Lab 02
"Can I investigate and operate within that environment?"
```

---

# SIEM vs. EDR vs. XDR

Week 17 reinforced the relationship between three major Security Operations technologies.

---

## EDR — Microsoft Defender for Endpoint

Endpoint Detection and Response provides deep endpoint visibility and response capabilities.

```text
Endpoint
   │
   ├── Processes
   ├── Files
   ├── Registry
   ├── Network Activity
   ├── Logons
   ├── Command Lines
   └── Security Detections
```

EDR helps answer:

> **What happened on this endpoint?**

Microsoft Defender for Endpoint provides the endpoint-level telemetry required to reconstruct suspicious host behavior.

---

## XDR — Microsoft Defender XDR

Extended Detection and Response correlates security information across supported security domains and entities.

```text
Alerts
   +
Devices
   +
Users
   +
Evidence
   +
Security Workloads
        │
        ▼
Correlated Incident
```

XDR helps answer:

> **How are these security signals related?**

Microsoft Defender XDR allows an analyst to investigate the relationships between security signals rather than analyzing each detection independently.

---

## SIEM — Microsoft Sentinel

Security Information and Event Management provides centralized security monitoring, analytics, correlation, hunting, and Security Operations capabilities across broader data sources.

```text
Endpoints
    +
Identities
    +
Cloud
    +
Applications
    +
Network
    +
Security Products
        │
        ▼
Centralized Security Data
        │
        ▼
Microsoft Sentinel
        │
        ├── Analytics
        ├── Incidents
        ├── Hunting
        ├── KQL
        └── SOC Workflows
```

SIEM helps answer:

> **What is happening across the broader environment?**

---

# Combined Microsoft Security Operations Model

```text
                    MICROSOFT SECURITY OPERATIONS

Microsoft Entra ID
       │
       ▼
Identity & Administrative Context
       │
       ▼
Microsoft Azure
       │
       ▼
Azure Log Analytics
       │
       ▼
Microsoft Sentinel
       │
       │ SIEM Visibility & Analytics
       │
       ├─────────────────────────────────┐
       │                                 │
       ▼                                 ▼
Cross-Source Analytics            Microsoft Defender XDR
                                         │
                                         │ XDR Correlation
                                         ▼
                                      Incident
                                         │
                         ┌───────────────┼───────────────┐
                         ▼               ▼               ▼
                       Alerts          Devices          Users
                                         │               │
                                         ▼               ▼
                                  Device Timeline   User Timeline
                                         │
                                         ▼
                                  Process Lineage
                                         │
                                         ▼
                                 Defender for Endpoint
                                         │
                                         ▼
                                  Windows Endpoint
```

This architecture demonstrates how **identity, SIEM, XDR, and EDR capabilities complement one another during Security Operations investigations**.

---

# Evidence Collected

Portfolio evidence across Week 17 documents the progression from environment deployment into endpoint detection and XDR investigation.

## Lab 01 Evidence

Portfolio evidence for Lab 01 includes screenshots documenting:

- Microsoft Entra ID environment configuration
- Microsoft Azure security environment
- Azure resource group
- Azure Log Analytics workspace deployment
- Microsoft Sentinel enablement
- Microsoft Defender XDR configuration
- Microsoft Defender for Endpoint onboarding
- Windows 11 Enterprise endpoint configuration
- Defender service validation
- Device Inventory
- Controlled detection activity
- EDR alert investigation
- Process-tree evidence
- Device Timeline telemetry
- Microsoft Defender Advanced Hunting
- KQL query and results
- Incident classification
- Post-investigation cleanup

---

## Lab 02 Evidence

Portfolio evidence for Lab 02 includes:

| Evidence | Screenshot Filename |
|---|---|
| Microsoft Defender XDR dashboard | `week17_Lab2_Defender_XDR_Dashboard.png` |
| Endpoint Device Inventory | `week17_Lab2_Device_Inventory.png` |
| Microsoft Defender for Endpoint device overview | `week17_Lab2_MDE_Device_Overview.png` |
| Defender XDR Incident Queue | `week17_Lab2_Incident_Queue.png` |
| Incident Attack Story | `week17_Lab2_Incident_Attack_Story.png` |
| Alert details | `week17_Lab2_Alert_Details.png` |
| Incident assets | `week17_Lab2_Incident_Assets.png` |
| Evidence and response | `week17_Lab2_Evidence_and_Response.png` |
| Device Timeline | `week17_Lab2_Device_Timeline.png` |
| Device process tree | `week17_Lab2_Device_Process_Tree.png` |
| Incident lifecycle management | `week17_Lab2_Incident_Lifecycle_Management.png` |
| Device response actions | `week17_Lab2_Device_Response_Actions.png` |
| User entity overview | `week17_Lab2_User_Entity_Overview.png` |
| User Timeline | `week17_Lab2_User_Timeline.png` |

The Lab 02 evidence was selected to tell an investigation story rather than simply document every page visited.

```text
Microsoft Defender XDR Dashboard
              │
              ▼
        Device Inventory
              │
              ▼
         Incident Queue
              │
              ▼
           Incident
              │
              ▼
        Attack Story
              │
              ▼
            Alert
              │
              ▼
      Evidence / Entities
          ↙         ↘
       Device       User
          │           │
          ▼           ▼
 Device Timeline  User Timeline
          │
          ▼
     Process Tree
          │
          ▼
   Analyst Assessment
          │
          ▼
    Response Options
```

Sensitive identifiers, credentials, subscription information, tenant information, authentication information, personal account information, and other unnecessary private data are excluded or redacted from public portfolio evidence.

---

# MITRE ATT&CK-Relevant Investigation Concepts

Although the activity investigated during Week 17 involved authorized security testing rather than an uncontrolled real-world intrusion, the labs exercised analysis techniques relevant to behaviors encountered during genuine incidents.

Relevant concepts included:

- **Command and Scripting Interpreter**
- **PowerShell**
- **Process Execution**
- **User Execution**
- **Process Lineage**
- **Parent-Child Process Relationships**
- **Endpoint Telemetry Analysis**
- **Identity Investigation**
- **Security Event Correlation**
- **Incident Scoping**

MITRE ATT&CK mappings should always be based on the behavior actually observed in the evidence rather than assigned solely because an alert fired.

The correct analytical sequence is:

```text
Observed Behavior
       │
       ▼
Evidence Validation
       │
       ▼
Behavior Interpretation
       │
       ▼
Relevant ATT&CK Mapping
```

not:

```text
Alert Fired
    │
    ▼
Automatically Assign Technique
```

---

# Key Takeaways

### 1. Building the environment is part of security engineering

A functioning EDR, XDR, and SIEM environment depends on identity, licensing, endpoints, cloud resources, sensors, connectors, permissions, networking, and telemetry pipelines working together.

Understanding how these components connect is part of operating the security platform effectively.

### 2. Successful deployment does not automatically mean successful telemetry collection

An endpoint can appear configured while required services or telemetry pipelines are not functioning correctly.

Security tooling must be validated through observable telemetry and controlled testing.

### 3. An alert and an incident are not the same thing

An alert represents a specific detection.

An incident represents the broader security case that can correlate related alerts, evidence, devices, users, processes, and other investigation context.

### 4. Detection does not automatically mean compromise

An alert indicates that behavior matched security detection logic.

The analyst must determine what the activity actually represents.

### 5. Alerts require validation

An alert should be treated as the beginning of an investigation.

The underlying endpoint, identity, process, file, network, and timeline evidence should determine the conclusion.

### 6. Correlation creates context

A single process, authentication event, file, or network connection may be difficult to interpret in isolation.

Relationships between those events can reveal the broader security story.

### 7. Process lineage provides critical context

Understanding what launched a process and what that process launched afterward can substantially change the interpretation of endpoint activity.

### 8. A process name alone is rarely enough

Legitimate tools such as PowerShell can be used for normal administration or malicious activity.

Command-line arguments, parent processes, users, timestamps, child processes, files, and network behavior provide the context required for analysis.

### 9. Device Timeline reconstructs endpoint behavior

Device Timeline helps analysts determine what happened before, during, and after suspicious endpoint activity.

This provides significantly more investigative context than an alert title alone.

### 10. User Timeline adds identity context

Endpoint activity and identity activity should not always be investigated separately.

User Timeline provides another perspective for determining whether an account was involved in additional or suspicious activity.

### 11. XDR investigation is entity-driven

Analysts can pivot between alerts, devices, users, processes, files, IP addresses, and other evidence to understand relationships and determine scope.

### 12. KQL enables independent validation

Microsoft Defender Advanced Hunting allows analysts to search the underlying telemetry rather than relying exclusively on summarized detection information.

### 13. Threat hunting should be hypothesis-driven

Analysts should search and filter telemetry according to a specific investigative question.

The goal is not to review every available event.

The goal is to identify the evidence required to test an investigative hypothesis.

### 14. SIEM, XDR, and EDR are complementary

Microsoft Defender for Endpoint provides deep endpoint visibility.

Microsoft Defender XDR correlates security information into unified investigations.

Microsoft Sentinel provides broader SIEM visibility, analytics, hunting, and cross-source security monitoring.

### 15. Incident management is operational work

Security Operations involves prioritizing, assigning, investigating, classifying, responding to, documenting, and resolving security cases.

The work does not end when an alert is acknowledged.

### 16. Response actions require analyst judgment

The availability of containment capabilities does not mean they should automatically be used.

Response should be proportional to the available evidence, confidence, business context, and potential impact.

### 17. Troubleshooting is a security skill

Diagnosing endpoint onboarding, licensing, services, operating-system compatibility, permissions, connectivity, and telemetry issues is directly relevant to security engineering and Security Operations work.

### 18. Authorized testing must be classified correctly

A detection can be technically valid while the underlying behavior is expected.

Authorized security testing should not automatically be labeled a false positive or a genuine compromise.

### 19. Cleanup is part of the investigation lifecycle

Temporary infrastructure, services, files, and testing artifacts should be removed when they are no longer required.

### 20. Strong investigations are evidence-driven

The objective is not to find evidence that supports the alert title.

The objective is to determine which conclusion is best supported by the available evidence.

---

# Professional Skills Demonstrated

## Security Operations

- SOC alert triage
- Incident queue analysis
- Incident prioritization
- Incident investigation
- Alert investigation
- Attack-story analysis
- Evidence correlation
- Entity investigation
- Incident classification
- Case lifecycle management
- Investigation documentation
- Analyst decision-making

## Microsoft Defender XDR

- Defender XDR portal navigation
- Incident correlation
- Incident queue analysis
- Attack-story analysis
- Alert evidence review
- Evidence and entity investigation
- Device investigation
- User investigation
- Device Timeline analysis
- User Timeline analysis
- Process-tree analysis
- Incident lifecycle management
- Response-action evaluation

## Endpoint Security

- Microsoft Defender for Endpoint
- Windows endpoint onboarding
- Defender sensor validation
- EDR telemetry analysis
- Device investigation
- Process analysis
- Parent-child process analysis
- Process lineage
- Command-line analysis
- Endpoint timeline reconstruction
- Endpoint response awareness

## Threat Hunting

- Microsoft Defender Advanced Hunting
- Kusto Query Language
- `DeviceProcessEvents`
- Process telemetry investigation
- Command-line investigation
- Time-based filtering
- Device-based filtering
- Process-based filtering
- Field projection
- Chronological analysis
- Hypothesis-driven investigation
- Evidence validation

## Cloud Security

- Microsoft Azure
- Microsoft Entra ID
- Microsoft Sentinel
- Azure Log Analytics
- Cloud security architecture
- Security-service integration
- Cloud resource configuration

## Incident Response

- Detection validation
- Investigation
- Context development
- Evidence collection
- Scoping
- Classification
- Containment evaluation
- Remediation evaluation
- Secure cleanup
- Incident resolution
- Documentation

## Security Engineering

- Security-platform deployment
- Endpoint integration
- Service validation
- Licensing and dependency troubleshooting
- Telemetry-pipeline validation
- SIEM, XDR, and EDR integration
- Security architecture validation

---

# Combined SOC Investigation Methodology

The combined Week 17 investigation methodology can be summarized as:

```text
1. Build Security Environment
      │
      ▼
2. Configure Security Services
      │
      ▼
3. Onboard Endpoint
      │
      ▼
4. Validate Sensor & Telemetry
      │
      ▼
5. Generate Controlled Activity
      │
      ▼
6. Detect
      │
      ▼
7. Validate Detection
      │
      ▼
8. Identify Entities
      │
      ▼
9. Establish Context
      │
      ▼
10. Investigate Endpoint
      │
      ▼
11. Analyze Process Lineage
      │
      ▼
12. Review Device Timeline
      │
      ▼
13. Investigate Identity Context
      │
      ▼
14. Review User Timeline
      │
      ▼
15. Hunt Telemetry with KQL
      │
      ▼
16. Correlate Evidence
      │
      ▼
17. Determine Scope
      │
      ▼
18. Develop Analyst Verdict
      │
      ▼
19. Classify Activity
      │
      ▼
20. Evaluate Response
      │
      ▼
21. Contain / Remediate if Required
      │
      ▼
22. Perform Secure Cleanup
      │
      ▼
23. Document & Resolve
```

---

# Week 17 Outcome

Week 17 progressed beyond simply learning how to navigate individual Microsoft security products.

The labs established an end-to-end Microsoft Security Operations workflow.

```text
Building the Cloud Security Environment
                  │
                  ▼
Configuring Microsoft Entra ID
                  │
                  ▼
Deploying Azure Resources
                  │
                  ▼
Deploying Log Analytics
                  │
                  ▼
Enabling Microsoft Sentinel
                  │
                  ▼
Configuring Defender for Endpoint
                  │
                  ▼
Onboarding an Enterprise Endpoint
                  │
                  ▼
Validating Defender Services
                  │
                  ▼
Validating Endpoint Telemetry
                  │
                  ▼
Generating Controlled Security Activity
                  │
                  ▼
Investigating an EDR Detection
                  │
                  ▼
Analyzing Process Execution
                  │
                  ▼
Reconstructing Endpoint Activity
                  │
                  ▼
Hunting Raw Telemetry with KQL
                  │
                  ▼
Understanding Defender XDR Correlation
                  │
                  ▼
Navigating the Incident Queue
                  │
                  ▼
Investigating Incident Attack Story
                  │
                  ▼
Investigating Alerts & Evidence
                  │
                  ▼
Pivoting Across Security Entities
                  │
                  ▼
Investigating Device Context
                  │
                  ▼
Reviewing Device Timeline
                  │
                  ▼
Analyzing Process Lineage
                  │
                  ▼
Investigating Identity Context
                  │
                  ▼
Reviewing User Timeline
                  │
                  ▼
Correlating Endpoint & Identity Evidence
                  │
                  ▼
Determining Incident Scope
                  │
                  ▼
Developing an Analyst Verdict
                  │
                  ▼
Reviewing Response Capabilities
                  │
                  ▼
Classifying & Resolving Activity
                  │
                  ▼
Performing Secure Cleanup
                  │
                  ▼
Documenting the Investigation
```

By completing these labs, I developed hands-on experience across the major layers of a Microsoft-focused Security Operations environment:

```text
Identity
   +
Cloud Infrastructure
   +
Centralized Logging
   +
SIEM
   +
XDR
   +
EDR
   +
Endpoint Telemetry
   +
Threat Hunting
   +
Entity Investigation
   +
Incident Investigation
   +
Incident Response
```

The resulting environment provides a foundation for more advanced exercises involving:

- Microsoft Sentinel analytics
- KQL threat hunting
- Microsoft Defender XDR investigations
- Endpoint attack simulation
- Identity investigations
- Detection engineering
- Incident response
- Endpoint containment
- Cross-source security correlation
- Threat hunting
- Security automation
- SOC investigation workflows

---

# Final Takeaway

The most important lesson from Week 17 was not simply how to navigate Microsoft Defender, deploy Microsoft Sentinel, or execute a KQL query.

It was understanding how the individual layers of a modern Security Operations environment connect.

```text
Telemetry creates visibility.
          │
          ▼
Detection creates attention.
          │
          ▼
Correlation creates context.
          │
          ▼
Investigation creates understanding.
          │
          ▼
Evidence supports the verdict.
          │
          ▼
Response reduces risk.
```

A security alert is not the final answer.

It is a signal that requires context.

The analyst must determine:

```text
What happened?

Who or what was involved?

What happened before and after it?

How far did the activity extend?

What does the evidence actually support?

What action should be taken?
```

That process transforms raw security telemetry into an evidence-based security decision.

**The alert is where the investigation begins—not where it ends.**
