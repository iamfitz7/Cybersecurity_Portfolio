# Week 17 — Microsoft Defender XDR & Sentinel Security Operations

> **Focus:** Microsoft Defender XDR • Microsoft Defender for Endpoint • Microsoft Sentinel • Microsoft Entra ID • Azure Log Analytics • Kusto Query Language (KQL) • Endpoint Detection and Response (EDR) • Security Information and Event Management (SIEM) • Extended Detection and Response (XDR) • Threat Hunting • Detection Engineering Fundamentals • Incident Investigation • Incident Response

---

## Overview

Week 17 focuses on building, validating, and operating a Microsoft-based Security Operations environment using **Microsoft Defender XDR, Microsoft Defender for Endpoint, Microsoft Sentinel, Microsoft Entra ID, Azure Log Analytics, and Kusto Query Language (KQL)**.

Rather than beginning with an already configured security platform, this week starts by building the cloud and endpoint infrastructure required to support security monitoring. The environment is then used to generate controlled security activity, validate endpoint and cloud telemetry, investigate Microsoft Defender detections, hunt security data with KQL, examine Sentinel analytics rules, map detections to MITRE ATT&CK, and practice investigation workflows used within modern Security Operations Centers.

Week 17 consists of three complementary labs:

### Lab 01 — Microsoft Security Environment Setup & EDR Investigation

Lab 01 focuses on building the underlying Microsoft security environment. This includes configuring Azure resources, creating a Log Analytics workspace, enabling Microsoft Sentinel, configuring Microsoft Defender for Endpoint, onboarding a Windows 11 Enterprise endpoint, validating Defender services and telemetry, generating controlled security activity, investigating the resulting EDR detection, hunting supporting endpoint telemetry with KQL, classifying the activity, and securely cleaning up temporary test infrastructure.

### Lab 02 — Microsoft Defender XDR Fundamentals

Lab 02 expands from investigating a single controlled endpoint detection into the broader Microsoft Defender XDR Security Operations workflow. The lab focuses on the Defender XDR dashboard, device inventory, incident queue, incident correlation, alert investigation, evidence and entity analysis, device investigation, Device Timeline analysis, process lineage, user investigation, User Timeline analysis, incident lifecycle management, and endpoint response capabilities.

### Lab 03 — Microsoft Sentinel & KQL Fundamentals

Lab 03 expands the environment from endpoint-focused XDR investigation into **cloud SIEM monitoring, telemetry ingestion, KQL analysis, behavioral aggregation, and detection engineering fundamentals**.

The lab follows Azure administrative activity from its original source through the complete Sentinel telemetry pipeline:

```text
Azure Administrative Activity
            │
            ▼
     Azure Activity Log
            │
            ▼
   Azure Activity Connector
            │
            ▼
   Log Analytics Workspace
            │
            ▼
      AzureActivity Table
            │
            ▼
        KQL Analysis
            │
            ├── Filtering
            ├── Projection
            ├── Aggregation
            ├── Time Analysis
            └── Behavioral Analysis
            │
            ▼
 Sentinel Analytics Rules
            │
            ▼
     MITRE ATT&CK Mapping
            │
            ▼
      Alert / Incident
            │
            ▼
      Analyst Investigation
```

The purpose of Lab 03 is not simply to demonstrate navigation through Microsoft Sentinel.

The goal is to understand and validate the relationship between:

> **Data Source → Data Connector → Log Analytics → Security Table → KQL → Detection Logic → Alert → Incident → Analyst Investigation**

