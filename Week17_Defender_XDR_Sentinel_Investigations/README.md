# Week 17 — Microsoft Defender XDR, Microsoft Sentinel & Identity Security Operations

> **Focus:** Microsoft Defender XDR • Microsoft Defender for Endpoint • Microsoft Sentinel • Microsoft Entra ID • Azure Log Analytics • Kusto Query Language (KQL) • Endpoint Detection and Response (EDR) • Security Information and Event Management (SIEM) • Extended Detection and Response (XDR) • Identity Security • Authentication Investigation • Threat Hunting • Detection Engineering • MITRE ATT&CK • Incident Investigation • Incident Response

---

# Executive Summary

Week 17 focused on building and operating a Microsoft-based Security Operations environment from the ground up.

Instead of beginning with an already configured SIEM or EDR platform, I worked through the complete security-monitoring process:

```text
BUILD THE ENVIRONMENT
        ↓
CONNECT SECURITY DATA
        ↓
GENERATE CONTROLLED ACTIVITY
        ↓
VALIDATE TELEMETRY
        ↓
QUERY RAW SECURITY EVENTS
        ↓
INVESTIGATE ALERTS
        ↓
CORRELATE USERS, DEVICES, IPs & EVENTS
        ↓
BUILD DETECTION LOGIC
        ↓
GENERATE ALERTS
        ↓
CREATE INCIDENTS
        ↓
INVESTIGATE THE EVIDENCE
        ↓
MAKE A DEFENSIBLE DECISION
        ↓
DOCUMENT & RESOLVE
```

The week combined four connected labs:

| Lab | Main Focus | Major Skills Demonstrated |
| --- | --- | --- |
| **Lab 01** | Microsoft Security Environment Setup & EDR Investigation | Azure, Defender for Endpoint, endpoint onboarding, EDR telemetry, process investigation |
| **Lab 02** | Microsoft Defender XDR Fundamentals | Incident triage, alerts, entities, device investigation, timelines, process lineage |
| **Lab 03** | Microsoft Sentinel & KQL Fundamentals | SIEM telemetry, Azure Activity, Log Analytics, KQL, analytics rules, MITRE ATT&CK |
| **Lab 04** | Failed Login Investigation | Microsoft Entra ID, `SigninLogs`, authentication analysis, detection engineering, incident investigation |

Lab 04 brought the previous work together into a complete identity-security investigation:

```text
Microsoft Entra Sign-in
        ↓
Diagnostic Settings
        ↓
Log Analytics
        ↓
SigninLogs
        ↓
KQL Investigation
        ↓
Custom Sentinel Analytics Rule
        ↓
Security Alert
        ↓
Microsoft Defender Incident
        ↓
Analyst Investigation
        ↓
Final Classification
        ↓
Resolved
```

---

# Why This Project Matters

This project was designed to develop skills that are useful for roles such as:

- SOC Analyst
- Security Operations Analyst
- Incident Response Analyst
- Cybersecurity Analyst
- Detection Analyst
- Detection Engineer
- SIEM Engineer
- Security Engineer
- Cloud Security Analyst
- Microsoft Security Analyst

The main lesson from Week 17 was that security operations is not mainly about knowing where to click inside a product.

A strong investigation requires understanding:

- Where security data comes from
- How telemetry reaches the SIEM
- Which table contains the evidence
- Which fields answer the investigation questions
- Why a detection rule fired
- Whether the detection logic actually matches the intended behavior
- Which users, devices, processes, IP addresses, and resources are involved
- What happened before and after an alert
- Whether the evidence supports malicious activity
- Whether containment or escalation is justified
- How to clearly document the final decision

---

# Week 17 Security Operations Progression

The four labs were intentionally connected.

## Lab 01

> **Can I build and validate the Microsoft security environment?**

## Lab 02

> **Can I investigate security activity using Microsoft Defender XDR?**

## Lab 03

> **Can I understand where SIEM telemetry comes from and analyze it using KQL?**

## Lab 04

> **Can I take raw identity telemetry, investigate suspicious authentication, create a detection, generate an incident, and make a final analyst decision?**

Together, the labs created the following progression:

```text
SECURITY INFRASTRUCTURE
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
CUSTOM DETECTION ENGINEERING
        ↓
ALERT GENERATION
        ↓
INCIDENT INVESTIGATION
        ↓
EVIDENCE-BASED DISPOSITION
```

---

# Week 17 Objectives

The main objectives of Week 17 were to:

- Build a functional Microsoft cloud security lab
- Configure Microsoft Sentinel
- Configure Azure Log Analytics
- Configure Microsoft Defender XDR
- Configure Microsoft Defender for Endpoint
- Establish a Microsoft Entra ID identity environment
- Onboard a Windows 11 Enterprise endpoint
- Validate Defender for Endpoint services
- Validate endpoint telemetry
- Troubleshoot endpoint onboarding
- Generate controlled endpoint security activity
- Investigate Defender alerts
- Investigate Defender incidents
- Understand the difference between alerts and incidents
- Review device inventory
- Investigate device entity pages
- Review device timelines
- Review user entity pages
- Analyze process execution
- Analyze parent-child process relationships
- Use Microsoft Defender Advanced Hunting
- Query endpoint security telemetry using KQL
- Configure Microsoft Sentinel data ingestion
- Understand data connectors
- Understand diagnostic settings
- Understand Log Analytics tables
- Investigate the `AzureActivity` table
- Investigate the `SigninLogs` table
- Filter logs using `where`
- Select useful investigation fields using `project`
- Aggregate data using `summarize`
- Count events using `count()`
- Identify distinct entities using `dcount()`
- Sort timelines using `order by`
- Analyze time using `TimeGenerated`
- Understand `bin()` and time grouping
- Identify common authentication failure reasons
- Compare successful and failed authentication
- Analyze failed sign-ins by account
- Analyze failed sign-ins by source IP
- Build authentication timelines
- Create custom Microsoft Sentinel scheduled analytics rules
- Configure detection thresholds
- Configure rule scheduling
- Configure entity mappings
- Configure custom alert details
- Map detections to MITRE ATT&CK
- Validate detection logic using real telemetry
- Troubleshoot a detection that did not initially fire
- Generate a Microsoft Sentinel alert
- Generate a Microsoft Defender incident
- Investigate incident evidence
- Classify expected versus malicious activity
- Document a professional investigation
- Resolve an incident

---

# Security Operations Architecture

## Cloud Security Layer

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
Resource Group
RG-Microsoft-Security-Lab
        │
        ▼
Log Analytics Workspace
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

---

## Endpoint Security Layer

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
        ├── Process Evidence
        ├── Advanced Hunting
        └── Response Actions
```

---

## SIEM Layer

```text
Security-Relevant Activity
        │
        ▼
Data Source
        │
        ▼
Connector / Diagnostic Setting
        │
        ▼
