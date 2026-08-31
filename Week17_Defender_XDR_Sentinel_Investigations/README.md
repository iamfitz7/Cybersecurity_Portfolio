# Week 17 — Microsoft Defender XDR, Microsoft Sentinel, Endpoint, Identity & Detection Engineering

> **Focus:** Microsoft Defender XDR • Microsoft Defender for Endpoint • Microsoft Sentinel • Microsoft Entra ID • Azure Log Analytics • Kusto Query Language (KQL) • EDR • XDR • SIEM • Endpoint Security • Identity Security • Threat Hunting • Detection Engineering • Incident Investigation • Incident Response • PowerShell Investigation • Process Analysis • File Analysis • Network Analysis • Security Engineering • MITRE ATT&CK

---

# Executive Summary

Week 17 focused on building, operating, testing, troubleshooting, and investigating a Microsoft-based cybersecurity monitoring environment from the ground up.

Rather than beginning with an already configured SIEM or EDR environment, I worked through the security lifecycle from infrastructure and telemetry collection through detection, investigation, correlation, classification, and response decision-making.

The week consisted of five connected hands-on labs:

| Lab | Project | Primary Focus |
| --- | --- | --- |
| **Lab 01** | Microsoft Security Environment Setup & EDR Investigation | Azure, Defender for Endpoint, endpoint onboarding, telemetry validation, process investigation |
| **Lab 02** | Microsoft Defender XDR Fundamentals | Alerts, incidents, devices, users, evidence, timelines, process lineage, response actions |
| **Lab 03** | Microsoft Sentinel & KQL Fundamentals | SIEM architecture, Azure Activity, Log Analytics, KQL, analytics rules, MITRE ATT&CK |
| **Lab 04** | Microsoft Entra Failed Login Investigation | Identity telemetry, `SigninLogs`, authentication analysis, detection engineering, incident investigation |
| **Lab 05** | Suspicious PowerShell Endpoint Investigation & Response | Advanced Hunting, encoded PowerShell, process ancestry, file/network telemetry, incident validation, containment assessment |

Together, the labs covered the following security workflow:

```text
BUILD SECURITY INFRASTRUCTURE
        ↓
CONNECT SECURITY DATA
        ↓
ONBOARD ENDPOINT
        ↓
VALIDATE SECURITY SERVICES
        ↓
GENERATE CONTROLLED ACTIVITY
        ↓
VALIDATE TELEMETRY
        ↓
QUERY RAW EVENTS
        ↓
HUNT FOR SUSPICIOUS BEHAVIOR
        ↓
BUILD DETECTION LOGIC
        ↓
GENERATE ALERT
        ↓
CREATE INCIDENT
        ↓
INVESTIGATE ENTITIES
        ↓
RECONSTRUCT TIMELINE
        ↓
CORRELATE PROCESS / FILE / NETWORK / IDENTITY EVIDENCE
        ↓
ASSESS RISK
        ↓
EVALUATE CONTAINMENT
        ↓
MAKE EVIDENCE-BASED DECISION
        ↓
DOCUMENT & RESOLVE
```

The most important lesson from Week 17 was that security tools do not make the final investigation decision.

They provide telemetry, detections, alerts, incidents, and response capabilities.

The security professional still has to determine:

> **What actually happened, what does the evidence mean, and what action is justified?**

---

# Why This Project Matters

This project demonstrates practical skills relevant across several cybersecurity career paths.

These include:

- Cybersecurity Analyst
- SOC Analyst
- Security Operations Analyst
- Incident Response Analyst
- Junior Incident Responder
- Cyber Defense Analyst
- Blue Team Analyst
- Endpoint Security Analyst
- Endpoint Detection and Response Analyst
- Threat Detection Analyst
- Threat Hunter
- Detection Analyst
- Detection Engineer
- SIEM Engineer
- Security Engineer
- Security Operations Engineer
- Cloud Security Analyst
- Identity Security Analyst
- Microsoft Security Analyst
- Junior Security Engineer

The project was intentionally built around concepts that transfer between security products.

These concepts included:

- Processes
- Parent processes
- Child processes
- Command lines
- Process IDs
- Files
- File paths
- File hashes
- Users
- Authentication
- Identity
- IP addresses
- Domains
- Ports
- Network connections
- Registry activity
- Logs
- Telemetry
- Detection logic
- Alerts
- Incidents
- Timelines
- Evidence
- Risk
- Containment
- Response

Understanding these concepts makes it easier to work across different SIEM, EDR, XDR, cloud, and endpoint security platforms.

---

# Week 17 Security Progression

The five labs were intentionally connected.

## Lab 01

> **Can I build and validate the Microsoft security environment?**

## Lab 02

> **Can I investigate security activity using Microsoft Defender XDR?**

## Lab 03

> **Can I understand where SIEM telemetry comes from and analyze it using KQL?**

## Lab 04

> **Can I investigate suspicious authentication, convert hunting logic into a detection, generate an incident, and make a defensible analyst decision?**

## Lab 05

> **Can I reconstruct suspicious PowerShell activity across process, file, network, user, timeline, and incident evidence and determine whether endpoint containment is justified?**

The overall progression was:

```text
SECURITY INFRASTRUCTURE
        ↓
ENDPOINT ONBOARDING
        ↓
ENDPOINT TELEMETRY
        ↓
EDR DETECTION
        ↓
XDR INVESTIGATION
        ↓
CLOUD TELEMETRY
        ↓
SIEM ANALYSIS
        ↓
KQL HUNTING
        ↓
IDENTITY TELEMETRY
        ↓
DETECTION ENGINEERING
        ↓
ALERT GENERATION
        ↓
INCIDENT INVESTIGATION
        ↓
ENDPOINT THREAT HUNTING
        ↓
PROCESS ANCESTRY
        ↓
FILE CORRELATION
        ↓
NETWORK CORRELATION
        ↓
RESPONSE ASSESSMENT
        ↓
EVIDENCE-BASED DISPOSITION
```

---

# Week 17 Objectives

The main objectives were to:

- Build a functional Microsoft cloud security lab
- Configure Microsoft Sentinel
- Configure Azure Log Analytics
- Configure Microsoft Defender XDR
- Configure Microsoft Defender for Endpoint
- Establish a Microsoft Entra ID identity environment
- Onboard a Windows 11 Enterprise endpoint
- Validate Defender for Endpoint services
- Troubleshoot endpoint onboarding
- Validate endpoint telemetry
- Generate controlled endpoint activity
- Investigate Defender alerts
- Investigate Defender incidents
- Understand alerts versus incidents
- Investigate device entities
- Investigate user entities
- Review Device Timeline activity
- Analyze process execution
- Analyze process ancestry
- Analyze command lines
- Correlate process IDs
- Investigate encoded PowerShell
- Investigate file creation
- Investigate network communication
- Investigate Registry activity
- Use Microsoft Defender Advanced Hunting
- Query endpoint telemetry with KQL
- Configure Microsoft Sentinel telemetry ingestion
- Understand data connectors
- Understand diagnostic settings
- Investigate `AzureActivity`
- Investigate `SigninLogs`
- Use `where`, `project`, `summarize`, `count()`, and `dcount()`
- Reconstruct authentication timelines
- Analyze authentication failure reasons
- Compare successful and failed authentication
- Build a scheduled Sentinel analytics rule
- Configure detection thresholds
- Configure rule scheduling
- Configure entity mappings
- Add alert enrichment
- Map behavior to MITRE ATT&CK
- Test detection logic
- Troubleshoot a detection false negative
- Correct detection logic
- Generate an alert
- Generate an incident
- Validate incident evidence
- Distinguish telemetry from alerts
- Distinguish suspicious behavior from compromise
- Evaluate endpoint response capabilities
- Make evidence-based containment decisions
- Document professional investigation findings

---

# Security Architecture

## Cloud & Identity Layer

```text
Microsoft Entra ID
        │
        ├── Users
        ├── Authentication
        ├── Identity
        └── Administrative Access
        │
        ▼
Microsoft Azure
        │
        ▼
RG-Microsoft-Security-Lab
        │
        ▼
LAW-Microsoft-Security-Lab
        │
        ▼
Microsoft Sentinel
        │
        ├── Data Connectors
        ├── Logs
        ├── Analytics
        ├── Incidents
        ├── Hunting
        ├── Workbooks
        ├── Watchlists
        └── Automation
```

## Endpoint Layer

```text
Windows 11 Enterprise
        │
        ▼
Microsoft Defender for Endpoint
        │
        ▼
Endpoint Telemetry
        │
        ▼
Microsoft Defender XDR
        │
        ├── Alerts
        ├── Incidents
        ├── Devices
        ├── Users
        ├── Evidence
        ├── Entities
        ├── Device Timeline
        ├── Advanced Hunting
        └── Response Actions
```

## SIEM / Detection Layer

```text
Security Activity
        ↓
Data Source
        ↓
Connector / Diagnostic Setting
        ↓
Log Analytics
        ↓
Security Table
        ↓
KQL
        ↓
Detection Logic
        ↓
Analytics Rule
        ↓
Alert
        ↓
Incident
        ↓
Investigation
        ↓
Response Decision
```