Together, the three labs progress from **building the security platform**, to **operating the XDR investigation environment**, to **querying and analyzing cloud SIEM telemetry and understanding how that telemetry becomes automated detection logic**.

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
- Generate controlled endpoint security test activity
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
- Configure and validate Microsoft Sentinel data ingestion
- Understand the role of Microsoft Sentinel data connectors
- Understand the relationship between Microsoft Sentinel and Log Analytics
- Validate Azure Activity telemetry
- Investigate the `AzureActivity` table
- Understand tables, rows, columns, and schemas within security telemetry
- Use KQL to filter security events by time
- Use KQL to filter specific Azure resources
- Use KQL to select investigation-relevant fields
- Use KQL to sort events chronologically
- Use KQL to count and aggregate events
- Use `summarize` to identify behavioral patterns
- Use `bin()` to group activity into time intervals
- Analyze frequently occurring Azure operations
- Search for failed administrative operations
- Understand manual hunting versus automated detection
- Examine Microsoft Sentinel analytics-rule templates
- Understand scheduled analytics-rule logic
- Review rule severity and data-source requirements
- Review MITRE ATT&CK mappings within Sentinel detections
- Understand how analytics rules can produce alerts
- Understand how alerts can become incidents
- Understand Sentinel workbooks and watchlists
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
        │
        ├── Data Connectors
        ├── Analytics
        ├── Incidents
        ├── Hunting
        ├── Workbooks
        ├── Watchlists
        └── Security Content
```

This portion of the environment provides the identity, cloud-resource, centralized logging, SIEM, detection, and investigation foundation supporting the security lab.

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

The endpoint layer provides detailed host telemetry required to investigate processes, users, files, command lines, detections, and other security-relevant activity.

---

## SIEM Telemetry Layer

```text
Azure Subscription
        │
        ▼
Administrative Operations
        │
        ▼
Azure Activity Log
        │
        ▼
Azure Activity Data Connector
        │
        ▼
LAW-Microsoft-Security-Lab
        │
        ▼
AzureActivity
        │
        ▼
KQL
        │
        ├── where
        ├── project
        ├── count
        ├── summarize
        ├── top
        ├── order by
        ├── bin()
        └── render
        │
        ▼
Security Analysis
        │
        ▼
Analytics Rule
        │
        ▼
Alert
        │
        ▼
Incident
```

This architecture demonstrates that a SIEM depends on more than a dashboard.

Security data must first be **generated, transported, stored, structured, queried, and evaluated** before it can support meaningful detection and investigation.

---

# Technologies & Platforms

| Technology | Purpose |
| --- | --- |
| **Microsoft Defender XDR** | Unified detection, incident correlation, investigation, entity analysis, and response |
| **Microsoft Defender for Endpoint** | Endpoint Detection and Response, endpoint telemetry, device investigation, and endpoint response |
| **Microsoft Sentinel** | Cloud-native SIEM, security analytics, detection, hunting, investigation, and security operations |
| **Microsoft Entra ID** | Identity, tenant, user, and administrative access management |
| **Azure Log Analytics** | Centralized security telemetry storage and query workspace |
| **Azure Activity Log** | Records subscription-level Azure control-plane activity |
| **Azure Activity Connector** | Sends Azure administrative activity into the Sentinel-connected Log Analytics workspace |
| **Microsoft Azure** | Cloud infrastructure supporting Sentinel, Log Analytics, and Azure administrative telemetry |
| **Windows 11 Enterprise** | Monitored enterprise endpoint used for endpoint security investigation |
| **Kusto Query Language (KQL)** | Security telemetry querying, filtering, aggregation, hunting, and investigation |
| **Microsoft Defender Advanced Hunting** | Query interface for investigating Defender and available Sentinel security telemetry |
| **Sentinel Analytics Rules** | Automated detection logic used to continuously evaluate security telemetry |
| **MITRE ATT&CK** | Framework used to map detection logic to adversary tactics and techniques |
| **PowerShell** | Endpoint administration, validation, troubleshooting, and controlled test activity |
| **Oracle VirtualBox** | Virtualization platform supporting the Windows lab endpoint |

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

## Endpoint Onboarding & Troubleshooting

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

The troubleshooting process required validating several layers of endpoint configuration rather than assuming the onboarding package itself was the source of the problem.

> **Security platforms depend on underlying services, operating-system capabilities, licensing, connectivity, permissions, and telemetry pipelines. Effective troubleshooting requires validating those dependencies rather than repeatedly rerunning a failed deployment command.**

---

## Controlled Detection Test

After the endpoint was successfully onboarded, controlled Microsoft Defender security test activity was generated to validate the detection pipeline.

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

## Alert Investigation

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

> **An alert represents a reason to investigate. It does not automatically represent the final conclusion.**

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

The process lineage helped establish:

- Which process initiated the activity
- The user execution context
- How PowerShell was launched
- What occurred before PowerShell execution
- Whether the execution chain matched the controlled test
- Whether unexpected parent processes were present
- Whether unexpected child processes were present

---

## Advanced Hunting with KQL

Microsoft Defender Advanced Hunting was used to independently search the endpoint telemetry supporting the detection.

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

The hunt returned relevant endpoint events associated with the controlled test activity.

This independently validated that the alert was supported by underlying endpoint telemetry.

---

## Lab 01 Incident Classification

```text
Detection Result: Valid Detection
Activity Type: Authorized Security Testing
Compromise: No
Analyst Disposition: Expected / Security Testing
```

A security detection can be technically valid even when the underlying behavior was intentionally generated.

The detection correctly identified the behavior.

The analyst's responsibility was to determine the context and intent behind that behavior.

---

# Lab 02 — Microsoft Defender XDR Fundamentals

## Objective

Develop practical familiarity with the **Microsoft Defender XDR investigation environment** and understand how a SOC analyst moves from high-level security information into the incidents, alerts, evidence, devices, users, timelines, processes, and response capabilities associated with a security case.

Where Lab 01 focused on:

> **Can I build and validate the Microsoft security environment?**

Lab 02 focused on:

> **Can I navigate and investigate within that environment like a security analyst?**

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

## Alert vs. Incident

### Alert

An **alert** represents a specific security detection.

```text
Observed Telemetry
       │
       ▼