Log Analytics Workspace
        │
        ▼
Structured Security Table
        │
        ▼
KQL
        │
        ▼
Detection Logic
        │
        ▼
Analytics Rule
        │
        ▼
Alert
        │
        ▼
Incident
        │
        ▼
Analyst Investigation
```

---

# Technologies & Platforms

| Technology | Purpose |
| --- | --- |
| **Microsoft Defender XDR** | Unified security detection, investigation, evidence correlation, and incident response |
| **Microsoft Defender for Endpoint** | Endpoint telemetry, Endpoint Detection and Response, device investigation, and response |
| **Microsoft Sentinel** | Cloud-native SIEM, analytics, hunting, detection, and incident investigation |
| **Microsoft Entra ID** | Identity, user authentication, tenant, and access management |
| **Azure Log Analytics** | Central location for storing and querying security telemetry |
| **Azure Activity Log** | Azure control-plane administrative activity |
| **Microsoft Entra Sign-in Logs** | Authentication telemetry used for identity investigations |
| **Kusto Query Language (KQL)** | Security querying, filtering, aggregation, hunting, and detection logic |
| **Microsoft Defender Advanced Hunting** | Security telemetry investigation using KQL |
| **Sentinel Analytics Rules** | Automated detection logic |
| **MITRE ATT&CK** | Standard framework for mapping behavior to adversary tactics and techniques |
| **Windows 11 Enterprise** | Enterprise endpoint monitored through Defender for Endpoint |
| **PowerShell** | Endpoint testing, validation, and administration |
| **Oracle VirtualBox** | Virtualization environment supporting endpoint labs |

---

# Core Security Tables Used

| Table | Security Purpose |
| --- | --- |
| `DeviceProcessEvents` | Endpoint process execution telemetry |
| `AzureActivity` | Azure administrative and control-plane events |
| `SigninLogs` | Interactive Microsoft Entra authentication activity |
| `SecurityAlert` | Security alerts available through Log Analytics |
| `SecurityIncident` | Microsoft Sentinel incident information |

---

# Lab 01 — Microsoft Security Environment Setup & EDR Investigation

## Objective

Build the Microsoft security infrastructure required for later investigations and validate the environment through a controlled endpoint-security exercise.

The goal was not simply to turn on Microsoft Defender.

The goal was to understand the complete endpoint security pipeline:

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
2. Establishing the Microsoft Entra tenant
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
13. Investigating the resulting detection
14. Reviewing process execution
15. Reviewing process lineage
16. Reviewing the Device Timeline
17. Hunting telemetry using KQL
18. Correlating alert and raw telemetry
19. Classifying the activity
20. Cleaning up temporary infrastructure

---

## Endpoint Onboarding Troubleshooting

One of the most useful parts of the lab was troubleshooting an onboarding problem.

During the initial attempt, the Microsoft Defender for Endpoint `SENSE` service was unavailable.

I checked the service using:

```powershell
sc query sense
```

I validated the Windows edition using:

```powershell
DISM /Online /Get-CurrentEdition
```

I also inspected Defender for Endpoint capability information:

```powershell
DISM.EXE /Online /Get-CapabilityInfo /CapabilityName:Microsoft.Windows.Sense.Client~~~~
```

This reinforced an important security-engineering lesson:

> A security product depends on operating-system capabilities, services, licensing, connectivity, permissions, configuration, and telemetry. Troubleshooting requires validating those dependencies instead of repeatedly running the same failed command.

---

## Controlled Detection Test

After the endpoint was onboarded successfully, I generated controlled Defender security test activity.

The workflow was:

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

The resulting investigation included a suspicious PowerShell command-line alert.

---

## Process Tree Analysis

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

Instead of analyzing `powershell.exe` by itself, I reviewed:

- Parent process
- Child process
- User context
- Command line
- Process order
- Related activity

This helped me understand how analysts use process lineage to determine whether execution matches expected user activity or something more suspicious.

---

## Advanced Hunting

Microsoft Defender Advanced Hunting was used to independently validate the endpoint telemetry.

Example query:

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

This demonstrated that an alert could be validated using underlying telemetry instead of being accepted only because the security product generated it.

---

## Lab 01 Final Classification

```text
Detection Result: Valid Detection
Activity Type: Authorized Security Testing
Compromise: No
Analyst Disposition: Expected / Security Testing
```

### Main Lesson

> An alert can correctly detect security-relevant behavior even when the behavior was intentionally generated.

The analyst still has to determine context and intent.

---

# Lab 02 — Microsoft Defender XDR Fundamentals

## Objective

Develop practical familiarity with the Microsoft Defender XDR investigation environment and understand how a SOC analyst moves from high-level security information into incidents, alerts, devices, users, evidence, timelines, and response actions.

---

## SOC Investigation Workflow

```text
Defender XDR Dashboard
        ↓
Incident Queue
        ↓
Incident
        ↓
Alert
        ↓
Evidence & Entities
       ↙       ↘
   Device      User
      │          │
      ▼          ▼
Device       User
Timeline     Timeline
      │
      ▼
Process Lineage
      │
      ▼
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
- Affected user
- Affected device
- Process
- File
- IP address
- Timestamp
- Supporting evidence
- MITRE ATT&CK mapping

> **An alert is a reason to investigate. It is not automatically proof of compromise.**

---

## Incident

An incident represents the larger security case.

```text
Alert 1 ─────┐
             │
Alert 2 ─────┼────► Incident
             │
Alert 3 ─────┘
```

An incident can connect:

- Multiple alerts
- Users
- Devices
- Processes
- Files
- IP addresses
- Evidence
- Investigation information
- Response actions

The analyst therefore moves from:

> **What does this individual alert say?**

to:

> **What happened across the environment, and how are these signals related?**

---

# Device Investigation

Device entity pages were reviewed to understand:

- Device identity
- Operating system
- Risk level
- Exposure level
- Logged-on users
- Alerts
- Incidents
- Software
- Security recommendations
- Timeline
- Response options

---

# Device Timeline

The Device Timeline allows activity to be reconstructed in chronological order.

```text
Before Detection
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
- What happened immediately before it?
- What happened afterward?
- Did the activity continue?
- Did other processes become involved?

---

# Process Lineage

A suspicious process should not automatically be investigated in isolation.

Instead of:

```text
powershell.exe
```

the analyst should think:

```text
Parent Process
      ↓
Process
      ↓
Command Line
      ↓
Child Processes
      ↓