---

# Technologies & Platforms

| Technology | Purpose |
| --- | --- |
| **Microsoft Defender XDR** | Unified investigation, detection, evidence correlation, and incident response |
| **Microsoft Defender for Endpoint** | Endpoint telemetry, EDR, investigation, hunting, and response |
| **Microsoft Sentinel** | Cloud-native SIEM, detection, analytics, hunting, and incident management |
| **Microsoft Entra ID** | Identity and authentication telemetry |
| **Azure Log Analytics** | Central telemetry storage and KQL querying |
| **Azure Activity Log** | Azure control-plane and administrative activity |
| **Microsoft Defender Advanced Hunting** | Cross-telemetry endpoint investigation |
| **KQL** | Security hunting, filtering, aggregation, correlation, and detection logic |
| **Windows 11 Enterprise** | Monitored enterprise endpoint |
| **PowerShell** | Controlled testing and endpoint activity generation |
| **Oracle VirtualBox** | Virtualization platform |
| **MITRE ATT&CK** | Standardized behavior and technique mapping |

---

# Core Security Tables Used

| Table | Investigation Purpose |
| --- | --- |
| `DeviceInfo` | Endpoint inventory and device information |
| `DeviceProcessEvents` | Process execution, command lines, users, process IDs, and ancestry |
| `DeviceFileEvents` | File creation and filesystem activity |
| `DeviceNetworkEvents` | Network connections, domains, IPs, ports, and protocols |
| `DeviceRegistryEvents` | Registry activity and possible persistence evidence |
| `DeviceEvents` | Additional endpoint security telemetry |
| `AzureActivity` | Azure administrative and control-plane activity |
| `SigninLogs` | Microsoft Entra interactive authentication |
| `SecurityAlert` | Security alert telemetry |
| `SecurityIncident` | Microsoft Sentinel incident information |

---

# Lab 01 — Microsoft Security Environment Setup & EDR Investigation

## Objective

Build the Microsoft security infrastructure required for later investigations and validate the environment through a controlled endpoint-security exercise.

The objective was not simply:

> **Turn Microsoft Defender on.**

The objective was to understand the endpoint security pipeline:

```text
Windows Endpoint
        ↓
Defender for Endpoint Sensor
        ↓
Endpoint Telemetry
        ↓
Microsoft Cloud Analytics
        ↓
Detection Logic
        ↓
Defender Alert
        ↓
Analyst Investigation
```

---

## Environment Setup

The implementation included:

1. Preparing the Windows endpoint
2. Establishing the Microsoft Entra environment
3. Configuring Microsoft Azure
4. Creating `RG-Microsoft-Security-Lab`
5. Creating `LAW-Microsoft-Security-Lab`
6. Enabling Microsoft Sentinel
7. Configuring Microsoft Defender XDR
8. Configuring Microsoft Defender for Endpoint
9. Onboarding Windows 11 Enterprise
10. Validating Microsoft Defender services
11. Confirming the endpoint in Device Inventory
12. Generating controlled security activity
13. Investigating resulting telemetry
14. Reviewing process execution
15. Reviewing process lineage
16. Reviewing Device Timeline
17. Hunting endpoint telemetry with KQL
18. Correlating alerts with raw telemetry
19. Classifying the activity

---

## Endpoint Onboarding Troubleshooting

During the initial onboarding process, the Microsoft Defender for Endpoint `SENSE` service was unavailable.

I checked the service using:

```powershell
sc query sense
```

I validated the Windows edition using:

```powershell
DISM /Online /Get-CurrentEdition
```

I also investigated Defender for Endpoint capability information:

```powershell
DISM.EXE /Online /Get-CapabilityInfo /CapabilityName:Microsoft.Windows.Sense.Client~~~~
```

This reinforced an important security engineering lesson:

> A security product depends on operating-system capabilities, services, licensing, connectivity, permissions, configuration, and telemetry.

Troubleshooting therefore requires identifying which dependency is failing rather than repeatedly attempting the same command.

---

## Controlled Detection Testing

After successfully onboarding the endpoint, I generated controlled Defender test activity.

```text
Controlled Activity
        ↓
Endpoint Sensor
        ↓
Process Telemetry
        ↓
Defender Cloud Analytics
        ↓
Detection
        ↓
Security Alert
        ↓
Investigation
```

The resulting investigation included suspicious PowerShell activity.

---

## Process Tree Analysis

The observed execution chain included:

```text
userinit.exe
     ↓
explorer.exe
     ↓
cmd.exe
     ↓
powershell.exe
```

Instead of analyzing `powershell.exe` by itself, I reviewed:

- Parent process
- User context
- Command line
- Execution order
- Related activity

This demonstrated why process lineage matters during endpoint investigations.

---

## Advanced Hunting

I used Microsoft Defender Advanced Hunting to validate the underlying endpoint telemetry.

Example:

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

This demonstrated that I could independently validate what happened instead of trusting the alert title alone.

---

## Lab 01 Final Classification

```text
Detection Result:
VALID DETECTION

Activity:
AUTHORIZED SECURITY TESTING

Compromise:
NO

Disposition:
EXPECTED / SECURITY TESTING
```

### Main Lesson

> **A detection can correctly identify suspicious-looking behavior even when the behavior was intentionally generated.**

Detection accuracy and malicious intent are different questions.

---

# Lab 02 — Microsoft Defender XDR Fundamentals

## Objective

Develop practical familiarity with Microsoft Defender XDR and understand how security investigations move from high-level incidents into alerts, devices, users, processes, evidence, timelines, and response actions.

---

## Investigation Workflow

```text
Defender XDR
        ↓
Incident Queue
        ↓
Incident
        ↓
Alert
        ↓
Evidence & Entities
       ↙         ↘
   Device        User
      ↓            ↓
 Timeline       Context
      ↓
Process Lineage
      ↓
Additional Evidence
      ↓
Analyst Decision
```

---

# Alert vs. Incident

## Alert

An alert represents a specific detection.

```text
Telemetry
    ↓
Detection Logic
    ↓
Alert
```

An alert may contain:

- Alert title
- Severity
- Detection source
- User
- Device
- Process
- File
- IP address
- Timestamp
- Evidence
- MITRE ATT&CK information

> **An alert is a reason to investigate. It is not automatically proof of compromise.**

---

## Incident

An incident represents a broader security case.

```text
Alert 1 ─────┐
              │
Alert 2 ──────┼────► Incident
              │
Alert 3 ─────┘
```

An incident may connect:

- Alerts
- Users
- Devices
- Processes
- Files
- IP addresses
- Evidence
- Investigation context
- Response actions

The investigation therefore moves from:

> **What does this alert say?**

to:

> **What happened across the environment and how are the signals connected?**

---

# Device Investigation

Device entity pages were reviewed for:

- Device identity
- Operating system
- Risk
- Exposure
- Logged-on users
- Alerts
- Incidents
- Software
- Security recommendations
- Timeline
- Response options

---

# Device Timeline

Device Timeline helped reconstruct activity chronologically.

```text
Activity Before Detection
        ↓
Process Execution
        ↓
Related Activity
        ↓
Detection
        ↓
Post-Detection Activity
```

This helps answer:

- What happened first?
- What caused the detection?
- What happened immediately beforehand?
- What happened afterward?
- Did activity continue?
- Did additional processes become involved?

---

# Process Lineage

A suspicious process should not automatically be investigated in isolation.

Instead of:

```text
powershell.exe
```

the investigation should consider:

```text
Parent Process
      ↓
Process
      ↓
Command Line
      ↓
Child Processes
      ↓
Files
      ↓
Network
      ↓
Registry
```

This provides substantially more context.

---

# User Investigation

User entity information was reviewed to understand:

- Identity
- Related alerts
- Associated devices
- Activity
- Related evidence
- Whether the behavior matched expected activity

This became especially important during the identity investigation in Lab 04.

---

# Response Capabilities Reviewed

Microsoft Defender response capabilities included:

- Isolate device
- Run antivirus scan
- Collect investigation package
- Initiate Live Response
- Restrict application execution
- Investigate device
- Review remediation activity

These capabilities were evaluated but not automatically executed.

> **The existence of a response action does not mean it should immediately be used.**

Response should match:

- Evidence
- Severity
- Scope
- Confidence
- Ongoing risk
- Business impact

---

# Lab 02 Major Lesson

```text
ALERT
   ↓
INVESTIGATE
   ↓
CORRELATE
   ↓
VALIDATE
   ↓
DECIDE
```

not:

```text
ALERT
   ↓
ASSUME COMPROMISE
```

---

# Lab 03 — Microsoft Sentinel & KQL Fundamentals

## Objective

Develop practical experience with the Microsoft Sentinel SIEM data pipeline and understand how cloud activity becomes searchable telemetry and eventually detection logic.

The main question was:

> **Can I understand where SIEM data comes from, validate that it exists, query it, identify patterns, and understand how those queries become detections?**

---

# Lab 03 Data Pipeline

```text
Generate Azure Activity
        ↓
Verify Source Activity
        ↓
Azure Activity Connector
        ↓
Log Analytics
        ↓
AzureActivity
        ↓
KQL
        ↓
Filtering
        ↓
Aggregation
        ↓
Behavior Analysis
        ↓
Analytics Rule
        ↓
MITRE ATT&CK
        ↓
Alert
        ↓
Incident
```

---

# Lab Environment

```text
Microsoft Azure
        │
        ├── RG-Microsoft-Security-Lab
        │
        └── LAW-Microsoft-Security-Lab
                    │
                    ▼
             Microsoft Sentinel
                    │
                    ├── Azure Activity
                    ├── Logs
                    ├── Analytics
                    ├── Incidents
                    ├── Workbooks
                    └── Watchlists
```

A temporary resource group was used to safely generate observable Azure administrative telemetry:

```text
rg-sentinel-kql-lab3-test
```

---

# Understanding the SIEM Data Pipeline

One of the most important lessons was that enabling Sentinel does not automatically mean every security event is available.

```text
Activity
   ↓
Data Source
   ↓
Connector
   ↓
Log Analytics
   ↓
Table
   ↓
KQL
   ↓
Detection / Investigation
```

Possible failure points include:

```text
No telemetry
    ↓
Nothing to ingest
```

```text
Connector not configured
    ↓
Telemetry does not reach workspace
```

```text
Wrong table
    ↓
Query returns nothing useful
```

```text
Wrong schema understanding
    ↓
Investigation logic fails
```

---

# Log Analytics vs. Microsoft Sentinel

## Log Analytics

Log Analytics stores and queries telemetry.

```text
Log Analytics
    ├── Tables
    ├── Rows
    ├── Columns
    ├── Retention
    └── Queries
```

## Microsoft Sentinel

Sentinel adds security operations functionality.

```text
Microsoft Sentinel
    ├── Data Connectors
    ├── Analytics Rules
    ├── Hunting
    ├── Incidents
    ├── Workbooks
    ├── Watchlists
    └── Automation
```

Relationship:

```text
Telemetry
    ↓
Log Analytics
    ↓
Structured Data
    ↓
Microsoft Sentinel
    ↓
Detect
Investigate
Hunt
Respond
```

---

# Azure Activity Investigation

Important fields included:

- `TimeGenerated`
- `OperationNameValue`
- `ActivityStatusValue`
- `ResourceGroup`
- `Caller`
- `CallerIpAddress`
- `SubscriptionId`
- `ResourceId`
- `CategoryValue`

---

# KQL Fundamentals

## Basic Query

```kusto
AzureActivity
| take 10
```

## Selecting Useful Fields

```kusto
AzureActivity
| project
    TimeGenerated,
    OperationNameValue,
    ActivityStatusValue,
    ResourceGroup,
    Caller,
    CallerIpAddress
| take 20
```

## Time Filtering

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
```

## Resource Filtering

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
| where ResourceGroup =~ "rg-sentinel-kql-lab3-test"
| project
    TimeGenerated,
    OperationNameValue,
    ActivityStatusValue,
    ResourceGroup,
    Caller
| order by TimeGenerated desc
```

## Counting Events

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
| count
```

## Status Aggregation

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
| summarize EventCount = count() by ActivityStatusValue
| order by EventCount desc
```

## Top Operations

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
| summarize EventCount = count() by OperationNameValue
| top 10 by EventCount desc
```

These queries moved the analysis from:

> **What does one event say?**

to:

> **What pattern does the collection of events show?**

---

# Sentinel Analytics Rules

A scheduled analytics rule can be understood as:

```text
Security Telemetry
       ↓
Scheduled KQL Query
       ↓
Detection Condition
       │
       ├── Not Met → Continue Monitoring
       │
       └── Met
             ↓
           Alert
             ↓
          Incident
```

A rule can include:

- Name
- Description
- Severity
- Required data sources
- KQL
- Query frequency
- Lookback period
- Threshold
- Entity mappings
- Incident settings
- MITRE ATT&CK mapping
- Automated response

---

# MITRE ATT&CK

A Sentinel rule related to suspicious resource deployment was reviewed.

It mapped to:

```text
Tactic:
Impact

Technique:
T1496 — Resource Hijacking
```

The important lesson was:

> **MITRE ATT&CK provides standardized language for behavior. It does not prove attacker intent.**

---

# Lab 03 Final Result

```text
Microsoft Sentinel:
CONFIGURED

Log Analytics:
VALIDATED

Azure Activity:
INGESTED

AzureActivity:
QUERYABLE

KQL:
SUCCESSFULLY USED

Aggregation:
COMPLETED

Analytics Rules:
REVIEWED

MITRE ATT&CK:
APPLIED

SIEM DATA PIPELINE:
UNDERSTOOD & VALIDATED
```

---

# Lab 04 — Microsoft Entra Failed Login Investigation, Detection Engineering & Incident Response

## Objective

Investigate repeated Microsoft Entra failed sign-ins from beginning to end and determine whether the authentication behavior represented normal activity, authorized testing, or a possible identity attack.

Lab 04 combined:

```text
IDENTITY
   +
AUTHENTICATION
   +
LOG INGESTION
   +
KQL
   +
SIEM
   +
DETECTION ENGINEERING
   +
ALERT TRIAGE
   +
INCIDENT INVESTIGATION
   +
ANALYST JUDGMENT
```

The central question was:

> **Can I investigate failed authentication using raw evidence, build detection logic around the behavior, and make a final decision supported by evidence?**

---

# Security Scenario

Repeated authentication failures may represent:

- User password mistakes
- Forgotten passwords
- Expired credentials
- Applications using stale credentials
- Authentication configuration problems
- Password guessing
- Brute force
- Password spraying
- Credential stuffing
- Authorized testing

Therefore:

```text
FAILED LOGIN
    ≠
CONFIRMED ATTACK
```

A stronger investigation asks:

```text
WHO?
Which account?

FROM WHERE?
Which IP?

WHEN?
Which timestamps?

HOW OFTEN?
How many attempts?

HOW QUICKLY?
Seconds, minutes, or hours?

WHAT APPLICATION?
What was accessed?

WHY?
Why did authentication fail?

WHAT HAPPENED NEXT?
More failures or eventual success?

WHAT IS THE CONTEXT?
Known user? Known IP? Controlled test?

WHAT SHOULD HAPPEN?
Close? Monitor? Escalate? Contain?
```

---

# Identity Telemetry Architecture

```text
Microsoft Entra ID
        ↓
Interactive Sign-In Activity
        ↓
SignInLogs Diagnostic Category
        ↓
Entra Diagnostic Setting
        ↓
LAW-Microsoft-Security-Lab
        ↓
SigninLogs
        ↓
Microsoft Sentinel
        ↓
KQL
        ↓
Scheduled Analytics Rule
        ↓
Alert
        ↓
Microsoft Defender Incident
        ↓
Investigation
        ↓
Disposition
```

---

# Phase 1 — Testing Telemetry

I first tested:

```kusto
SigninLogs
| take 10
```

Initially, no records were returned.

I then checked which tables contained data:

```kusto
search *
| summarize Events=count() by $table
| sort by Events desc
```

The workspace contained other telemetry, but `SigninLogs` was not being populated.

This established:

```text
Log Analytics:
WORKING

Sentinel:
WORKING

Other Telemetry:
WORKING

Entra Sign-In Telemetry:
MISSING
```

The lesson:

> **A query cannot investigate data that was never collected.**

---

# Phase 2 — Configuring Entra Diagnostic Settings

Microsoft Entra diagnostic settings were configured to send:

```text
AuditLogs
SigninLogs
```

to:

```text
LAW-Microsoft-Security-Lab
```

The pipeline became:

```text
Microsoft Entra
      ↓
Sign-In Activity
      ↓
Diagnostic Settings
      ↓
Log Analytics
      ↓
SigninLogs
      ↓
Microsoft Sentinel
```

Another important lesson:

> **Configuration is not validation.**

I still needed to prove that telemetry arrived.

---

# Phase 3 — Controlled Authentication Activity

A dedicated lab account was used to safely generate known failed authentication.

```text
Controlled Failed Login
        ↓
Microsoft Entra
        ↓
Diagnostic Setting
        ↓
Log Analytics
        ↓
SigninLogs
        ↓
Investigation
```

This separated two questions:

> **Did the authentication happen?**

from:

> **Did the detection identify it?**

---

# Phase 4 — Initial Investigation

```kusto
SigninLogs
| where ResultType != 0
| project
    TimeGenerated,
    UserPrincipalName,
    UserDisplayName,
    IPAddress,
    Location,
    AppDisplayName,
    ResourceDisplayName,
    ClientAppUsed,
    ResultType,
    ResultDescription,
    ConditionalAccessStatus,
    AuthenticationRequirement