Detection Logic
       │
       ▼
Security Alert
```

An alert can contain:

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

> **An alert does not automatically establish malicious intent or compromise.**

### Incident

An **incident** represents the broader correlated security case.

```text
Alert A ─────┐
             │
Alert B ─────┼────► Incident
             │
Alert C ─────┘
```

An incident can connect:

- Alerts
- Devices
- Users
- Processes
- Files
- IP addresses
- Evidence
- Investigation information
- Response activity

The analyst's perspective therefore changes from:

> **"What does this individual alert say?"**

to:

> **"What security activity is occurring across the environment, and how are these signals related?"**

---

## Device Investigation

Device entity pages were reviewed to understand the affected endpoint in greater context.

Relevant areas included:

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
- Response actions

---

## Device Timeline

The Device Timeline was used to reconstruct endpoint activity chronologically.

```text
Before Detection
      │
      ▼
Process Execution
      │
      ▼
Related Activity
      │
      ▼
Detection
      │
      ▼
Post-Detection Activity
```

This helps determine what happened before, during, and after suspicious activity.

---

## Process Lineage

Process-tree investigation provides context around process execution.

Instead of evaluating:

```text
powershell.exe
```

in isolation, the analyst investigates:

```text
Parent Process
      │
      ▼
Process
      │
      ▼
Command Line
      │
      ▼
Child Process
      │
      ▼
Related File / Network Activity
```

This helps distinguish normal administrative activity from suspicious execution patterns.

---

## Response Capabilities

Endpoint response capabilities reviewed included:

- Isolate device
- Run antivirus scan
- Collect investigation package
- Initiate Live Response
- Restrict application execution
- Investigate device
- Review remediation actions

These capabilities were evaluated in context rather than automatically executed.

> **The existence of a response capability does not mean it should immediately be used. Response actions should be based on evidence, business impact, incident severity, scope, and confidence.**

---

# Lab 03 — Microsoft Sentinel & KQL Fundamentals

## Objective

Develop practical experience with the Microsoft Sentinel SIEM data pipeline and build a foundational understanding of how security telemetry moves from its original source into a searchable security dataset and eventually into automated detection logic.

Where Lab 01 asked:

> **Can I build and validate the Microsoft security environment?**

And Lab 02 asked:

> **Can I investigate security activity through Microsoft Defender XDR?**

Lab 03 asks:

> **Can I understand where SIEM data comes from, validate that it is being collected, query it with KQL, identify patterns, and understand how those queries can become automated detections?**

The core technical workflow was:

```text
Generate Azure Activity
        │
        ▼