Files / Network / Registry Activity
```

This gives much more useful context.

---

# User Investigation

User entity information was reviewed to understand:

- User identity
- Alerts involving the account
- Devices associated with the account
- Authentication or activity timeline
- Other security evidence
- Whether behavior matched normal activity

Identity context becomes especially important in Lab 04.

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

These were evaluated but not automatically executed.

> **The existence of a response button does not mean an analyst should immediately use it. Response must match the evidence, scope, severity, confidence, and possible business impact.**

---

# Lab 03 — Microsoft Sentinel & KQL Fundamentals

## Objective

Develop practical experience with the Microsoft Sentinel SIEM data pipeline and understand how cloud activity becomes searchable telemetry and eventually automated detection logic.

The core question was:

> **Can I understand where SIEM data comes from, validate that it exists, query it, identify patterns, and understand how those queries can become detections?**

---

# Lab 03 Data Pipeline

```text
Generate Azure Activity
        ↓
Verify Source Event
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

# Lab 03 Environment

```text
Microsoft Azure Subscription
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

A temporary resource group was used to safely create observable administrative telemetry:

```text
rg-sentinel-kql-lab3-test
```

---

# Understanding the SIEM Data Pipeline

One of the biggest lessons from Lab 03 was that enabling Sentinel does not automatically mean every security event is available.

The full telemetry path must work.

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

If any part of this chain fails, visibility is reduced.

For example:

```text
No telemetry
    ↓
Nothing to ingest

Connector not configured
    ↓
Telemetry does not reach workspace

Wrong table
    ↓
Query returns nothing useful

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

Microsoft Sentinel adds security operations capabilities.

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

The relationship is:

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

# Azure Activity Table

Important fields investigated included:

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

# KQL Fundamentals Practiced

## Basic Table Query

```kusto
AzureActivity
| take 10
```

---

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

---

## Time Filtering

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
```

---

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

---

## Counting Events

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
| count
```

---

## Behavioral Aggregation

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
| summarize EventCount = count() by ActivityStatusValue
| order by EventCount desc
```

---

## Top Operations

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
| summarize EventCount = count() by OperationNameValue
| top 10 by EventCount desc
```

These queries helped move the investigation from:

> **What does this one event say?**

to:

> **What pattern does this group of events show?**

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
      ├── Condition Not Met → Continue Monitoring
      │
      └── Condition Met
                ↓
              Alert
                ↓
             Incident
```

A Sentinel analytics rule may include:

- Rule name
- Description
- Severity
- Required data sources
- KQL query
- Query frequency
- Lookback period
- Alert threshold
- Entity mappings
- Incident settings
- MITRE ATT&CK mapping
- Automated response

---

# MITRE ATT&CK in Lab 03

A Sentinel rule for suspicious resource deployment was reviewed.

The rule mapped to:

```text
Tactic:
Impact

Technique:
T1496 — Resource Hijacking
```

The important lesson was:

> **MITRE ATT&CK mapping gives behavior a standard name. It does not prove that an attacker performed that behavior.**

The analyst still needs evidence and context.

---

# Lab 04 — Failed Login Investigation

## Objective

Investigate repeated Microsoft Entra failed sign-ins from beginning to end and determine whether the authentication activity represented normal behavior, expected testing, or a possible identity attack.

This lab brought together the main skills developed throughout Week 17:

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

> **Can I investigate failed authentication using raw evidence, build detection logic around the behavior, and make a final decision that is supported by the evidence?**

---

# Lab 04 Security Scenario

Repeated failed authentication attempts can happen for many reasons.

Possible explanations include:

- User password mistakes
- Forgotten passwords
- Expired passwords
- Applications using old credentials
- Automated authentication problems
- Password guessing
- Brute-force attacks
- Password spraying
- Credential stuffing
- Authorized security testing

Because of this, seeing several failed logins is not enough to say:

> **This account is being attacked.**

A stronger analyst asks:

```text
WHO?
Which account?

FROM WHERE?
Which IP address?

WHEN?
What timestamps?

HOW OFTEN?
How many attempts?

HOW QUICKLY?
Seconds? Minutes? Hours?

WHAT APPLICATION?
What service was being accessed?

WHY?
What was the failure reason?

WHAT HAPPENED NEXT?
More failures?
A successful login?

WHAT IS THE CONTEXT?
Known user?
Known IP?
Controlled testing?

WHAT SHOULD I DO?
Close?
Monitor?
Escalate?
Contain?
```

---

# Lab 04 Architecture

The identity telemetry path used in this lab was:

```text
Microsoft Entra ID
        ↓
Interactive Sign-in Activity
        ↓
SignInLogs Diagnostic Category
        ↓
Microsoft Entra Diagnostic Setting
        ↓
LAW-Microsoft-Security-Lab
        ↓
SigninLogs
        ↓
Microsoft Sentinel
        ↓
KQL Investigation
        ↓
Scheduled Analytics Rule
        ↓
Security Alert
        ↓
Microsoft Defender Incident
        ↓
Analyst Investigation
        ↓
Final Disposition
```

---

# Phase 1 — Testing for Sign-in Telemetry

The investigation began by checking whether the `SigninLogs` table contained authentication data.

```kusto
SigninLogs
| take 10
```

Initially, no results were returned.

Instead of assuming there simply were no authentication events, I checked which tables contained data.

```kusto
search *
| summarize Events=count() by $table
| sort by Events desc
```

The workspace contained telemetry such as:

- `AzureActivity`
- `Usage`
- `SecurityAlert`
- `SecurityIncident`

but `SigninLogs` was not being populated.

This established that:

```text
Log Analytics Workspace
        ↓
WORKING

Microsoft Sentinel
        ↓
WORKING

Other Security Telemetry
        ↓
WORKING

Entra Sign-in Telemetry
        ↓
MISSING
```

This was an important troubleshooting step.

> **A query cannot investigate data that was never collected.**

---

# Phase 2 — Configuring Microsoft Entra Diagnostic Settings

Microsoft Entra diagnostic settings were configured to send authentication telemetry into the existing Log Analytics workspace.

The diagnostic setting included:

```text
AuditLogs
SignInLogs
```

The destination was:

```text
LAW-Microsoft-Security-Lab
```

This completed the identity telemetry path:

```text
Microsoft Entra
      ↓
SignInLogs
      ↓
Diagnostic Setting
      ↓
Log Analytics
      ↓
SigninLogs
      ↓
Microsoft Sentinel
```

This step reinforced an important SIEM lesson:

> **Configuration is not the same as validation.**

After configuring the source, I still needed to prove that the data arrived.

---

# Phase 3 — Generating Controlled Authentication Activity

A dedicated lab account was used to safely create known authentication events.

Controlled failed login attempts were generated instead of testing against an important administrator account.

This provided known activity that could later be traced through the environment.

```text
Controlled Failed Login
        ↓
Microsoft Entra Records Event
        ↓
Diagnostic Setting Sends Event
        ↓
Log Analytics Stores Event
        ↓