| sort by TimeGenerated desc
```

Important investigation fields included:

| Field | Question Answered |
| --- | --- |
| `TimeGenerated` | When did it happen? |
| `UserPrincipalName` | Which identity? |
| `IPAddress` | Which source IP? |
| `Location` | What geographic context exists? |
| `AppDisplayName` | Which application? |
| `ResultType` | Success or failure? |
| `ResultDescription` | Why did authentication fail? |

---

# Failed Sign-ins by User

```kusto
SigninLogs
| where ResultType != 0
| summarize FailedSignins = count() by UserPrincipalName
| sort by FailedSignins desc
```

This changed the question from:

> Failed logins exist.

to:

> Which identities are experiencing them?

---

# Failed Sign-ins by IP

```kusto
SigninLogs
| where ResultType != 0
| summarize
    FailedSignins = count(),
    TargetedUsers = dcount(UserPrincipalName)
    by IPAddress
| sort by FailedSignins desc
```

This allowed me to distinguish:

```text
1 IP
100 failures
1 user
```

from:

```text
1 IP
100 failures
50 users
```

The second pattern may be more consistent with password spraying.

Neither proves an attack without additional evidence.

---

# Authentication Timeline

```kusto
let TargetUser = "<LAB-ACCOUNT>";
SigninLogs
| where UserPrincipalName =~ TargetUser
| project
    TimeGenerated,
    UserPrincipalName,
    IPAddress,
    Location,
    AppDisplayName,
    ClientAppUsed,
    ResultType,
    ResultDescription
| sort by TimeGenerated asc
```

Chronological reconstruction helped determine:

- What happened first
- How quickly attempts occurred
- Whether failures continued
- Whether authentication eventually succeeded

---

# Success vs. Failure Correlation

```kusto
let TargetUser = "<LAB-ACCOUNT>";
SigninLogs
| where UserPrincipalName =~ TargetUser
| extend Outcome = iff(ResultType == 0, "SUCCESS", "FAILURE")
| project
    TimeGenerated,
    Outcome,
    UserPrincipalName,
    IPAddress,
    Location,
    AppDisplayName,
    ResultType,
    ResultDescription
| sort by TimeGenerated asc
```

Compare:

```text
FAIL
FAIL
FAIL
```

with:

```text
FAIL
FAIL
FAIL
SUCCESS
```

The second deserves additional investigation.

---

# Failure Reason Analysis

```kusto
SigninLogs
| where ResultType != 0
| summarize
    FailureCount = count()
    by ResultType, ResultDescription
| sort by FailureCount desc
```

This moved the analysis from:

```text
Authentication Failed
```

to:

```text
WHY did authentication fail?
```

---

# Creating the Detection

I created:

```text
Repeated Failed Microsoft Entra Sign-ins
```

Configuration:

```text
Severity:
Medium

Rule Type:
Scheduled

MITRE ATT&CK:
T1110 — Brute Force

Run Frequency:
5 minutes

Lookback:
10 minutes

Incident Creation:
Enabled
```

---

# Entity Mapping

## Account

```text
Name
→ AccountName

UPNSuffix
→ AccountUPNSuffix
```

## IP

```text
Address
→ IPAddress
```

Custom alert details included:

```text
FailedAttempts
FirstAttempt
LastAttempt
```

---

# Detection Engineering Failure

The first version used:

```kusto
| summarize
    FailedAttempts = count()
    by UserPrincipalName, IPAddress, bin(TimeGenerated, 5m)
| where FailedAttempts >= 3
```

It returned:

```text
0 results
```

even though the authentication failures existed.

Instead of assuming Sentinel was broken, I returned to the raw telemetry.

```text
DATA INGESTION:
WORKING

SIGNINLOGS:
WORKING

FAILED LOGIN EVENTS:
PRESENT

DETECTION:
NOT MATCHING
```

Therefore, the problem was the detection logic.

---

# Root Cause — Time Bucket Boundary

The attempts crossed a fixed five-minute boundary.

Conceptually:

```text
8:29:59 → Bucket 1
8:32:11 → Bucket 2
8:33:19 → Bucket 2
```

The query saw:

```text
8:25–8:30
1 failure

8:30–8:35
2 failures
```

The threshold required:

```text
FailedAttempts >= 3
```

Neither bucket contained three failures.

This produced one of the most important lessons of Week 17:

> **A detection query can be syntactically correct and still be behaviorally wrong.**

---

# Corrected Detection

```kusto
SigninLogs
| where ResultType != 0
| extend
    AccountName = tostring(split(UserPrincipalName, "@")[0]),
    AccountUPNSuffix = tostring(split(UserPrincipalName, "@")[1])
| summarize
    FailedAttempts = count(),
    FirstAttempt = min(TimeGenerated),
    LastAttempt = max(TimeGenerated)
    by UserPrincipalName, AccountName, AccountUPNSuffix, IPAddress
| where FailedAttempts >= 3
| extend TimeGenerated = LastAttempt
| project
    TimeGenerated,
    UserPrincipalName,
    AccountName,
    AccountUPNSuffix,
    IPAddress,
    FailedAttempts,
    FirstAttempt,
    LastAttempt
```

The corrected query successfully matched.

```text
TELEMETRY:
✓

QUERY:
✓

AGGREGATION:
✓

THRESHOLD:
✓

ENTITY FIELDS:
✓

DETECTION:
✓
```

---

# Alert & Incident Generation

Fresh failed authentication generated:

```text
Failed Login
      ↓
Failed Login
      ↓
Failed Login
      ↓
SigninLogs
      ↓
Scheduled Sentinel Rule
      ↓
FailedAttempts >= 3
      ↓
Detection Match
      ↓
Alert
      ↓
Incident
```

The resulting incident was:

```text
Repeated Failed Microsoft Entra Sign-ins
```

with:

```text
Severity:
Medium
```

---

# Incident Investigation

I reviewed:

- Incident name
- Severity
- Status
- Alert
- Account entity
- Source IP
- Detection evidence
- Failed attempt count
- Authentication timing
- Query information
- Incident activity

The key distinction became:

```text
DETECTION QUESTION

Did repeated failed authentication occur?
        ↓
YES
```

versus:

```text
INVESTIGATION QUESTION

Was the activity unauthorized or malicious?
        ↓
REQUIRES CONTEXT
```

---

# Lab 04 Final Decision

The failures had been intentionally generated.

```text
Did the failures happen?
YES

Did Sentinel detect them?
YES

Did the rule behave correctly?
YES

Was the activity authorized?
YES

Was compromise identified?
NO
```

Final disposition:

```text
Incident:
Repeated Failed Microsoft Entra Sign-ins

Severity:
Medium

Status:
Resolved

Classification:
Informational / Expected Confirmed Activity

Detection:
Successful

Unauthorized Compromise:
Not Identified
```

This was not simply a "false positive."

The detection correctly identified the behavior.

The investigation established that the behavior had an authorized explanation.

---

# Lab 04 Major Lessons

1. A SIEM cannot investigate telemetry it never received.
2. Always validate the source data.
3. Authentication failures require context.
4. Timeline analysis changes the meaning of events.
5. Success after repeated failures deserves additional attention.
6. Raw logs, alerts, and incidents serve different purposes.
7. Detection logic must be behaviorally tested.
8. Troubleshooting should isolate the broken layer.
9. A correct detection does not automatically mean malicious intent.

---

# Lab 04 Final Result

```text
Data Collection:
SUCCESSFUL

SigninLogs Ingestion:
SUCCESSFUL

KQL Investigation:
SUCCESSFUL

Authentication Analysis:
SUCCESSFUL

Detection Rule:
SUCCESSFUL

Detection Troubleshooting:
SUCCESSFUL

MITRE ATT&CK:
T1110 — Brute Force

Alert Generation:
SUCCESSFUL

Incident Generation:
SUCCESSFUL

Incident Investigation:
SUCCESSFUL

Final Status:
RESOLVED

Final Classification:
EXPECTED CONFIRMED ACTIVITY

Confirmed Compromise:
NO
```

---

# Lab 05 — Suspicious PowerShell Endpoint Investigation & Response

## Objective

Lab 05 expanded the endpoint investigation side of Week 17.

The central question was:

> **What did PowerShell actually do?**

Rather than assuming PowerShell was malicious because it used suspicious-looking options, I generated controlled PowerShell activity and reconstructed its behavior using Microsoft Defender endpoint telemetry.

The investigation included:

- Encoded PowerShell
- Full command lines
- User context
- Parent processes
- Child processes
- Process IDs
- File activity
- Network activity
- Registry investigation
- Device Timeline
- Existing incident validation
- Endpoint response assessment
- Containment decision-making

---

# Lab 05 Environment

```text
Operating System:
Windows 11 Enterprise

Defender Device:
desktop-3sepq1q

Local IP:
10.0.2.15

Virtualization:
Oracle VirtualBox

Network:
NAT
```

The endpoint was onboarded into Microsoft Defender for Endpoint and actively sending telemetry.

---

# Defender Protection Verification

Before generating controlled activity, I verified Defender protection.

```text
AntivirusEnabled:
True