Verify Source Event
        │
        ▼
Ingest Through Data Connector
        │
        ▼
Store in Log Analytics
        │
        ▼
Inspect AzureActivity Schema
        │
        ▼
Query with KQL
        │
        ▼
Filter Relevant Activity
        │
        ▼
Aggregate Events
        │
        ▼
Identify Behavioral Patterns
        │
        ▼
Review Analytics Rule
        │
        ▼
Review MITRE ATT&CK Mapping
        │
        ▼
Understand Alert / Incident Creation
```

---

## Lab 03 Environment

Lab 03 reused the Microsoft cloud security environment established earlier in Week 17.

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
                    ├── Azure Activity Solution
                    ├── Azure Activity Connector
                    ├── AzureActivity Table
                    ├── Analytics
                    ├── Incidents
                    ├── Workbooks
                    └── Watchlists
```

A temporary resource group was also used to generate safe Azure control-plane telemetry:

```text
rg-sentinel-kql-lab3-test
```

The temporary resource group was used as a controlled source of administrative activity rather than deploying unnecessary compute resources.

---

# Understanding the Sentinel Data Pipeline

One of the most important lessons from Lab 03 was understanding that Microsoft Sentinel does not automatically have visibility into every event simply because Sentinel has been enabled.

A complete telemetry path must exist.

```text
Security-Relevant Activity
          │
          ▼
      Data Source
          │
          ▼
     Data Connector
          │
          ▼
 Log Analytics Workspace
          │
          ▼
    Structured Table
          │
          ▼
         KQL
          │
          ▼
Security Investigation
```

For this lab:

```text
Azure Administrative Activity
          │
          ▼
    Azure Activity Log
          │
          ▼
 Azure Activity Connector
          │
          ▼
LAW-Microsoft-Security-Lab
          │
          ▼
    AzureActivity
          │
          ▼
     KQL Analysis
```

This relationship is fundamental to SIEM engineering.

If the source does not generate telemetry, the SIEM has nothing to ingest.

If the connector is not configured correctly, the telemetry does not reach the workspace.

If the data is not stored in an expected table, the analyst cannot query it as expected.

If the analyst does not understand the schema, useful investigation becomes more difficult.

---

# Log Analytics Workspace vs. Microsoft Sentinel

An important distinction reinforced during this lab was the difference between **Azure Log Analytics** and **Microsoft Sentinel**.

## Log Analytics Workspace

The Log Analytics workspace provides the underlying environment where telemetry is stored in structured tables and queried.

```text
Log Analytics
      │
      ├── Tables
      ├── Rows
      ├── Columns
      ├── Retention
      └── Queries
```

## Microsoft Sentinel

Microsoft Sentinel adds security-focused SIEM capabilities over the collected data.

```text
Microsoft Sentinel
      │
      ├── Data Connectors
      ├── Analytics Rules
      ├── Incidents
      ├── Hunting
      ├── Workbooks
      ├── Watchlists
      └── Automation Capabilities
```

The relationship can therefore be represented as:

```text
Telemetry
   │
   ▼
Log Analytics
   │
   ▼
Structured Security Data
   │
   ▼
Microsoft Sentinel
   │
   ├── Detect
   ├── Investigate
   ├── Hunt
   └── Respond
```

---

# Azure Activity Data Connector

The Azure Activity connector was configured so subscription-level administrative events could be sent to the existing Log Analytics workspace.