Analyst Queries Event
```

Using known activity helped separate:

> **Did the authentication happen?**

from:

> **Did the detection identify it?**

---

# Phase 4 — Initial Failed Sign-in Investigation

Once `SigninLogs` began receiving data, I used:

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

This created a focused authentication-investigation view.

---

# Understanding the Main Authentication Fields

## `TimeGenerated`

Answers:

> **When did the authentication attempt happen?**

---

## `UserPrincipalName`

Answers:

> **Which identity was involved?**

---

## `IPAddress`

Answers:

> **Which network source generated the request?**

An IP address provides useful network context but does not automatically identify a person.

---

## `Location`

Provides geographic context associated with the source.

Location alone is not proof of malicious behavior because VPNs, proxies, mobile carriers, cloud infrastructure, and ISP routing can affect geolocation.

---

## `AppDisplayName`

Answers:

> **What application was involved?**

The authentication activity in this lab included the Azure Portal.

---

## `ResultType`

Provides the authentication result code.

Successful Microsoft Entra authentication normally uses:

```text
ResultType = 0
```

Failed authentication can therefore be filtered using:

```kusto
| where ResultType != 0
```

---

## `ResultDescription`

Answers:

> **Why did authentication fail?**

This is much stronger than simply saying:

> "There was a failed login."

The investigation should try to understand the reason behind the failure.

---

# Phase 5 — Failed Sign-ins by User

I summarized failures by identity:

```kusto
SigninLogs
| where ResultType != 0
| summarize FailedSignins = count() by UserPrincipalName
| sort by FailedSignins desc
```

This moved the investigation from:

> **Failed logins exist.**

to:

> **These accounts are experiencing the failed logins.**

This is an example of using aggregation to turn individual logs into security information.

---

# Phase 6 — Failed Sign-ins by Source IP

I analyzed failed authentication by source IP:

```kusto
SigninLogs
| where ResultType != 0
| summarize
    FailedSignins = count(),
    TargetedUsers = dcount(UserPrincipalName)
    by IPAddress
| sort by FailedSignins desc
```

This answered two different questions.

### `count()`

> How many failures came from each IP?

### `dcount(UserPrincipalName)`

> How many different accounts did each IP touch?

This distinction matters because:

```text
1 IP
100 failures
1 user
```

may represent a different pattern from:

```text
1 IP
100 failures
50 users
```

The second could be more consistent with password spraying.

Neither pattern proves an attack by itself.

---

# Phase 7 — Authentication Timeline Reconstruction

The target account was isolated and events were sorted chronologically.

Example:

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

Chronological ordering helps reconstruct:

```text
Event 1
   ↓
Event 2
   ↓
Event 3
   ↓
Event 4
```

rather than viewing unrelated rows without context.

Timeline reconstruction is important because the analyst needs to understand:

- What happened first
- How quickly the activity occurred
- Whether attempts continued
- Whether authentication eventually succeeded

---

# Phase 8 — Success vs. Failure Correlation

Authentication activity was labeled as success or failure.

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

This highlights one of the most important identity-investigation questions:

> **Did authentication eventually succeed after repeated failures?**

Compare:

```text
FAILURE
FAILURE
FAILURE
```

with:

```text
FAILURE
FAILURE
FAILURE
SUCCESS
```

The second sequence requires more investigation.

A successful authentication after repeated failures may increase concern, especially when the same unusual source IP is involved.

However:

> **Success after failure still does not automatically prove account compromise.**

Context must still be investigated.

---

# Phase 9 — Authentication Failure Analysis

Failure reasons were summarized using:

```kusto
SigninLogs
| where ResultType != 0
| summarize
    FailureCount = count()
    by ResultType, ResultDescription
| sort by FailureCount desc
```

Observed failure information included different authentication failure conditions instead of one identical reason for every event.

This reinforced an important lesson:

```text
OBSERVATION
Authentication failed

        ↓

DEEPER QUESTION
Why did it fail?
```

An analyst should investigate the reason instead of stopping at the alert title.

---

# Phase 10 — Application Authentication Summary

Authentication activity was summarized by application:

```kusto
let TargetUser = "<LAB-ACCOUNT>";
SigninLogs
| where UserPrincipalName =~ TargetUser
| summarize
    Attempts = count(),
    Failures = countif(ResultType != 0),
    Successes = countif(ResultType == 0)
    by AppDisplayName
| sort by Attempts desc
```

This identified the application involved and summarized:

- Total attempts
- Failed attempts
- Successful attempts

Application context helps explain what the identity was attempting to access.

---

# Phase 11 — Time-Based Behavior Analysis

Authentication failures were also analyzed over time.

An early version used:

```kusto
let TargetUser = "<LAB-ACCOUNT>";
SigninLogs
| where UserPrincipalName =~ TargetUser
| where ResultType != 0
| summarize FailedAttempts = count() by bin(TimeGenerated, 5m)
| sort by TimeGenerated asc
```

This introduced an important detection-engineering problem later in the lab.

---

# Phase 12 — Creating a Custom Sentinel Analytics Rule

After manually investigating the authentication activity, I created a custom scheduled analytics rule:

```text
Repeated Failed Microsoft Entra Sign-ins
```

The purpose was to move from manual hunting to automatic detection.

```text
MANUAL HUNTING

Analyst
   ↓
Runs KQL
   ↓
Finds Pattern
```

became:

```text
AUTOMATED DETECTION

SigninLogs
   ↓
Scheduled KQL
   ↓
Detection Condition
   ↓
Alert
   ↓
Incident
```

---

# Detection Configuration

The rule used:

```text
Rule Name:
Repeated Failed Microsoft Entra Sign-ins

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

Medium severity was used because repeated failed authentication deserved investigation, but failed authentication alone did not prove that an account had been compromised.

---

# Entity Mapping

The analytics rule mapped security objects so they could appear as investigation entities.

## Account

```text
Name
→ AccountName

UPNSuffix
→ AccountUPNSuffix
```

## IP Address

```text
Address
→ IPAddress
```

This allowed the resulting alert and incident to connect directly to:

- The affected identity
- The source IP

---

# Custom Alert Details

The rule also included:

```text
FailedAttempts
FirstAttempt
LastAttempt
```

These values gave the analyst immediate context about:

- How many failures occurred
- When the sequence started
- When the sequence ended

---

# Phase 13 — First Detection Logic

The first rule attempted to group activity using a fixed five-minute bucket.

Conceptually:

```kusto
| summarize
    FailedAttempts = count()
    by UserPrincipalName, IPAddress, bin(TimeGenerated, 5m)
| where FailedAttempts >= 3
```

The logic looked correct at first.

Three failed sign-ins within a few minutes should have matched.

However, the rule returned:

```text
0 results
```

---

# Phase 14 — Detection Engineering Troubleshooting

Instead of assuming Microsoft Sentinel was broken, I went back to the raw telemetry.

The source events proved that:

```text
Failed Login 1
Failed Login 2
Failed Login 3
```

really existed.

The events involved:

- The same lab account
- The same source IP
- Similar timestamps
- Failed authentication results

Therefore:

```text
DATA INGESTION
✓ WORKING

SIGNINLOGS
✓ WORKING

FAILED LOGIN EVENTS
✓ PRESENT

DETECTION RESULT
✗ NOT MATCHING
```

That narrowed the problem to the detection logic.

---

# Root Cause — Fixed Time Bucket Boundary

The failed sign-ins occurred close together but crossed a fixed five-minute clock boundary.

Conceptually:

```text
8:29:59 → Bucket 1
8:32:11 → Bucket 2
8:33:19 → Bucket 2
```

The query therefore saw:

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

The query was technically valid, but the logic did not represent the behavior I actually wanted to detect.

This became one of the most valuable lessons in the project:

> **A detection query can be syntactically correct and still be behaviorally wrong.**

---

# Phase 15 — Corrected Detection Logic

The query was changed to evaluate related failures across the rule's recent lookback window instead of splitting them into fixed clock buckets.

The corrected detection query was:

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

---

# Understanding the Corrected Query

## Select the Data Source

```kusto
SigninLogs
```

Use Microsoft Entra interactive sign-in telemetry.

---

## Keep Failed Authentication

```kusto
| where ResultType != 0
```

Remove successful authentication from this detection.

---

## Prepare Account Entity Fields

```kusto
| extend
    AccountName = tostring(split(UserPrincipalName, "@")[0]),
    AccountUPNSuffix = tostring(split(UserPrincipalName, "@")[1])
```

Split the full UPN into fields that can be mapped to a Sentinel Account entity.

---

## Group Related Authentication

```kusto
| summarize
    FailedAttempts = count(),
    FirstAttempt = min(TimeGenerated),
    LastAttempt = max(TimeGenerated)
    by UserPrincipalName, AccountName, AccountUPNSuffix, IPAddress
```

Group authentication by:

- User
- Account name
- UPN suffix
- Source IP

Then calculate:

- Failure count
- First attempt
- Last attempt

---

## Apply Detection Threshold

```kusto
| where FailedAttempts >= 3
```

Return only authentication groups containing at least three failures.

---

## Preserve a Detection Timestamp

```kusto
| extend TimeGenerated = LastAttempt
```

Use the most recent failed attempt as the detection result's event time.

---

# Phase 16 — Detection Validation

The corrected query successfully returned a matching result.

This proved:

```text
TELEMETRY
✓

QUERY
✓

AGGREGATION
✓

THRESHOLD
✓

ENTITY FIELDS
✓

DETECTION RESULT
✓
```

This validation was especially important because the rule was tested against actual telemetry instead of being assumed correct simply because the query had no syntax errors.

---

# Phase 17 — Fresh Detection Test

Fresh controlled failed authentication was generated after the corrected rule was enabled.

The full pipeline then operated automatically:

```text
Failed Sign-in
      ↓
Failed Sign-in
      ↓
Failed Sign-in
      ↓
Microsoft Entra
      ↓
SigninLogs
      ↓
Sentinel Scheduled Rule
      ↓
FailedAttempts >= 3
      ↓
Detection Match
      ↓
Alert
```

---

# Phase 18 — Security Alert Generation

The scheduled analytics rule successfully generated an alert related to:

```text
Repeated Failed Microsoft Entra Sign-ins
```

Alert evidence contained useful information including:

- Affected account
- Source IP
- Failed attempt count
- First attempt
- Last attempt
- Query results
- Medium severity
- Sentinel detection context

At this point, the project had moved from:

> **I can manually find failed logins.**

to:

> **I can build a detection that automatically finds them.**

---

# Phase 19 — Incident Generation

The alert resulted in a Microsoft Defender incident:

```text
Repeated Failed Microsoft Entra Sign-ins
```

The incident was created with:

```text
Severity:
Medium
```

This demonstrated the complete operational path:

```text
RAW AUTHENTICATION
        ↓
KQL
        ↓
DETECTION LOGIC
        ↓
ANALYTICS RULE
        ↓
ALERT
        ↓
INCIDENT
```

---

# Phase 20 — Incident Investigation

Inside Microsoft Defender, I reviewed the incident rather than immediately closing it.

The investigation included:

- Incident name
- Severity
- Status
- Alert
- Account entity
- IP information
- Detection evidence
- Failed attempt count
- Authentication timing
- Related query information
- Incident activity

The key investigation questions were:

```text
WHO?
Which account?

WHERE?
Which IP?

WHEN?
What time?

HOW MANY?
How many attempts?

WHY?
What failure information exists?

WHAT HAPPENED NEXT?
Any success?

WHAT DOES THE CONTEXT SAY?
Expected or unauthorized?
```

---

# Detection vs. Analyst Conclusion

The analytics rule correctly identified repeated failed authentication.

That meant:

> **The detection was valid.**

It did not automatically mean:

> **A real attacker compromised the account.**

Those are two different questions.

```text
DETECTION QUESTION

Did repeated failed authentication occur?
        ↓
YES


INVESTIGATION QUESTION

Was that authentication unauthorized or malicious?
        ↓
Requires Context
```

---

# Phase 21 — Final Analyst Decision

The authentication failures had been intentionally generated during the controlled lab.

The evidence showed:

```text
Did the failures actually happen?
YES

Did Sentinel detect them correctly?
YES

Did the rule behave as intended?
YES

Was the activity authorized?
YES

Was unauthorized compromise identified?
NO
```

The final decision was therefore not to escalate the activity as a real credential attack.

---

# Final Incident Disposition

```text
Incident:
Repeated Failed Microsoft Entra Sign-ins

Severity:
Medium

Status:
Resolved

Classification:
Informational, expected activity — Confirmed activity

Detection Result:
Successful

Unauthorized Compromise:
Not Identified
```

The incident was documented and saved after the investigation was complete.

---

# Why This Was Not a False Positive

The failed sign-ins were real.

The analytics rule correctly identified the behavior it was designed to detect.

Therefore, the detection itself was not wrong.

The important distinction was:

```text
FALSE POSITIVE

Detection says behavior happened
        ↓
Behavior did not actually match intended condition
```

versus:

```text
EXPECTED / BENIGN ACTIVITY

Detection says behavior happened
        ↓
Behavior really happened
        ↓
Investigation finds authorized explanation
```

Lab 04 matched the second case.

---

# Why No Containment Was Required

No containment action was necessary because the investigation did not identify unauthorized compromise.

I did not need to:

- Disable the account
- Force a password reset
- Revoke sessions
- Block the source IP
- Isolate an endpoint
- Escalate to incident response

Those actions would not have been supported by the evidence.

In a real environment, containment could become appropriate if additional evidence showed:

- Successful unauthorized login
- MFA changes
- Password reset activity
- Privilege escalation
- Suspicious mailbox access
- New device registration
- Unusual cloud resource access
- Known malicious source IPs
- Suspicious activity immediately after authentication

---

# Lab 04 Evidence-Based Investigation Model