RealTimeProtectionEnabled:
True

BehaviorMonitorEnabled:
True

IoavProtectionEnabled:
True

IsTamperProtected:
True
```

Security intelligence was also updated.

This established a known monitoring baseline before testing.

---

# Controlled PowerShell Activity

I generated harmless activity that could resemble attacker behavior during an investigation.

The controlled activity included:

1. Base64-encoded PowerShell execution
2. File creation
3. HTTPS communication
4. PowerShell launching CMD
5. CMD launching WHOAMI

No malware was intentionally downloaded or executed.

---

# Ground Truth

Known artifacts included:

```text
week17-lab5.txt
week17-example.html
week17-user.txt
```

Known processes included:

```text
powershell.exe
cmd.exe
whoami.exe
```

Known network activity included:

```text
example.com
TCP 443
```

Creating known ground truth allowed me to compare:

```text
WHAT I KNOW I DID
```

against:

```text
WHAT DEFENDER TELEMETRY SHOWS
```

---

# Encoded PowerShell

A harmless PowerShell command was converted into Base64 and executed using:

```powershell
powershell.exe -NoProfile -EncodedCommand <Base64-data>
```

The encoded command created:

```text
week17-lab5.txt
```

Important lesson:

```text
-EncodedCommand
      ≠
Automatic Malware
```

Encoding is a security signal.

It is not a final verdict.

Legitimate scripts, administrators, automation platforms, software, and attackers may all use encoded PowerShell.

---

# Advanced Hunting

The primary endpoint tables were:

```text
DeviceInfo
DeviceProcessEvents
DeviceFileEvents
DeviceNetworkEvents
DeviceRegistryEvents
DeviceEvents
```

---

# PowerShell Process Investigation

```kusto
DeviceProcessEvents
| where Timestamp > ago(1d)
| where DeviceName =~ "desktop-3sepq1q"
| where FileName in~ ("powershell.exe", "pwsh.exe")
| project
    Timestamp,
    DeviceName,
    AccountName,
    InitiatingProcessFileName,
    InitiatingProcessCommandLine,
    FileName,
    ProcessCommandLine,
    ProcessId,
    InitiatingProcessId,
    SHA1
| order by Timestamp desc
```

This exposed:

- Timestamp
- User
- Process
- Full command line
- Initiating process
- Initiating command line
- Process ID
- Initiating process ID
- Hash information

---

# Encoded PowerShell Hunting

```kusto
DeviceProcessEvents
| where Timestamp > ago(1d)
| where DeviceName =~ "desktop-3sepq1q"
| where FileName =~ "powershell.exe"
| where ProcessCommandLine contains "-EncodedCommand"
| project
    Timestamp,
    DeviceName,
    AccountName,
    InitiatingProcessFileName,
    InitiatingProcessCommandLine,
    ProcessCommandLine,
    ProcessId,
    InitiatingProcessId,
    SHA1
| order by Timestamp desc
```

Defender successfully returned:

```text
powershell.exe -NoProfile -EncodedCommand ...
```

The correct investigation model became:

```text
OBSERVATION
Encoded PowerShell occurred
        ↓
QUESTION
What was encoded?
        ↓
QUESTION
What process launched it?
        ↓
QUESTION
What happened afterward?
```

---

# Parent & Child Process Analysis

The main controlled process chain was:

```text
powershell.exe
      ↓
cmd.exe
      ↓
whoami.exe
```

Telemetry showed:

```text
InitiatingProcessFileName = powershell.exe
FileName                  = cmd.exe
```

followed by:

```text
InitiatingProcessFileName = cmd.exe
FileName                  = whoami.exe
```

This demonstrated process ancestry.

---

# Why Process Ancestry Matters

These are all legitimate Windows executables:

```text
powershell.exe
cmd.exe
whoami.exe
```

Seeing:

```text
whoami.exe
```

alone provides limited information.

Seeing:

```text
powershell.exe
      ↓
cmd.exe
      ↓
whoami.exe
```

provides much more context.

Process ancestry is valuable across:

- Incident response
- Endpoint security
- Threat hunting
- Malware investigations
- SOC investigations
- Detection engineering
- Cyber defense
- Security engineering

---

# Process ID Correlation

I also used:

```text
ProcessId
InitiatingProcessId
```

to support the parent-child relationships.

Conceptually:

```text
cmd.exe
PID 6772
    ↓
whoami.exe
InitiatingProcessId 6772
```

This strengthened the execution-chain reconstruction.

---

# Focused Process Timeline

```kusto
DeviceProcessEvents
| where Timestamp > ago(1d)
| where DeviceName =~ "desktop-3sepq1q"
| where ProcessCommandLine contains "EncodedCommand"
    or ProcessCommandLine contains "week17-user.txt"
    or ProcessCommandLine contains "whoami"
| project
    Timestamp,
    AccountName,
    InitiatingProcessFileName,
    FileName,
    ProcessCommandLine,
    ProcessId,
    InitiatingProcessId
| order by Timestamp asc
```

This reconstructed the relevant execution in chronological order.

---

# File Investigation

Using `DeviceFileEvents`, I identified:

```text
week17-example.html
```

with:

```text
ActionType:
FileCreated

InitiatingProcessFileName:
powershell.exe
```

The evidence chain became:

```text
PowerShell
    ↓
Web Request
    ↓
File Creation
    ↓
week17-example.html
```

---

# Network Investigation

PowerShell made a harmless HTTPS request to:

```text
https://example.com
```

Defender recorded:

```text
Initiating Process:
powershell.exe

Remote URL:
example.com

Remote IP:
104.20.23.154

Remote Port:
443

Protocol:
Tcp

Action:
ConnectionSuccess
```

This was a major distinction.

A command containing:

```text
https://example.com
```

shows what PowerShell was instructed to do.

`DeviceNetworkEvents` showing:

```text
example.com
104.20.23.154
443
Tcp
ConnectionSuccess
```

shows observed network behavior.

---

# File + Network Correlation

The combined evidence showed:

```text
powershell.exe
      ↓
example.com
      ↓
104.20.23.154
      ↓
TCP 443
      ↓
ConnectionSuccess
      ↓
week17-example.html
```

Different telemetry answered different questions:

```text
DeviceProcessEvents
→ What executed?

DeviceFileEvents
→ What happened to the filesystem?

DeviceNetworkEvents
→ Where did the process communicate?
```

Together, the tables produced a stronger reconstruction.

---

# Registry Investigation

I checked `DeviceRegistryEvents` for relevant PowerShell Registry modifications.

No Registry persistence associated with the controlled Lab 05 activity was identified.

This was useful negative evidence.

A complete investigation should document:

```text
WHAT WAS FOUND
```

and:

```text
WHAT WAS CHECKED BUT NOT FOUND
```

---

# Device Timeline Investigation

I reviewed the Microsoft Defender Device Timeline for:

- Process execution
- PowerShell activity
- Command lines
- Timestamps
- Process IDs
- Image paths
- Users
- Network activity
- Defender events

Not every PowerShell event belonged to Lab 05.

I only used evidence that could be correlated using:

- Timestamp
- Device
- User
- Process
- Command line
- Known artifacts
- File activity
- Network activity

---

# Existing Incident Validation

The endpoint already contained:

```text
Suspicious PowerShell command line
```

and:

```text
Execution incident on one endpoint
```

The names initially looked relevant.

However, both were from:

```text
August 6, 2026
```

while Lab 05 occurred on:

```text
August 31, 2026
```

The older execution incident had already been classified:

```text
Resolved
Benign Positive
```

Therefore:

```text
INCIDENT LOOKS RELEVANT
        ↓
CHECK TIMESTAMP
        ↓
TIMESTAMP DOES NOT MATCH
        ↓
EXCLUDE FROM LAB 05
```

This prevented incorrect evidence attribution.

---

# Telemetry vs. Alert

## Telemetry

Telemetry means:

> **The security platform recorded activity.**

Examples:

```text
PowerShell execution
Encoded command
CMD execution
WHOAMI execution
File creation
Network connection
Remote IP
Remote port
```

## Alert

An alert means:

> **Detection logic determined that activity met a suspicious condition.**

Therefore:

```text
TELEMETRY
    ≠
ALERT

ALERT
    ≠
COMPROMISE
```

Lab 05 remained a successful investigation even without validating a new incident generated by the controlled activity.

Advanced Hunting provided the evidence needed to reconstruct what happened.

---

# Endpoint Response Assessment

Available Defender response options included:

```text
Collect Investigation Package
Initiate Automated Investigation
Initiate Live Response Session
Isolate Device
Go Hunt
```

The important question was:

> **Should the endpoint actually be isolated?**

---

# Initial Reasons for Concern

The activity contained:

```text
Encoded PowerShell
        +
PowerShell child processes
        +
CMD
        +
WHOAMI
        +
File creation
        +