```text
Azure Subscription
        │
        ▼
Azure Control-Plane Activity
        │
        ▼
Azure Activity Log
        │
        ▼
Azure Activity Connector
        │
        ▼
LAW-Microsoft-Security-Lab
        │
        ▼
AzureActivity
```

Connector validation was an important step.

Simply configuring a connector was not treated as proof that telemetry was available.

> **Configuration is not the same as validation.**

---

# Controlled Azure Telemetry Generation

A lightweight resource group was created to generate safe Azure administrative telemetry.

```text
rg-sentinel-kql-lab3-test
```

The goal was not to create suspicious or destructive cloud activity.

The goal was to create known activity that could be traced through the monitoring pipeline.

```text
Known Administrative Action
          │
          ▼
Azure Records Event
          │
          ▼
Connector Ingests Event
          │
          ▼
Log Analytics Stores Event
          │
          ▼
Analyst Queries Event
```

Using known activity made it possible to validate the SIEM pipeline without relying on unknown background events.

---

# Understanding the `AzureActivity` Schema

The `AzureActivity` table was inspected before more advanced queries were performed.

A security table can be understood as:

```text
Table
 │
 ├── Row
 │    └── Individual Event
 │
 └── Columns
      └── Attributes Describing Event
```

Relevant fields observed or investigated included:

- `TimeGenerated`
- `OperationNameValue`
- `ActivityStatusValue`
- `ResourceGroup`
- `Caller`
- `CallerIpAddress`
- `SubscriptionId`
- `ResourceId`
- `CategoryValue`

Understanding the schema matters because KQL queries depend on knowing what information is available.

An analyst should not blindly copy a query without understanding the fields being referenced.

---

# KQL Fundamentals

## First `AzureActivity` KQL Query

```kusto
AzureActivity
```

A limited version can be written as:

```kusto
AzureActivity
| take 10
```

The pipe operator can be understood as:

> **Take the results produced so far and pass them into the next operation.**

---

## Selecting Investigation-Relevant Fields with `project`

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

The query reduces unnecessary information and keeps fields relevant to the investigation.

---

## Time-Based Filtering with `where`

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
| project
    TimeGenerated,
    OperationNameValue,
    ActivityStatusValue,
    ResourceGroup,
    Caller
```

Useful relative time examples include:

```kusto
ago(1h)
```

```kusto
ago(7d)
```

```kusto
ago(30d)
```

Time filtering is essential because investigations normally require a defined timeframe.

---

## Targeted Resource Group Investigation

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

The `=~` operator performs case-insensitive equality comparison.

This query demonstrates a realistic investigation pattern:

```text
Select Data
    │
    ▼
Define Time Range
    │
    ▼
Filter Relevant Resource
    │
    ▼
Select Important Fields
    │
    ▼
Sort Chronologically
```

---

## Successful Administrative Operations

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
| where ActivityStatusValue =~ "Success"
| project
    TimeGenerated,
    OperationNameValue,
    ActivityStatusValue,
    ResourceGroup,
    Caller,
    CallerIpAddress
| order by TimeGenerated desc
```

---

## Text Searching with `contains`

```kusto
AzureActivity
| where OperationNameValue contains "resource"
| project
    TimeGenerated,
    OperationNameValue,
    ActivityStatusValue,
    ResourceGroup
| order by TimeGenerated desc
```

This type of filtering is useful when an analyst knows part of an expected value but does not want to require exact equality.

---

## Counting Events

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
| count
```

Instead of returning every event, the query returns the total number of matching events.

```text
Individual Logs
      │
      ▼
Aggregation
      │
      ▼
Security Information
```

---

## Behavioral Aggregation with `summarize`

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
| summarize EventCount = count() by ActivityStatusValue
| order by EventCount desc
```

Instead of asking:

> **What does every individual event say?**

The query asks:

> **How many events occurred for each activity status?**

This converts individual events into a higher-level behavioral summary.