The completed investigation can be summarized as:

```text
OBSERVATION
Repeated failed sign-ins occurred
        ↓
CONTEXT
Dedicated lab identity and known test environment
        ↓
CORRELATION
Same account, source IP, timestamps, application, and authentication evidence
        ↓
INTERPRETATION
Pattern resembled credential guessing
        ↓
VALIDATION
Raw SigninLogs + corrected KQL + analytics rule + generated alert
        ↓
DECISION
Expected controlled activity
        ↓
DISPOSITION
Resolved
```

---

# Lab 04 Major Technical Lessons

## 1. A SIEM cannot investigate data that it never received

The first `SigninLogs` query returned no records because sign-in telemetry was not being forwarded into the workspace.

This required fixing the telemetry pipeline before investigating the authentication activity.

---

## 2. Always verify the source data

The raw authentication events were checked before assuming that the analytics rule was working or broken.

---

## 3. Authentication failures require context

A failed login can represent:

- User error
- Expired password
- Application problem
- Password guessing
- Password spraying
- Brute force
- Controlled testing

The event alone does not provide the final answer.

---

## 4. Timeline analysis changes the meaning of events

Three failed attempts over several months are very different from three failed attempts within several minutes.

---

## 5. Success after failure deserves attention

A sequence such as:

```text
FAIL
FAIL
FAIL
SUCCESS
```

can increase the importance of an authentication investigation.

It still requires context before declaring compromise.

---

## 6. Raw logs and alerts serve different purposes

Raw logs answer:

> **What actually happened?**

Alerts answer:

> **What behavior matched detection logic?**

Incidents answer:

> **What security case should the analyst investigate?**

---

## 7. Detection logic must be tested

The original time-binning approach was technically correct KQL but did not match the intended behavior because related events crossed a clock boundary.

Testing exposed the problem.

---

## 8. Troubleshooting should isolate the broken layer

The troubleshooting sequence became:

```text
Does the activity exist?
YES
        ↓
Did Entra record it?
YES
        ↓
Did SigninLogs receive it?
YES
        ↓
Does basic KQL see it?
YES
        ↓
Does detection query match it?
NO
        ↓
Problem = Detection Logic
```

This is more effective than randomly changing settings.

---

## 9. A correct detection does not equal malicious intent

The detection worked.

The incident was still resolved as expected activity because the investigation established an authorized explanation.

---

# Lab 04 Portfolio Evidence

The strongest evidence captured during the lab included:

```text
week17_Lab4_initial_failed_signins.png
week17_Lab4_failed_signins_by_user.png
week17_Lab4_failed_signins_by_ip.png
week17_Lab4_user_authentication_timeline.png
week17_Lab4_success_failure_correlation.png
week17_Lab4_failure_reason_analysis.png
week17_Lab4_application_authentication_summary.png
week17_Lab4_recent_failed_signins.png
week17_Lab4_detection_rule_validation.png
week17_Lab4_detection_rule_match.png
week17_Lab4_failed_login_analytics_rule.png
week17_Lab4_failed_login_incident_queue.png
week17_Lab4_failed_login_alert_evidence.png
week17_Lab4_incident_overview.png
week17_Lab4_affected_entities.png
week17_Lab4_incident_timeline.png
week17_Lab4_final_incident_disposition.png
```

Not every troubleshooting screenshot needs to be published.

The strongest screenshots are the ones that prove:

```text
Evidence
   ↓
Analysis
   ↓
Detection
   ↓
Incident
   ↓
Decision
```

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

MITRE ATT&CK Mapping:
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

# Week 17 Detection Engineering Lessons

Across the Sentinel labs, detection engineering became easier to understand as a process rather than a product feature.

```text
1. Identify the behavior
        ↓
2. Identify the telemetry
        ↓
3. Identify the correct table
        ↓
4. Understand the schema
        ↓
5. Write KQL
        ↓
6. Test against real data
        ↓
7. Identify false negatives
        ↓
8. Fix logic
        ↓
9. Set severity
        ↓
10. Map entities
        ↓
11. Map MITRE ATT&CK
        ↓
12. Configure schedule
        ↓
13. Generate alert
        ↓
14. Generate incident
        ↓
15. Investigate outcome
        ↓
16. Tune when necessary
```

The most important lesson was:

> **A query that runs successfully is not automatically a good detection.**

Good detection logic must represent the behavior correctly.

---

# Week 17 Investigation Method

A repeatable investigation method developed across the four labs:

```text
OBSERVATION
What do I know happened?
        ↓
CONTEXT
Which user, device, IP, process, resource, or application is involved?
        ↓
CORRELATION
What other evidence exists?
        ↓
TIMELINE
What happened before and after?
        ↓
INTERPRETATION
What explanation best matches the evidence?
        ↓
VALIDATION
What would prove or disprove that explanation?
        ↓
DECISION
What should happen next?
```

---

# SOC Skills Demonstrated

## Alert Triage

- Reviewed alerts
- Reviewed severity
- Identified affected entities
- Determined detection source
- Compared alert claims with raw telemetry

## Incident Investigation

- Opened incidents
- Reviewed related alerts
- Reviewed users
- Reviewed devices
- Reviewed IP entities
- Reviewed timelines
- Correlated evidence
- Documented conclusions

## Endpoint Investigation

- Validated Defender onboarding
- Reviewed endpoint telemetry
- Analyzed process lineage
- Used Device Timeline
- Used Advanced Hunting
- Evaluated response actions

## Identity Investigation

- Investigated Microsoft Entra authentication
- Queried `SigninLogs`
- Identified failed sign-ins
- Compared successes and failures
- Analyzed authentication failure reasons
- Investigated source IP information
- Built identity timelines

## SIEM Analysis

- Configured Microsoft Sentinel
- Validated data ingestion
- Used Azure Activity
- Used Entra sign-in telemetry
- Queried Log Analytics
- Aggregated events
- Identified behavioral patterns

## Detection Engineering

- Reviewed built-in Sentinel analytics rules
- Created a custom scheduled analytics rule
- Configured thresholds
- Configured scheduling
- Configured lookback periods
- Mapped entities
- Added custom alert details
- Mapped MITRE ATT&CK
- Tested detection logic
- Identified a detection failure
- Performed root cause analysis
- Corrected KQL
- Validated the corrected detection
- Generated a real alert and incident from controlled activity

## Incident Response Decision-Making

- Distinguished alert from compromise
- Distinguished expected activity from malicious behavior
- Evaluated whether containment was justified
- Documented analyst conclusions
- Resolved incidents appropriately

---

# KQL Skills Demonstrated

Week 17 included practical use of:

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
```

These operators were used for actual investigation questions rather than only syntax practice.

---

# KQL Investigation Mindset

KQL became a way to ask security questions.

```text
QUESTION
Which users have the most failures?