Outbound HTTPS communication
```

These behaviors deserve investigation.

However, the complete evidence showed:

```text
Known lab endpoint
Known user
Authorized testing
Known harmless destination
No malicious payload
No persistence
No credential theft
No lateral movement
No command-and-control
No exfiltration
No continued attacker access
```

Therefore:

```text
DEVICE ISOLATION:
NOT REQUIRED
```

---

# Why Isolation Was Not Required

Isolation is a powerful response action that can affect:

- Users
- Applications
- Business operations
- Network access
- Remote administration
- Investigation workflows

The appropriate model is:

```text
SUSPICIOUS SIGNAL
        ↓
INVESTIGATE
        ↓
CORRELATE
        ↓
ASSESS RISK
        ↓
CONTAIN IF JUSTIFIED
```

not:

```text
SUSPICIOUS SIGNAL
        ↓
AUTOMATIC ISOLATION
```

---

# What Would Increase the Need for Isolation?

Examples include:

- Malware execution
- Known malicious hash
- Credential dumping
- LSASS access
- Ransomware
- Persistence
- Malicious scheduled tasks
- Malicious service creation
- Registry persistence
- Command-and-control
- Beaconing
- Known malicious infrastructure
- Lateral movement
- Data staging
- Data exfiltration
- Continued unauthorized activity

---

# Final Lab 05 Investigation Chain

```text
AUTHORIZED LAB USER
        ↓
powershell.exe
        ↓
-EncodedCommand
        ↓
Controlled File Creation

powershell.exe
        ↓
cmd.exe
        ↓
whoami.exe

powershell.exe
        ↓
example.com
        ↓
104.20.23.154
        ↓
TCP 443
        ↓
ConnectionSuccess
        ↓
week17-example.html

        ↓

No Malicious Payload
No Persistence
No Credential Theft
No Lateral Movement
No Command-and-Control
No Exfiltration
No Continued Unauthorized Access

        ↓

BENIGN AUTHORIZED SECURITY TESTING

        ↓

NO DEVICE ISOLATION REQUIRED
```

---

# Lab 05 Final Classification

| Category | Result |
| --- | --- |
| Investigation | Suspicious PowerShell Endpoint Activity |
| Platform | Microsoft Defender XDR / Defender for Endpoint |
| Hunting | Advanced Hunting |
| Query Language | KQL |
| Endpoint | `desktop-3sepq1q` |
| Initial Assessment | Suspicious / Requires Investigation |
| Initial Risk | Medium |
| Encoded PowerShell | Confirmed |
| Child Process Activity | Confirmed |
| File Creation | Confirmed |
| Network Activity | Confirmed |
| Network Result | `ConnectionSuccess` |
| Malware | Not Identified |
| Persistence | Not Identified |
| Credential Theft | Not Identified |
| Lateral Movement | Not Identified |
| Command-and-Control | Not Identified |
| Exfiltration | Not Identified |
| Existing Incidents | Reviewed and excluded by timestamp |
| Final Classification | Benign / Authorized Security Testing |
| Final Risk | Low |
| Device Isolation | Not Required |

---

# What Did PowerShell Actually Do?

The investigation established that PowerShell:

1. Executed a controlled Base64-encoded command.
2. Created a harmless text artifact.
3. Launched `cmd.exe`.
4. Caused `cmd.exe` to launch `whoami.exe`.
5. Made an HTTPS request to `example.com`.
6. Established a successful TCP connection.
7. Connected to `104.20.23.154`.
8. Used TCP port `443`.
9. Created `week17-example.html`.
10. Did not execute a malicious payload during the controlled test.

```text
powershell.exe
      │
      ├── Encoded Controlled Command
      │
      ├── cmd.exe
      │      ↓
      │   whoami.exe
      │
      └── example.com
              ↓
        104.20.23.154
              ↓
           TCP 443
              ↓
       ConnectionSuccess
              ↓
     week17-example.html
```

---

# Lab 05 Major Lessons

## PowerShell Is Not Automatically Malicious

```text
powershell.exe
      ≠
Malware
```

The investigation must determine how PowerShell was used.

---

## Encoded PowerShell Is a Signal

```text
-EncodedCommand
      ≠
Confirmed Attack
```

It should increase investigation interest but still requires behavioral analysis.

---

## Process Ancestry Matters

```text
whoami.exe
```

provides less context than:

```text
powershell.exe
      ↓
cmd.exe
      ↓
whoami.exe
```

---

## Intent and Observed Behavior Are Different

```text
Command contains URL
        ↓
INTENT
```

versus:

```text
DeviceNetworkEvents
        ↓
ConnectionSuccess
        ↓
OBSERVED BEHAVIOR
```

---

## Negative Evidence Matters

The absence of:

- Persistence
- Credential theft
- Lateral movement
- C2
- Exfiltration

helped inform the final risk assessment.

---

## Evidence Must Match the Timeline

Existing PowerShell incidents were not attached to Lab 05 simply because their names looked relevant.

Timestamp validation showed they were unrelated.

---

## Containment Must Be Evidence-Based

Response actions should be based on:

```text
Evidence
   +
Scope
   +
Confidence
   +
Ongoing Risk
   +
Business Impact
```

---

# Cross-Lab Detection Engineering Lessons

Week 17 developed the following detection engineering process:

```text
1. Identify Behavior
        ↓
2. Identify Telemetry
        ↓
3. Identify Table
        ↓
4. Understand Schema
        ↓
5. Write KQL
        ↓
6. Test Against Data
        ↓
7. Identify False Negatives
        ↓
8. Fix Logic
        ↓
9. Set Severity
        ↓
10. Map Entities
        ↓
11. Map MITRE ATT&CK
        ↓
12. Configure Schedule
        ↓
13. Generate Alert
        ↓
14. Generate Incident
        ↓
15. Investigate
        ↓
16. Tune
```

The central lesson was:

> **A query that executes successfully is not automatically a good detection.**

Detection logic must accurately represent the behavior it is intended to identify.

---

# Week 17 Investigation Method

A repeatable investigation method emerged across all five labs:

```text
OBSERVATION
What happened?
        ↓
CONTEXT
Which user, device, process, IP, resource, file, or application?
        ↓
CORRELATION
What other evidence exists?
        ↓
TIMELINE
What happened before and after?
        ↓
BEHAVIOR
What did the system, process, or user actually do?
        ↓
INTERPRETATION
What explanation best fits the evidence?
        ↓
VALIDATION
What proves or disproves that explanation?
        ↓
RISK
Is there evidence of continued threat?
        ↓
DECISION
What should happen next?
        ↓
RESPONSE
Close, monitor, escalate, contain, isolate, or remediate?
```

---

# KQL Skills Demonstrated

```text
where
project
take
count
count()
countif()
dcount()
summarize
top
contains
order by
sort by
ago()
extend
iff()
split()
tostring()
min()
max()
bin()
in~
=~
```

The important skill was not simply memorizing syntax.

It was converting investigation questions into queries.

Examples:

```text
QUESTION:
Which users have the most failed authentication?

KQL:
summarize count() by UserPrincipalName
```

```text
QUESTION:
How many users did one IP target?

KQL:
dcount(UserPrincipalName)
```

```text
QUESTION:
Which PowerShell processes executed?

TABLE:
DeviceProcessEvents
```

```text
QUESTION:
What file did PowerShell create?

TABLE:
DeviceFileEvents
```

```text
QUESTION:
Where did PowerShell connect?

TABLE:
DeviceNetworkEvents
```

---

# Cybersecurity Skills Demonstrated

## Cybersecurity Analysis

- Security telemetry analysis
- Endpoint analysis
- Cloud activity analysis
- Identity analysis
- Process analysis
- Command-line analysis
- File analysis
- Network analysis
- Evidence correlation
- Timeline reconstruction
- Risk assessment
- Technical documentation

## Incident Response

- Incident triage
- Scope determination
- Process-chain reconstruction
- File investigation
- Network investigation
- Persistence checks
- Credential-theft assessment
- Lateral-movement assessment
- C2 assessment
- Exfiltration assessment
- Containment evaluation
- Endpoint isolation assessment
- Evidence-based disposition

## Security Operations

- Alert triage
- Incident investigation
- Entity investigation
- Device investigation
- User investigation
- Timeline analysis
- Evidence correlation
- Incident classification
- Incident resolution

## Threat Hunting

- Advanced Hunting
- Behavior-based hunting
- PowerShell hunting
- Encoded command hunting
- Process-chain hunting
- File telemetry hunting
- Network telemetry hunting
- Identity hunting
- Cross-table correlation

## Detection Engineering

- KQL development
- Detection thresholds
- Time-window analysis
- Detection validation
- False-negative troubleshooting
- Entity mapping
- Alert enrichment
- MITRE ATT&CK mapping
- Detection tuning

## Endpoint Security

- Defender for Endpoint onboarding
- Defender service validation
- EDR telemetry
- Process lineage
- Parent-child process analysis
- Process ID correlation
- Command-line analysis
- PowerShell investigation
- File telemetry
- Network telemetry
- Registry analysis
- Device Timeline
- Endpoint response capabilities

## Identity Security

- Microsoft Entra authentication
- `SigninLogs`
- Failed login investigation
- Source IP analysis
- Authentication timelines
- Success/failure correlation
- Failure reason analysis
- Identity entity mapping

## SIEM Engineering

- Microsoft Sentinel
- Log Analytics
- Data connectors
- Diagnostic settings
- Security telemetry ingestion
- Table validation
- Analytics rules
- Incident generation
- KQL

## Security Engineering

- Security environment deployment
- Endpoint onboarding
- Security service validation
- Telemetry-pipeline troubleshooting
- Detection testing
- Detection troubleshooting
- Security control validation
- Response capability assessment

---

# MITRE ATT&CK Exposure

| Technique | Context |
| --- | --- |
| **T1110 — Brute Force** | Repeated Microsoft Entra failed sign-in detection |
| **T1496 — Resource Hijacking** | Sentinel suspicious resource deployment analytics-rule review |

MITRE ATT&CK was treated as a standardized behavioral framework rather than proof that an attacker was responsible.

---

# Security Engineering Lessons

## Telemetry Comes Before Detection

```text
No Data
   ↓