---

## Top Azure Operations

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
| summarize EventCount = count() by OperationNameValue
| top 10 by EventCount desc
```

This answers:

> **Which Azure operations appeared most frequently during the selected period?**

This is a basic form of behavioral analysis.

The same concept becomes useful in more advanced security investigations involving:

- Authentication activity
- Process execution
- Network connections
- DNS requests
- Cloud administrative operations
- File access
- Data transfer
- Endpoint detections

---

## Time-Based Aggregation with `bin()`

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
| summarize EventCount = count() by bin(TimeGenerated, 1h)
| order by TimeGenerated asc
```

The expression:

```kusto
bin(TimeGenerated, 1h)
```

places timestamps into one-hour groups.

Time-based aggregation becomes especially valuable when investigating:

- Authentication spikes
- Password spraying
- Port scanning
- Repeated malware execution
- Command-and-control beaconing
- Data-transfer spikes
- Cloud administration bursts
- Repeated failed operations

---

## KQL Time Visualization

```kusto
AzureActivity
| where TimeGenerated >= ago(24h)
| summarize EventCount = count() by bin(TimeGenerated, 1h)
| order by TimeGenerated asc
| render timechart
```

Visualization can help analysts identify patterns that are difficult to recognize while reading individual rows.

```text
Raw Logs
    │
    ▼
Aggregation
    │
    ▼
Time Buckets
    │
    ▼
Visualization
    │
    ▼
Pattern Recognition
```

---

## Failed Azure Operations

```kusto
AzureActivity
| where TimeGenerated >= ago(7d)
| where ActivityStatusValue =~ "Failed"
| project
    TimeGenerated,
    OperationNameValue,
    ResourceGroup,
    Caller,
    CallerIpAddress
| order by TimeGenerated desc
```

A query returning zero events is still meaningful.

The goal of security analysis is not to force interesting results.

The query answers a defined investigative question:

> **Were failed Azure administrative operations recorded during this period?**

---

# Manual Hunting vs. Automated Detection

## Manual Hunting

```text
Analyst
   │
   ▼
Writes / Runs KQL
   │
   ▼
Reviews Results
   │
   ▼
Interprets Activity
   │
   ▼
Determines Significance
```

The analyst decides when to run the query.

## Automated Detection

```text
Microsoft Sentinel
       │
       ▼
Runs Detection Logic
       │
       ▼
Evaluates Telemetry
       │
       ▼
Condition Matches
       │
       ▼
Alert Generated
       │
       ▼
Possible Incident
       │
       ▼
Analyst Investigation
```

The underlying logic may still depend heavily on KQL, but Sentinel executes the detection automatically according to the configured rule.

This creates a direct relationship between threat hunting and detection engineering.

```text
Question
   │
   ▼
KQL Query
   │
   ▼
Useful Detection Logic
   │
   ▼
Scheduled Analytics Rule
   │
   ▼
Alert
```

---

# Microsoft Sentinel Analytics Rules

The Sentinel **Analytics** section was reviewed to understand how detection logic is operationalized.

An analytics rule can define areas such as:

- Rule name
- Rule description
- Severity
- Required data sources
- Rule type
- KQL detection query
- Query frequency
- Lookback period
- Alert threshold
- Entity mappings
- Incident settings
- MITRE ATT&CK tactics
- MITRE ATT&CK techniques
- Automated-response configuration

A scheduled analytics rule can conceptually operate as:

```text
Telemetry
    │
    ▼
Scheduled Query
    │
    ▼
Detection Condition
    │
    ├── Not Met ──► Continue Monitoring
    │
    └── Met
         │
         ▼
       Alert
         │
         ▼
       Incident
```

---

# Suspicious Resource Deployment Analytics Rule

An Azure Activity-related analytics-rule template named:

```text
Suspicious Resource deployment
```

was reviewed during the lab.

The rule details showed:

```text
Rule Name: Suspicious Resource deployment
Severity: Low
Rule Type: Scheduled
Data Source: Azure Activity
MITRE ATT&CK Tactic: Impact
MITRE ATT&CK Technique: T1496 — Resource Hijacking
```

The rule provided an example of how cloud activity can be evaluated using detection logic instead of requiring an analyst to manually inspect every administrative event.

The important lesson was not simply that the template existed.

The investigation required understanding:

```text
What data source does the rule require?
        │
        ▼
What activity does the query examine?
        │
        ▼
What behavior is considered suspicious?
        │
        ▼
What severity is assigned?
        │
        ▼
What ATT&CK behavior is associated?
        │
        ▼
What happens if the detection condition matches?
```

---

# MITRE ATT&CK Mapping

The reviewed detection was mapped to:

```text
Tactic:
Impact

Technique:
T1496 — Resource Hijacking
```

The relationship can be represented as:

```text
Raw Telemetry
      │
      ▼
Detection Query
      │
      ▼
Suspicious Behavior
      │
      ▼
MITRE ATT&CK
      │
      ▼
Tactic / Technique Context
```

MITRE ATT&CK mapping does **not** automatically prove that an attacker performed the mapped behavior.

Instead, it provides standardized language for describing the adversary behavior that the detection is intended to identify.

> **ATT&CK Mapping ≠ Proof of Compromise**

An analyst still needs evidence and context.

---

# Sentinel Alerts and Incidents

Lab 03 reinforced the distinction between detections, alerts, and incidents.

```text
Telemetry
   │
   ▼
Analytics Rule
   │
   ▼
Detection Condition
   │
   ▼
Alert
   │
   ▼
Incident
   │
   ▼
Investigation
```

## Alert

An alert means:

> **A detection condition matched.**

## Incident

An incident represents the larger investigation case.

An incident may contain:

```text
Incident
├── Alert 1
├── Alert 2
├── User / Account Entities
├── Devices
├── IP Addresses
├── Resources
├── Evidence
├── Timeline
├── Comments
├── Owner
└── Status
```

This gives the analyst a case-management structure around related security information.

---

# Sentinel Workbooks

Workbooks were reviewed conceptually as a way of turning telemetry and KQL results into visual security information.

```text
Raw Telemetry
      │
      ▼
     KQL
      │
      ▼
Aggregation
      │
      ▼
Workbook
      │
      ├── Charts
      ├── Tables
      ├── Counts
      ├── Trends
      └── Dashboards
```

Workbooks can reduce the need to manually inspect every event when the goal is to understand larger trends or operational patterns.

---

# Sentinel Watchlists

Watchlists were reviewed as a method of adding organization-specific reference information to security analytics.

Example:

```text
CriticalAssets
--------------
DC01
SQL01
FILE01
```

Another example:

```text
ApprovedAdminIPs
----------------
10.0.0.10
10.0.0.20
```

This allows generic telemetry to be evaluated against environmental context.

For example:

```text
Successful Administrative Login
              +
Source IP Not in ApprovedAdminIPs
              =
Higher Investigation Interest
```

Watchlists can therefore help transform generic security events into more meaningful organization-specific detections and investigations.

---

# Lab 03 Investigation Methodology

The methodology practiced throughout Lab 03 can be summarized as:

```text
1. Identify Data Source
        ↓
2. Configure Data Connector
        ↓
3. Generate Controlled Activity
        ↓
4. Verify Source Event
        ↓
5. Validate Data Ingestion
        ↓
6. Identify Destination Table
        ↓
7. Inspect Schema
        ↓
8. Run Basic KQL
        ↓
9. Filter Relevant Activity
        ↓
10. Select Important Fields
        ↓
11. Sort Chronologically
        ↓
12. Aggregate Events
        ↓
13. Identify Patterns
        ↓
14. Analyze Activity Over Time
        ↓
15. Review Detection Logic
        ↓
16. Review MITRE ATT&CK Mapping
        ↓
17. Understand Alert Generation
        ↓
18. Understand Incident Creation
        ↓
19. Document Findings
```