KQL
summarize count() by UserPrincipalName
```

```text
QUESTION
Which IP generated the failures?

KQL
summarize count() by IPAddress
```

```text
QUESTION
How many different users did one IP touch?

KQL
dcount(UserPrincipalName)
```

```text
QUESTION
Did authentication eventually succeed?

KQL
iff(ResultType == 0, "SUCCESS", "FAILURE")
```

```text
QUESTION
When did suspicious activity begin and end?

KQL
min(TimeGenerated)
max(TimeGenerated)
```

The important skill is not memorizing KQL.

It is learning how to convert an investigation question into a query.

---

# MITRE ATT&CK Coverage

Week 17 included practical exposure to ATT&CK mapping.

| Technique | Context |
| --- | --- |
| **T1110 — Brute Force** | Repeated Microsoft Entra failed sign-in detection |
| **T1496 — Resource Hijacking** | Sentinel suspicious resource deployment analytics-rule review |

MITRE ATT&CK was treated as a behavior-description system rather than proof of attacker activity.

```text
Detection
    ↓
Observed Behavior
    ↓
ATT&CK Mapping
    ↓
Standardized Technique
```

---

# Security Engineering Lessons

The labs also reinforced several engineering principles.

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

## Detection Comes Before Automation

A detection should be tested before response actions are automated.

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

## Troubleshooting Must Be Layered

```text
SOURCE
   ↓
CONNECTOR
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
```

Each layer should be tested separately.

---

# Core Security Operations Lessons

The most important lesson from Week 17 was that effective Security Operations depends on understanding what sits underneath the product interface.

A strong analyst should understand:

- Processes
- Files
- Users
- Authentication
- Authorization
- Network connections
- IP addresses
- Logs
- Identity
- Permissions
- Detection logic
- Risk
- Evidence
- Timelines
- Context
- Correlation
- Response decisions

Microsoft Defender and Microsoft Sentinel provide the interface.

The real security work is understanding what the data means.

---

# What I Would Do in a Real Failed Login Incident

If Lab 04 had involved unknown activity instead of controlled testing, I would continue investigating:

- Whether the source IP was known for the user
- Whether the location was normal
- Whether the user was traveling
- Whether the IP had threat-intelligence matches
- Whether other users were targeted
- Whether a successful authentication followed the failures
- Whether MFA was completed
- Whether MFA methods were changed
- Whether password reset activity occurred
- Whether new devices were registered
- Whether privileged roles changed
- Whether suspicious cloud activity followed authentication
- Whether Microsoft 365 activity became suspicious
- Whether Defender had endpoint alerts involving the same identity

Possible containment could include:

- Force password reset
- Revoke active sessions
- Disable the user
- Block malicious IP infrastructure where appropriate
- Require MFA reauthentication
- Investigate affected endpoints
- Escalate to Incident Response

Those actions would only be taken if supported by the evidence and organizational procedures.

---

# Hiring Manager / Recruiter Quick View

## What I Actually Built

I did not only watch tutorials about Microsoft security products.

I configured and operated a working lab that included:

- Microsoft Azure
- Microsoft Entra ID
- Azure Log Analytics
- Microsoft Sentinel
- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Windows 11 Enterprise endpoint telemetry
- Azure administrative telemetry
- Microsoft Entra authentication telemetry
- KQL threat hunting
- Custom detection logic
- Alert generation
- Incident generation
- Incident investigation
- Incident disposition

---

## What I Actually Investigated

### Endpoint Security

- Suspicious PowerShell activity
- Endpoint process execution
- Parent-child process chains
- Defender endpoint telemetry

### Cloud Security

- Azure administrative operations
- Azure control-plane telemetry
- Resource activity

### Identity Security

- Failed Microsoft Entra authentication
- Repeated failed sign-ins
- Source IP behavior
- Authentication timelines
- Success vs. failure sequences
- Failure reasons

---

## What I Actually Created

- Azure security lab infrastructure
- Log Analytics workspace
- Microsoft Sentinel environment
- Defender for Endpoint-connected Windows endpoint
- KQL investigation queries
- Custom scheduled Sentinel analytics rule
- Account entity mapping
- IP entity mapping
- Alert custom details
- MITRE ATT&CK mapping
- Microsoft Defender security incident

---

## What I Actually Troubleshot

- Defender endpoint onboarding
- Missing Defender service dependencies
- Missing Entra `SigninLogs`
- Diagnostic settings
- Log Analytics ingestion
- Query results
- Sentinel entity mappings
- Scheduled analytics-rule timing
- Detection logic that failed across fixed time buckets

---

# Interview-Ready Project Explanation

> I built and operated a Microsoft security lab using Microsoft Defender XDR, Defender for Endpoint, Microsoft Sentinel, Microsoft Entra ID, Azure Log Analytics, and KQL.
>
> I started by configuring the cloud and endpoint environment, including a Sentinel-connected Log Analytics workspace and a Windows 11 Enterprise endpoint onboarded to Defender for Endpoint. I generated controlled endpoint activity, investigated the resulting Defender detection, reviewed process lineage, and validated the underlying endpoint events with Advanced Hunting.
>
> I then expanded the environment into Sentinel by connecting Azure administrative telemetry and using KQL to investigate the `AzureActivity` table. I practiced filtering, projection, aggregation, time analysis, analytics rules, and MITRE ATT&CK mapping.
>
> In the fourth lab, I performed a complete identity-security investigation. I configured Microsoft Entra diagnostic settings so interactive sign-in activity reached `SigninLogs`, generated controlled failed authentication attempts, and used KQL to investigate the account, source IP, timing, application, and failure reasons.
>
> I created a scheduled Microsoft Sentinel analytics rule for repeated failed sign-ins and mapped the account and IP entities. During testing, my first detection did not trigger even though the raw events existed. I traced the issue to fixed five-minute time buckets that split related authentication attempts across separate groups. I corrected the KQL, validated the new logic, and successfully generated a Microsoft Sentinel alert and Microsoft Defender incident.
>
> I investigated the resulting incident and determined that the detection was valid but the behavior was expected because I intentionally generated it during the lab. I documented the evidence and resolved the incident as confirmed expected activity.
>
> The biggest lesson from the project was that security operations is not about trusting an alert title. I need to understand the telemetry, validate the detection logic, correlate the evidence, reconstruct what happened, and make a conclusion that the evidence actually supports.

---

# Example Interview Question — "How Do You Investigate Failed Logins?"

My process would be:

```text
1. Confirm the detection
        ↓
2. Identify the affected account
        ↓
3. Identify source IP addresses
        ↓
4. Determine failure volume
        ↓
5. Determine timing and frequency
        ↓
6. Review failure reasons
        ↓
7. Identify targeted applications
        ↓
8. Determine whether other accounts were targeted
        ↓
9. Look for successful authentication afterward
        ↓
10. Compare with normal user behavior
        ↓