No Query
   ↓
No Detection
   ↓
No Alert
   ↓
No Investigation
```

---

## Telemetry Can Be Useful Without an Alert

Lab 05 demonstrated:

```text
Endpoint Activity
        ↓
Telemetry
        ↓
Advanced Hunting
        ↓
Investigation
        ↓
Conclusion
```

Threat hunting does not always begin with an alert.

---

## Detection Comes Before Automation

```text
Understand Behavior
        ↓
Build Detection
        ↓
Validate Detection
        ↓
Tune Detection
        ↓
Then Consider Automation
```

---

## Investigation Comes Before Response

```text
Suspicious Behavior
        ↓
Investigation
        ↓
Evidence
        ↓
Risk Assessment
        ↓
Response Decision
```

---

## Troubleshooting Must Be Layered

```text
SOURCE
   ↓
SENSOR / CONNECTOR
   ↓
WORKSPACE
   ↓
TABLE
   ↓
QUERY
   ↓
RULE
   ↓
ALERT
   ↓
INCIDENT
   ↓
RESPONSE
```

---

# What I Would Do in a Real Suspicious PowerShell Incident

If the activity were unknown rather than controlled, I would continue investigating:

- Which user executed PowerShell
- Whether the account was privileged
- Whether PowerShell usage was normal for that user
- What launched PowerShell
- Full command line
- Encoded command contents
- Execution-policy bypasses
- Hidden execution
- Download activity
- Downloaded file hashes
- Threat-intelligence matches
- Domains contacted
- Remote IP reputation
- Beaconing
- Persistence
- Scheduled tasks
- Services
- Registry Run keys
- Credential access
- LSASS access
- Lateral movement
- Remote execution
- Other affected endpoints
- Data staging
- Data exfiltration
- Additional Defender alerts
- Related suspicious authentication

Potential response actions could include:

- Isolate endpoint
- Collect investigation package
- Start Live Response
- Run antivirus scan
- Block malicious hashes
- Block malicious infrastructure
- Disable compromised account
- Revoke sessions
- Preserve forensic evidence
- Escalate to Incident Response
- Hunt across other endpoints

---

# Hiring Manager / Recruiter Quick View

## What I Built

I configured and operated a working Microsoft cybersecurity lab containing:

- Microsoft Azure
- Microsoft Entra ID
- Azure Log Analytics
- Microsoft Sentinel
- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Windows 11 Enterprise
- Endpoint telemetry
- Cloud administrative telemetry
- Authentication telemetry
- KQL hunting
- Advanced Hunting
- Custom detection logic
- Alert generation
- Incident generation
- Incident investigation
- Response assessment

---

## What I Investigated

### Endpoint

- Suspicious PowerShell
- Encoded PowerShell
- Process execution
- Process ancestry
- Process IDs
- CMD execution
- WHOAMI execution
- File creation
- Network connections
- Domains
- IP addresses
- Ports
- Registry activity
- Device Timeline
- Defender incidents

### Identity

- Failed Microsoft Entra authentication
- Repeated failures
- Source IPs
- Authentication timing
- Failure reasons
- Success/failure sequences
- Targeted applications

### Cloud

- Azure administrative operations
- Azure Activity telemetry
- Resource activity

### Detection Engineering

- Detection query development
- Thresholds
- Time windows
- False-negative troubleshooting
- Entity mapping
- MITRE ATT&CK
- Alert generation
- Incident generation

### Incident Response

- Initial triage
- Evidence validation
- Timeline reconstruction
- Process correlation
- File correlation
- Network correlation
- Persistence assessment
- Containment assessment
- Final disposition

---

# What I Troubleshot

- Defender endpoint onboarding
- Defender service dependencies
- Windows endpoint capabilities
- Missing Entra `SigninLogs`
- Diagnostic settings
- Log Analytics ingestion
- KQL results
- Sentinel entity mappings
- Scheduled analytics timing
- Detection logic across fixed time buckets
- PowerShell Advanced Hunting queries
- Network-event field handling
- Timestamp differences
- Unrelated existing incidents

---

# Interview-Ready Project Explanation

> I built and operated a Microsoft cybersecurity lab using Microsoft Defender XDR, Defender for Endpoint, Microsoft Sentinel, Microsoft Entra ID, Azure Log Analytics, and KQL.
>
> I began by configuring the cloud and endpoint environment and onboarding a Windows 11 Enterprise endpoint into Defender for Endpoint. I encountered an onboarding problem involving the Defender endpoint service, so I validated the Windows edition, endpoint capabilities, and service dependencies before completing the onboarding.
>
> I generated controlled endpoint activity, investigated Defender detections, reviewed process lineage, and validated the underlying telemetry through Advanced Hunting.
>
> I then worked with Microsoft Defender XDR to understand the relationship between alerts, incidents, devices, users, evidence, timelines, and endpoint response actions.
>
> I expanded the environment into Microsoft Sentinel and Log Analytics, connected Azure Activity telemetry, and used KQL to filter, project, aggregate, and analyze cloud administrative activity. I also studied how KQL becomes scheduled detection logic and how detections map to MITRE ATT&CK.
>
> For the identity-security investigation, I discovered that Microsoft Entra sign-in telemetry was initially missing from Log Analytics. I configured diagnostic settings, validated `SigninLogs` ingestion, generated controlled failed authentication attempts, and investigated the account, source IP, timestamps, applications, and failure reasons.
>
> I converted the manually validated hunting logic into a scheduled Sentinel analytics rule for repeated failed sign-ins. The first version did not trigger even though the source events existed. I traced the problem to fixed five-minute time buckets that split related authentication attempts across different groups. I corrected the KQL, validated the logic, generated new activity, and successfully produced a Sentinel alert and Microsoft Defender incident.
>
> I investigated that incident and determined that the detection was valid but the activity was authorized testing. I documented the evidence and resolved the incident without unnecessary containment.
>
> In Lab 05, I performed a deeper suspicious PowerShell endpoint investigation. I generated controlled encoded PowerShell, child-process, file, and network activity and reconstructed the behavior using Advanced Hunting.
>
> `DeviceProcessEvents` showed the encoded PowerShell execution and a `powershell.exe → cmd.exe → whoami.exe` process chain. `DeviceFileEvents` showed file creation. `DeviceNetworkEvents` proved that PowerShell successfully connected to `example.com` over TCP port 443.
>
> I also reviewed Device Timeline and existing Defender incidents. Two incidents looked relevant based on their names, but timestamp validation showed that they occurred weeks before my Lab 05 activity, so I excluded them instead of incorrectly forcing them into the investigation.
>
> Finally, I evaluated endpoint response capabilities including isolation, Live Response, automated investigation, and investigation-package collection. Because I found no malicious payload, persistence, credential theft, lateral movement, command-and-control, exfiltration, or continuing unauthorized access, I determined that endpoint isolation was not justified.
>
> The main lesson from the entire project was that cybersecurity investigation is not about trusting an alert title or reacting to a suspicious process name. It requires validating telemetry, reconstructing behavior, correlating evidence, testing detection logic, understanding context, assessing risk, and making a defensible response decision.

---

# Interview Question — How Would You Investigate Suspicious PowerShell?

```text
1. Identify affected device
        ↓
2. Identify user
        ↓
3. Locate PowerShell process
        ↓
4. Review command line
        ↓
5. Check encoded arguments
        ↓
6. Understand / decode command
        ↓
7. Identify parent process
        ↓
8. Identify child processes
        ↓
9. Correlate process IDs
        ↓
10. Investigate file activity
        ↓
11. Investigate network activity
        ↓
12. Identify domains / IPs / ports
        ↓
13. Investigate Registry activity
        ↓
14. Review Device Timeline
        ↓
15. Review related alerts
        ↓
16. Review related incidents
        ↓
17. Validate timestamps
        ↓
18. Look for persistence / credential theft / lateral movement / C2 / exfiltration
        ↓