---

# Lab 03 Findings

The lab established that:

- The existing Log Analytics workspace could support Sentinel security telemetry.
- Microsoft Sentinel was available as the SIEM layer over the workspace.
- Azure Activity was configured as a Sentinel data source.
- Azure administrative operations generated observable source telemetry.
- The Azure Activity connector provided a path for that telemetry into the Log Analytics environment.
- Azure Activity events became queryable through the `AzureActivity` table.
- The `AzureActivity` schema exposed fields useful for security analysis.
- KQL could retrieve raw Azure administrative activity.
- `where` could restrict events according to time and other conditions.
- `project` could reduce unnecessary fields and improve investigation readability.
- `order by` could reconstruct activity chronologically.
- `contains` could perform text-based filtering.
- `count` could aggregate matching events.
- `summarize` could convert individual logs into behavioral information.
- `top` could identify frequently occurring operations.
- `bin()` could group activity into time intervals.
- Time-based visualization could help identify activity patterns.
- Failed administrative activity could be specifically hunted with KQL.
- Manual hunting and automated analytics rules serve different operational purposes.
- Sentinel analytics rules can repeatedly evaluate telemetry using detection logic.
- Analytics rules depend on appropriate data sources.
- Detection rules can be associated with MITRE ATT&CK tactics and techniques.
- The reviewed **Suspicious Resource deployment** rule was mapped to **Impact / T1496 — Resource Hijacking**.

---

# Core Security Operations Lessons

The most important outcome of the week was understanding that effective Security Operations is not based on memorizing a product interface.

It depends on understanding the systems underneath the interface:

- Where telemetry originates
- How telemetry is collected
- Where telemetry is stored
- How security data is structured
- How detections are created
- Why an alert was generated
- What evidence supports the alert
- Which users, devices, resources, and processes are involved
- How activity developed over time
- How multiple signals relate to one another
- How KQL can validate and extend an investigation
- How hunting logic can become automated detection logic
- How MITRE ATT&CK provides behavioral context
- How analysts determine scope and intent
- When response actions are justified
- How findings should be documented

```text
BUILD THE ENVIRONMENT
        │
        ▼
VALIDATE THE TELEMETRY
        │
        ▼
INVESTIGATE THE ENDPOINT
        │
        ▼
UNDERSTAND XDR CORRELATION
        │
        ▼
INVESTIGATE DEVICES & USERS
        │
        ▼
QUERY CLOUD SIEM TELEMETRY
        │
        ▼
IDENTIFY BEHAVIORAL PATTERNS
        │
        ▼
UNDERSTAND DETECTION LOGIC
        │
        ▼
MAP BEHAVIOR TO MITRE ATT&CK
        │
        ▼
MAKE A DEFENSIBLE SECURITY DECISION
```

---

# Final Technical Outcome

This Week 17 project demonstrates practical exposure to the complete security-monitoring lifecycle:

> **Telemetry Generation → Collection → Ingestion → Storage → Querying → Detection → Correlation → Investigation → Validation → Classification → Response → Documentation**

The three labs demonstrate a progression from building the underlying Microsoft security environment, to investigating endpoint activity through Microsoft Defender XDR, to analyzing cloud telemetry and detection logic through Microsoft Sentinel and KQL.

The key lesson is that the tools themselves are only one part of Security Operations.

A strong analyst must understand:

```text
DATA
  │
  ▼
TELEMETRY
  │
  ▼
DETECTION
  │
  ▼
EVIDENCE
  │
  ▼
CONTEXT
  │
  ▼
CORRELATION
  │
  ▼
INVESTIGATION
  │
  ▼
VALIDATION
  │
  ▼
DECISION
  │
  ▼
RESPONSE
```

That end-to-end understanding is the primary technical outcome of the **Microsoft Defender XDR and Microsoft Sentinel Security Operations** labs.