11. Correlate endpoint/cloud evidence
        ↓
12. Decide benign vs suspicious vs malicious
        ↓
13. Contain/escalate if justified
        ↓
14. Document the evidence and decision
```

---

# Example Interview Question — "What Is the Difference Between an Alert and an Incident?"

An alert tells me:

> **A detection condition matched.**

An incident tells me:

> **This is the security case I need to investigate.**

The relationship is:

```text
Telemetry
    ↓
Detection Rule
    ↓
Alert
    ↓
Incident
    ↓
Investigation
```

An incident can contain multiple alerts and connect users, devices, IP addresses, files, processes, and other evidence.

---

# Example Interview Question — "What Did You Learn About Detection Engineering?"

> A detection rule is not good just because the query runs without an error.
>
> In my failed-login lab, I initially grouped authentication events into fixed five-minute buckets. Three failed logins happened within only a few minutes, but one was on one side of the clock boundary and the other two were on the other side. The rule therefore failed to meet its threshold.
>
> I verified that the raw telemetry existed, isolated the problem to the query logic, changed the detection to use the scheduled rule's lookback window, tested it again, and successfully generated the expected alert and incident.
>
> That taught me that detection engineering requires testing the behavior, not just validating KQL syntax.

---

# Example Interview Question — "How Do You Avoid Overreacting to Security Alerts?"

I separate:

```text
OBSERVATION
What happened?

from

INTERPRETATION
What does it mean?

from

DECISION
What should I do?
```

For example:

```text
Observation:
Repeated failed authentication occurred.

Interpretation:
Could resemble password guessing.

Context:
Activity was generated intentionally in a controlled lab.

Decision:
No containment required. Resolve as expected activity.
```

This helps prevent an alert title from becoming an unsupported conclusion.

---

# Week 17 Final Findings

The four labs established that:

- A Microsoft cloud security environment can be built from the ground up.
- Defender for Endpoint depends on working endpoint services and capabilities.
- Endpoint telemetry can be independently validated with Advanced Hunting.
- XDR incidents provide broader context than individual alerts.
- Device and user entities help analysts pivot through related evidence.
- Process lineage gives more context than evaluating one process name.
- Log Analytics provides the underlying telemetry store for Sentinel.
- Sentinel depends on correctly configured security data sources.
- Azure Activity events can be queried through `AzureActivity`.
- Microsoft Entra authentication can be queried through `SigninLogs`.
- `where` can isolate investigation-relevant activity.
- `project` can reduce noise and expose useful evidence.
- `summarize` can turn individual events into behavior.
- `count()` can measure event volume.
- `dcount()` can measure distinct targeted entities.
- Chronological sorting can reconstruct an incident.
- Authentication result codes help explain why sign-ins failed.
- Success/failure correlation can change an incident's risk.
- Scheduled analytics rules can automate KQL detections.
- Entity mapping improves alert and incident context.
- MITRE ATT&CK provides standardized language for suspicious behavior.
- A rule can be syntactically correct but logically incomplete.
- Raw telemetry should be used to troubleshoot failed detections.
- Alerts should be treated as investigation starting points.
- A valid detection can still represent expected activity.
- Incident disposition should be supported by evidence.

---

# Final Technical Outcome

Week 17 demonstrates practical exposure to the complete security-monitoring lifecycle:

> **Telemetry Generation → Collection → Ingestion → Storage → Querying → Hunting → Detection → Alerting → Correlation → Investigation → Validation → Classification → Response → Documentation**

The four labs progressed from:

```text
BUILD
    ↓
MONITOR
    ↓
QUERY
    ↓
DETECT
    ↓
INVESTIGATE
    ↓
VALIDATE
    ↓
DECIDE
```

The environment included endpoint, cloud, and identity security rather than focusing on only one source of telemetry.

---

# Skills Demonstrated

### Microsoft Security

- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Microsoft Sentinel
- Microsoft Entra ID
- Microsoft Azure
- Azure Log Analytics

### Security Operations

- SOC alert triage
- Incident investigation
- Incident lifecycle management
- Evidence correlation
- Entity investigation
- Timeline analysis
- Root cause analysis
- Incident classification
- Incident documentation
- Response decision-making

### SIEM & Detection Engineering

- Security telemetry ingestion
- Data connector configuration
- Diagnostic settings
- KQL
- Scheduled analytics rules
- Detection thresholds
- Rule scheduling
- Lookback configuration
- Entity mapping
- Alert enrichment
- Detection validation
- Detection troubleshooting
- Detection tuning

### Endpoint Security

- Endpoint onboarding
- Defender service validation
- Process investigation
- Process lineage
- Device Timeline
- Advanced Hunting
- EDR alert investigation

### Identity Security

- Microsoft Entra authentication
- `SigninLogs`
- Failed login investigation
- Source IP analysis
- Authentication timeline reconstruction
- Success/failure correlation
- Authentication failure analysis
- Identity entity mapping

### Frameworks

- MITRE ATT&CK
- T1110 — Brute Force
- T1496 — Resource Hijacking

---

# Security Operations Mindset

The strongest lesson from Week 17 can be summarized as:

```text
ALERT
  ≠
CONCLUSION
```

Instead:

```text
ALERT
   ↓
EVIDENCE
   ↓
CONTEXT
   ↓
CORRELATION
   ↓
TIMELINE
   ↓
VALIDATION
   ↓
CONCLUSION
   ↓
ACTION
```

The tools help organize the evidence.

The analyst still has to understand what the evidence means.

---

# Final Project Status

```text
Microsoft Security Environment:
COMPLETED

Microsoft Defender for Endpoint:
CONFIGURED & VALIDATED

Microsoft Defender XDR Investigation:
COMPLETED

Microsoft Sentinel:
CONFIGURED & VALIDATED

Azure Activity Ingestion:
VALIDATED

Microsoft Entra Sign-in Ingestion:
VALIDATED

KQL Investigation:
COMPLETED

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

Final Incident Investigation:
COMPLETED

Final Incident Disposition:
RESOLVED — EXPECTED ACTIVITY
```

---

# Key Takeaway

> **Security tools generate information. Security analysts turn that information into defensible decisions.**

Week 17 strengthened my ability to move from raw endpoint, cloud, and identity telemetry to a final security conclusion using Microsoft Defender XDR, Microsoft Sentinel, Microsoft Entra ID, Azure Log Analytics, and KQL.

The most important outcome was not learning where buttons are located.

It was learning how to follow the evidence:

```text
What happened?
      ↓
Who or what was involved?
      ↓
Where did the evidence come from?
      ↓
What happened before and after?
      ↓
What does the raw telemetry show?
      ↓
Why did the detection trigger?
      ↓
Does the detection logic make sense?
      ↓
What explanation best fits the evidence?
      ↓
What action is actually justified?
```

That is the mindset I am continuing to build as I go about documenting my investigations.