19. Determine benign vs suspicious vs malicious
        ↓
20. Contain if justified
        ↓
21. Document evidence
```

---

# Interview Question — How Do You Investigate Failed Logins?

```text
1. Confirm the authentication activity
        ↓
2. Identify affected account
        ↓
3. Identify source IP
        ↓
4. Measure failure volume
        ↓
5. Analyze timing
        ↓
6. Review failure reasons
        ↓
7. Identify applications
        ↓
8. Determine whether other users were targeted
        ↓
9. Check for successful authentication afterward
        ↓
10. Compare against expected behavior
        ↓
11. Correlate cloud / endpoint evidence
        ↓
12. Assess risk
        ↓
13. Contain if justified
        ↓
14. Document conclusion
```

---

# Interview Question — What Did You Learn About Detection Engineering?

> A detection rule is not correct simply because the KQL executes without errors.
>
> During my failed-login investigation, I initially grouped authentication events using fixed five-minute time buckets. Three failures occurred within only a few minutes, but one landed on one side of the fixed boundary and two landed on the other side.
>
> The threshold therefore never reached three.
>
> I verified the source telemetry, isolated the failure to the query logic, corrected the detection to evaluate the events across the rule's lookback window, validated it again, and successfully generated the expected alert and incident.
>
> That demonstrated that detection engineering requires behavioral testing, not only syntax validation.

---

# Interview Question — Why Is Encoded PowerShell Suspicious?

> `-EncodedCommand` deserves investigation because attackers can use encoded PowerShell to make commands harder to quickly interpret.
>
> However, encoding does not automatically prove malicious activity.
>
> Administrators, automation systems, software, scripts, and security tools may also use encoded PowerShell.
>
> I therefore treat encoding as an investigation signal and determine what the command actually did by correlating process, file, network, user, and timeline telemetry.

---

# Interview Question — Why Did You Not Isolate the Endpoint?

> I considered isolation because the activity involved encoded PowerShell, CMD, WHOAMI, file creation, and outbound network communication.
>
> However, the investigation established that the behavior was authorized testing and I found no evidence of malware, persistence, credential theft, lateral movement, command-and-control, exfiltration, or continued unauthorized access.
>
> Isolation would therefore not have been supported by the evidence.
>
> The lab reinforced that containment decisions should be based on risk and evidence rather than automatically triggered by suspicious-looking behavior.

---

# Interview Question — How Do You Avoid Overreacting to Alerts?

I separate:

```text
OBSERVATION
What happened?
```

from:

```text
INTERPRETATION
What might it mean?
```

from:

```text
DECISION
What should I do?
```

Example:

```text
Observation:
Encoded PowerShell executed.

Interpretation:
Could represent suspicious or malicious execution.

Evidence:
Known controlled test, known endpoint, known user, harmless artifacts,
known destination, no persistence, no credential theft, no C2.

Decision:
No isolation required.
```

---

# Week 17 Final Findings

The five labs established that:

- A Microsoft security environment can be built from the ground up.
- Defender for Endpoint depends on working OS services and capabilities.
- Endpoint telemetry can be independently validated.
- Alerts and incidents are not the same thing.
- Alerts are investigation starting points.
- XDR incidents provide broader context than individual detections.
- Device and user entities provide useful pivots.
- Process lineage provides more context than process names alone.
- Process IDs can support process ancestry.
- Full command lines are critical investigation evidence.
- PowerShell is not automatically malicious.
- Encoded PowerShell is a signal, not a verdict.
- `DeviceProcessEvents` can reconstruct process execution.
- `DeviceFileEvents` can identify filesystem effects.
- `DeviceNetworkEvents` can prove network communication.
- Command intent and observed behavior are different.
- Registry telemetry can help investigate persistence.
- Negative evidence can affect risk assessment.
- Device Timeline provides chronological endpoint context.
- Similar-looking events should not automatically be attached to an investigation.
- Incident timestamps must be validated.
- Threat hunting can succeed without a new alert.
- Response actions should match evidence.
- Endpoint isolation should be evidence-based.
- Log Analytics provides the telemetry foundation for Sentinel.
- Sentinel depends on correctly configured data sources.
- Azure Activity can be investigated through `AzureActivity`.
- Entra authentication can be investigated through `SigninLogs`.
- `where` isolates relevant evidence.
- `project` reduces unnecessary fields.
- `summarize` converts events into behavioral patterns.
- `count()` measures event volume.
- `dcount()` measures distinct entities.
- Timeline ordering reconstructs activity.
- Authentication failure codes provide useful context.
- Success after repeated failure may increase risk.
- Scheduled analytics rules automate hunting logic.
- Entity mapping improves investigation context.
- MITRE ATT&CK standardizes behavior descriptions.
- KQL can be syntactically correct while detection logic remains wrong.
- Raw telemetry is essential for troubleshooting detections.
- A valid detection can still represent authorized behavior.
- Incident disposition should be supported by evidence.
- Containment should be supported by evidence.

---

# Final Technical Outcome

Week 17 provided practical exposure to:

> **Telemetry Generation → Collection → Ingestion → Storage → Querying → Hunting → Detection → Alerting → Correlation → Investigation → Timeline Reconstruction → Process Analysis → File Analysis → Network Analysis → Validation → Classification → Risk Assessment → Response → Documentation**

The five-lab progression was:

```text
BUILD
    ↓
MONITOR
    ↓
QUERY
    ↓
HUNT
    ↓
DETECT
    ↓
INVESTIGATE
    ↓
CORRELATE
    ↓
VALIDATE
    ↓
ASSESS
    ↓
DECIDE
    ↓
RESPOND
```

---

# Final Project Status

```text
Microsoft Security Environment:
COMPLETED

Microsoft Defender for Endpoint:
CONFIGURED & VALIDATED

Microsoft Defender XDR:
INVESTIGATED

Microsoft Sentinel:
CONFIGURED & VALIDATED

Azure Activity Ingestion:
VALIDATED

Microsoft Entra Sign-In Ingestion:
VALIDATED

KQL:
PRACTICED & APPLIED

Endpoint Hunting:
COMPLETED

Cloud SIEM Analysis:
COMPLETED

Identity Investigation:
COMPLETED

Custom Analytics Rule:
CREATED & ENABLED

Detection Troubleshooting:
COMPLETED

Alert Generation:
VALIDATED

Incident Generation:
VALIDATED

Failed Login Investigation:
COMPLETED

Failed Login Disposition:
RESOLVED — EXPECTED ACTIVITY

Suspicious PowerShell Investigation:
COMPLETED

Encoded PowerShell Analysis:
COMPLETED

Process Ancestry Analysis:
COMPLETED

File Telemetry Investigation:
COMPLETED

Network Telemetry Investigation:
COMPLETED

Registry Review:
COMPLETED

Device Timeline Investigation:
COMPLETED

Existing Incident Validation:
COMPLETED

Endpoint Response Assessment:
COMPLETED

Lab 05 Classification:
BENIGN / AUTHORIZED SECURITY TESTING

Device Isolation:
NOT REQUIRED

Confirmed Lab 05 Compromise:
NO
```

---

# Final Security Mindset

The strongest lesson from Week 17 can be summarized as:

```text
ALERT
  ≠
CONCLUSION
```

Lab 05 expanded that principle:

```text
POWERSHELL
  ≠
MALWARE
```

and:

```text
ENCODED COMMAND
  ≠
COMPROMISE
```

The correct workflow is:

```text
OBSERVATION
      ↓
EVIDENCE
      ↓
CONTEXT
      ↓
CORRELATION
      ↓
TIMELINE
      ↓
BEHAVIOR
      ↓
VALIDATION
      ↓
RISK ASSESSMENT
      ↓
CONCLUSION
      ↓
RESPONSE
```

---

# Key Takeaway

> **Security tools generate information. Security professionals turn that information into defensible decisions.**

Week 17 strengthened my ability to move from raw endpoint, cloud, identity, process, file, and network telemetry to a final security conclusion using Microsoft Defender XDR, Microsoft Defender for Endpoint, Microsoft Sentinel, Microsoft Entra ID, Azure Log Analytics, Advanced Hunting, and KQL.

The most important outcome was not learning where buttons are located.

It was learning how to follow the evidence:

```text
What happened?
      ↓
Who or what was involved?
      ↓
Which process executed?
      ↓
What command did it run?
      ↓
What launched it?
      ↓
What did it launch?
      ↓
What files changed?
      ↓
Where did it connect?
      ↓
Did the connection succeed?
      ↓
Which identity was involved?
      ↓
What happened before and after?
      ↓
What does the raw telemetry prove?
      ↓
Why did the detection trigger?
      ↓
Does the alert actually belong to this activity?
      ↓
Is there evidence of continued compromise?
      ↓
What explanation best fits all of the evidence?
      ↓
What response is actually justified?
```

That is the investigation and security engineering mindset I am continuing to develop across cybersecurity analysis, security operations, incident response, endpoint security, threat hunting, detection engineering, SIEM engineering, identity security, cloud security, and security engineering.
