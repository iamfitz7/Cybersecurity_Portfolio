# Week 17 — Microsoft Defender XDR, Microsoft Sentinel, Endpoint, Identity, Cloud Security & Detection Engineering

> **Focus:** Microsoft Defender XDR • Microsoft Defender for Endpoint • Microsoft Sentinel • Microsoft Entra ID • Azure Log Analytics • Azure RBAC • Network Security Groups • Kusto Query Language (KQL) • EDR • XDR • SIEM • Endpoint Security • Identity Security • Cloud Security • Threat Hunting • Detection Engineering • Security Automation • Incident Investigation • Incident Response • PowerShell Investigation • Process Analysis • File Analysis • Network Analysis • Security Engineering • Least Privilege • MITRE ATT&CK

---

# Executive Summary

Week 17 focused on building, operating, testing, and investigating a Microsoft security environment across endpoint, identity, cloud, SIEM, detection, and automation technologies.

The goal was not simply to learn where features are located inside Microsoft security products.

The goal was to understand how security data moves through an environment and how that data can be used to make evidence-based security decisions.

Throughout the week, I worked with:

- Microsoft Azure
- Microsoft Entra ID
- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Microsoft Sentinel
- Azure Log Analytics
- Azure Activity Logs
- Microsoft Entra sign-in logs
- Azure Role-Based Access Control (RBAC)
- Azure Network Security Groups
- Kusto Query Language (KQL)
- Advanced Hunting
- Endpoint telemetry
- Identity telemetry
- Cloud administrative telemetry
- Process telemetry
- File telemetry
- Network telemetry
- Registry telemetry
- Analytics rules
- Alert generation
- Incident investigation
- Detection engineering
- MITRE ATT&CK mapping
- Least-privilege remediation
- Security automation

The week consisted of six connected hands-on labs.

| Lab | Focus | Major Areas |
| --- | --- | --- |
| **Lab 01** | Microsoft Defender XDR & Defender for Endpoint Foundation | Endpoint onboarding, Defender validation, alerts, incidents, devices, users, response capabilities |
| **Lab 02** | Microsoft Sentinel & Azure Log Analytics | SIEM deployment, data ingestion, Azure Activity, KQL, analytics rules, MITRE ATT&CK |
| **Lab 03** | Microsoft Entra Identity Investigation | Sign-in telemetry, failed authentication, source IP analysis, authentication timelines |
| **Lab 04** | Custom Detection Engineering | KQL detection development, scheduled analytics rules, troubleshooting, alert and incident validation |
| **Lab 05** | Suspicious PowerShell Endpoint Investigation & Response | Advanced Hunting, encoded PowerShell, process ancestry, file/network telemetry, incident validation, containment assessment |
| **Lab 06** | Azure RBAC Security Monitoring, Detection & Automated Triage | Azure RBAC, Azure Activity, privileged-access monitoring, KQL hunting, least privilege, Sentinel analytics, automation |

Together, these labs created a progression from building the security environment to investigating endpoint and identity behavior, engineering detections, monitoring privileged Azure activity, applying least privilege, and introducing security automation.

---

# Week 17 Security Progression

The labs were designed around increasingly deeper security questions.

## Lab 01

> **Can I build and validate a working Microsoft endpoint security environment?**

## Lab 02

> **Can I collect cloud security telemetry, query it, and understand how Sentinel converts telemetry into detections?**

## Lab 03

> **Can I investigate Microsoft Entra authentication activity and determine what actually happened to an identity?**

## Lab 04

> **Can I convert manually validated KQL hunting logic into a working scheduled detection and troubleshoot it when it fails?**

## Lab 05

> **Can I reconstruct suspicious PowerShell activity across process, file, network, user, timeline, and incident evidence and determine whether endpoint containment is justified?**

## Lab 06

> **Can I monitor Azure RBAC permission changes, detect successful role assignments, investigate the identity and cloud evidence, remove unnecessary access, and automate part of the alert-triage workflow?**

The overall progression was:

```text
BUILD SECURITY ENVIRONMENT
        ↓
VALIDATE TELEMETRY
        ↓
QUERY DATA
        ↓
HUNT FOR BEHAVIOR
        ↓
BUILD DETECTIONS
        ↓
GENERATE ALERTS
        ↓
INVESTIGATE INCIDENTS
        ↓
CORRELATE EVIDENCE
        ↓
ASSESS RISK
        ↓
MAKE RESPONSE DECISIONS
        ↓
MONITOR PRIVILEGED ACCESS
        ↓
REMEDIATE EXCESSIVE PERMISSIONS
        ↓
VERIFY LEAST PRIVILEGE
        ↓
AUTOMATE APPROPRIATE ACTIONS
```

---

# Week 17 Objectives

The main objectives of Week 17 were to:

- Build a working Microsoft security lab
- Configure Microsoft Defender for Endpoint
- Validate endpoint onboarding
- Understand Defender XDR alerts and incidents
- Investigate devices and users
- Work with endpoint response capabilities
- Configure Microsoft Sentinel
- Work with Azure Log Analytics
- Connect Azure security telemetry
- Validate Azure Activity ingestion
- Configure Microsoft Entra sign-in telemetry
- Investigate authentication activity
- Use KQL for security investigations
- Create scheduled analytics rules
- Generate controlled security activity
- Validate detection logic
- Troubleshoot false negatives
- Map detections to MITRE ATT&CK
- Investigate suspicious PowerShell
- Reconstruct parent-child process relationships
- Investigate file creation
- Investigate network connections
- Review Registry activity
- Analyze Device Timeline
- Validate incident timestamps
- Evaluate endpoint isolation
- Make evidence-based containment decisions
- Work with Azure RBAC
- Compare Reader and Contributor permissions
- Monitor Azure role assignments
- Detect successful RBAC write activity
- Investigate privileged cloud changes
- Remove unnecessary permissions
- Apply least privilege
- Verify final access
- Create Sentinel automation rules
- Scope automation to specific detections
- Automate appropriate alert handling

---

# Technologies Used

| Technology | Purpose |
| --- | --- |
| **Microsoft Azure** | Cloud infrastructure and security lab environment |
| **Microsoft Entra ID** | Identity, authentication, users, groups, and sign-in telemetry |
| **Microsoft Defender XDR** | Cross-domain security operations and incident investigation |
| **Microsoft Defender for Endpoint** | Endpoint detection, telemetry, investigation, and response |
| **Microsoft Sentinel** | SIEM, analytics, detection, incident creation, and automation |
| **Azure Log Analytics** | Security telemetry storage and KQL querying |
| **Azure Activity Logs** | Azure management-plane and administrative telemetry |
| **Azure RBAC** | Resource authorization, role assignments, permission review, and least privilege |
| **Azure Network Security Groups** | Cloud network security configuration |
| **Advanced Hunting** | Endpoint and identity threat hunting |
| **Kusto Query Language** | Security telemetry querying and detection logic |
| **DeviceProcessEvents** | Process execution telemetry |
| **DeviceFileEvents** | File activity telemetry |
| **DeviceNetworkEvents** | Endpoint network telemetry |
| **DeviceRegistryEvents** | Registry activity telemetry |
| **SigninLogs** | Microsoft Entra authentication telemetry |
| **AzureActivity** | Azure administrative activity |
| **Sentinel Analytics Rules** | Scheduled detection logic |
| **Sentinel Automation Rules** | Automated security workflow actions |
| **MITRE ATT&CK** | Standardized security behavior mapping |
| **Windows 11 Enterprise** | Monitored endpoint |
| **PowerShell** | Controlled security activity generation and investigation |
| **VirtualBox** | Virtualized lab environment |

---

# Lab 01 — Microsoft Defender XDR & Defender for Endpoint Foundation

## Objective

The first lab focused on building the endpoint security foundation needed for the rest of Week 17.

The main goal was to understand how an endpoint becomes visible inside Microsoft Defender and how security telemetry can then support detection and investigation.

The workflow was:

```text
WINDOWS ENDPOINT
        ↓
DEFENDER FOR ENDPOINT
        ↓
ENDPOINT TELEMETRY
        ↓
MICROSOFT DEFENDER XDR
        ↓
ALERTS
        ↓
INCIDENTS
        ↓
INVESTIGATION
        ↓
RESPONSE
```

---

## Endpoint Environment

I used a Windows 11 Enterprise virtual machine as the monitored endpoint.

Before relying on Defender telemetry, I needed to make sure the endpoint could properly support Defender for Endpoint.

This required checking:

- Windows edition
- Defender components
- Security services
- Endpoint onboarding
- Device visibility
- Sensor health

This reinforced an important security engineering lesson:

> **Security monitoring depends on the underlying system being correctly configured.**

If the endpoint sensor is not working, later detections and investigations may be incomplete.

---

## Defender for Endpoint Onboarding

I worked through the endpoint onboarding process and validated that the Windows system appeared in Microsoft Defender.

The process required more than simply running an onboarding command.

I also reviewed service behavior and Windows security capabilities when the device did not immediately behave as expected.

The troubleshooting process followed:

```text
CHECK WINDOWS EDITION
        ↓
CHECK DEFENDER CAPABILITIES
        ↓
CHECK REQUIRED SERVICES
        ↓
RUN ONBOARDING
        ↓
VERIFY DEVICE
        ↓
VERIFY TELEMETRY
```

This gave me practical experience troubleshooting the security platform itself rather than assuming every problem was caused by an attacker.

---

## Defender XDR Investigation Model

Once the endpoint was available, I reviewed how Microsoft Defender organizes security information.

Important concepts included:

```text
ALERT
        ↓
INCIDENT
        ↓
DEVICE
        ↓
USER
        ↓
EVIDENCE
        ↓
TIMELINE
        ↓
RESPONSE ACTION
```

An alert represents a security detection.

An incident can group related alerts and evidence into a broader investigation.

This distinction became important throughout the remaining labs.

---

## Alerts Are Starting Points

One of the major lessons from Lab 01 was:

```text
ALERT
  ≠
FINAL CONCLUSION
```

An alert tells the analyst that something deserves investigation.

It does not automatically prove:

- Malware
- Account compromise
- Persistence
- Lateral movement
- Data theft
- Command-and-control
- Malicious intent

The analyst still needs to investigate the evidence.

---

## Endpoint Response Capabilities

I also reviewed Defender endpoint response capabilities such as:

- Device isolation
- Antivirus scanning
- Live Response
- Investigation package collection
- Automated investigation
- Device Timeline
- Security recommendations

The main lesson was that response capabilities should not be used automatically just because they are available.

Response should match the evidence and risk.

---

# Lab 01 Key Lessons

- Endpoint telemetry depends on correct onboarding.
- Security tooling can fail because of configuration problems.
- Device health should be validated before trusting missing telemetry.
- Alerts are investigation starting points.
- Incidents provide broader context than individual alerts.
- Devices and users are important investigation pivots.
- Response actions should be evidence-based.

---

# Lab 02 — Microsoft Sentinel & Azure Log Analytics

## Objective

Lab 02 expanded the environment from endpoint security into cloud SIEM monitoring.

The main goal was to understand how Microsoft Sentinel receives security telemetry and how KQL can be used to investigate it.

The basic architecture was:

```text
AZURE / SECURITY DATA
        ↓
LOG ANALYTICS
        ↓
MICROSOFT SENTINEL
        ↓
KQL
        ↓
ANALYTICS RULES
        ↓
ALERTS / INCIDENTS
```

---

## Azure Log Analytics

Azure Log Analytics provided the workspace where security telemetry could be stored and queried.

This introduced one of the most important Week 17 concepts:

```text
NO DATA
   ↓
NO QUERY
   ↓
NO DETECTION
   ↓
NO ALERT
   ↓
NO INVESTIGATION
```

Before creating detections, I first needed to verify that the required data actually existed.

---

## Azure Activity

I worked with the:

```text
AzureActivity
```

table.

Azure Activity provides management-plane telemetry involving Azure resources.

This can include:

- Resource creation
- Resource deletion
- Resource modification
- Administrative operations
- Security configuration changes
- Role assignments
- Network configuration

A basic investigation could begin with:

```kusto
AzureActivity
| take 20
```

From there, I could narrow the data using operators such as:

```kusto
where
project
summarize
order by
```

---

## KQL Fundamentals

Week 17 introduced and reinforced several KQL concepts.

### Filtering

```kusto
AzureActivity
| where TimeGenerated > ago(24h)
```

### Selecting Fields

```kusto
AzureActivity
| project TimeGenerated, Caller, OperationNameValue
```

### Counting Events

```kusto
AzureActivity
| summarize count()
```

### Grouping

```kusto
AzureActivity
| summarize count() by Caller
```

### Sorting

```kusto
AzureActivity
| order by TimeGenerated desc
```

The important skill was not memorizing commands.

It was learning how to convert a security question into a query.

---

# Lab 02 Detection Engineering Introduction

I also reviewed how KQL can become automated Sentinel detection logic.

```text
SECURITY QUESTION
        ↓
KQL HUNT
        ↓
VALIDATE RESULTS
        ↓
ANALYTICS RULE
        ↓
ALERT
        ↓
INCIDENT
```

This became the foundation for later custom detections.

---

# MITRE ATT&CK

Sentinel analytics rules can be mapped to MITRE ATT&CK behavior.

This provides standardized security context.

However:

```text
MITRE MAPPING
      ≠
PROOF OF ATTACK
```

MITRE ATT&CK helps describe behavior.

The investigation still determines what actually happened.

---

# Lab 02 Key Lessons

- Telemetry must exist before detection.
- Log Analytics provides the data foundation for Sentinel.
- KQL allows analysts to turn raw events into investigation evidence.
- Sentinel analytics rules automate detection logic.
- MITRE ATT&CK provides standardized behavioral context.
- A successful query is not automatically a good detection.

---

# Lab 03 — Microsoft Entra Identity Investigation

## Objective

Lab 03 focused on identity security.

The goal was to investigate Microsoft Entra authentication activity and understand how failed sign-ins appear in security telemetry.

The workflow was:

```text
USER AUTHENTICATION
        ↓
MICROSOFT ENTRA ID
        ↓
SIGN-IN LOGS
        ↓
LOG ANALYTICS
        ↓
KQL
        ↓
IDENTITY INVESTIGATION
```

---

# Sign-In Telemetry

The main table used was:

```text
SigninLogs
```

Important fields included:

- `TimeGenerated`
- `UserPrincipalName`
- `IPAddress`
- `AppDisplayName`
- `ResultType`
- `ResultDescription`
- `AuthenticationRequirement`
- `ConditionalAccessStatus`

These fields helped answer:

```text
WHO attempted authentication?
        ↓
WHEN did it happen?
        ↓
WHERE did it come from?
        ↓
WHICH application was targeted?
        ↓
DID it succeed?
        ↓
WHY did it fail?
```

---

# Missing SigninLogs Troubleshooting

An important part of the lab occurred when expected Entra sign-in data was not initially available in Log Analytics.

Instead of assuming the query was wrong, I worked through the telemetry pipeline.

```text
MICROSOFT ENTRA ID
        ↓
DIAGNOSTIC SETTINGS
        ↓
LOG ANALYTICS WORKSPACE
        ↓
SIGNINLOGS TABLE
        ↓
KQL
```

This reinforced a broader troubleshooting method:

```text
SOURCE
   ↓
CONNECTOR / SENSOR
   ↓
WORKSPACE
   ↓
TABLE
   ↓
QUERY
   ↓
DETECTION
```

---

# Failed Authentication Investigation

I generated controlled failed authentication activity and investigated it using KQL.

A query could begin with:

```kusto
SigninLogs
| where TimeGenerated > ago(24h)
| where ResultType != 0
```

I then reviewed:

- User
- Source IP
- Failure volume
- Failure timing
- Failure reason
- Application
- Successful authentication afterward

---

# Why Success After Failure Matters

Repeated failed authentication followed by a successful sign-in can deserve additional attention.

Conceptually:

```text
FAIL
   ↓
FAIL
   ↓
FAIL
   ↓
SUCCESS
```

This does not automatically prove account compromise.

However, it can increase the importance of the investigation.

---

# Lab 03 Key Lessons

- Identity telemetry is critical to modern security investigations.
- Failed authentication needs context.
- Source IP addresses can support investigation.
- Authentication timelines matter.
- Failure reasons matter.
- Success after repeated failure can increase risk.
- Missing telemetry should be troubleshot from the source forward.

---

# Lab 04 — Custom Detection Engineering

## Objective

Lab 04 focused on turning manually investigated authentication behavior into a repeatable Sentinel detection.

This lab was especially important because the first version of the detection did not behave as expected.

That failure became part of the learning process.

---

# Initial Detection Goal

The goal was to detect repeated failed authentication attempts against a Microsoft Entra identity.

The basic concept was:

```text
MULTIPLE FAILED LOGINS
        ↓
SAME USER
        ↓
SHORT TIME PERIOD
        ↓
DETECTION
```

I first verified that the underlying events existed.

Then I created the detection logic.

---

# Detection Logic Problem

The first version grouped authentication events using fixed five-minute time buckets.

Conceptually:

```kusto
bin(TimeGenerated, 5m)
```

This appeared reasonable.

However, the controlled failed logins occurred near the boundary between two fixed time buckets.

Instead of:

```text
Bucket A:
Failure 1
Failure 2
Failure 3
```

the events became:

```text
Bucket A:
Failure 1

Bucket B:
Failure 2
Failure 3
```

The threshold was never reached.

The query executed successfully.

But the detection logic did not represent the intended behavior.

---

# Detection Engineering Lesson

This created one of the most important lessons of Week 17:

> **A query that executes successfully is not automatically a good detection.**

Detection engineering requires more than valid syntax.

The logic must correctly represent the behavior being detected.

---

# Troubleshooting Process

I worked backward through the detection pipeline.

```text
NO ALERT
   ↓
CHECK ANALYTICS RULE
   ↓
CHECK QUERY
   ↓
CHECK QUERY RESULTS
   ↓
CHECK RAW SIGNINLOGS
   ↓
CONFIRM EVENTS EXIST
   ↓
IDENTIFY GROUPING PROBLEM
   ↓
CORRECT LOGIC
   ↓
RETEST
```

After correcting the query, I generated new controlled activity.

The updated rule successfully produced the expected security detection.

---

# Alert and Incident Validation

After the corrected analytics rule ran, I validated:

- Alert creation
- Incident creation
- User context
- Source IP
- Authentication evidence
- Detection timestamps
- Rule logic

The activity was real, but it was generated intentionally for the lab.

Final disposition:

```text
DETECTION:
VALID

ACTIVITY:
REAL

MALICIOUS:
NO

CONTEXT:
AUTHORIZED TESTING
```

---

# Lab 04 Key Lessons

- Detection logic must be behaviorally tested.
- Raw telemetry is essential when troubleshooting detections.
- Fixed time buckets can create false negatives.
- A successful KQL query can still contain flawed detection logic.
- Alerts should be validated against source events.
- A valid detection can still represent expected activity.

---

# Lab 05 — Suspicious PowerShell Endpoint Investigation & Response

## Objective

Lab 05 focused on deeper endpoint investigation.

The goal was to generate controlled PowerShell activity and reconstruct what happened using Microsoft Defender telemetry.

The activity included:

- Encoded PowerShell
- Child-process creation
- Command execution
- File creation
- HTTPS network activity

The investigation asked:

> **What did PowerShell actually do?**

---

# Controlled PowerShell Activity

The activity intentionally generated security-relevant behavior.

This included an encoded PowerShell command.

Encoded PowerShell deserves attention because attackers may use encoding to make commands harder to quickly interpret.

However:

```text
ENCODED POWERSHELL
        ≠
MALWARE
```

The investigation still needs to determine what the command actually did.

---

# DeviceProcessEvents Investigation

I used:

```text
DeviceProcessEvents
```

to investigate process execution.

The evidence showed a process relationship involving:

```text
powershell.exe
      ↓
cmd.exe
      ↓
whoami.exe
```

This was more useful than looking at `whoami.exe` alone.

---

# Why Process Ancestry Matters

A process name without context may tell only a small part of the story.

Compare:

```text
whoami.exe
```

with:

```text
powershell.exe
      ↓
cmd.exe
      ↓
whoami.exe
```

The second view provides execution context.

Process ancestry can help determine:

- What launched a process
- What the process launched
- Whether the chain makes sense
- Whether scripting was involved
- Whether activity matches known attack behavior

---

# Command-Line Analysis

Full command-line information was important during the investigation.

A process name may appear normal while the command-line arguments reveal suspicious behavior.

I reviewed:

- Executable
- Command line
- Encoded arguments
- Parent process
- Child process
- Process IDs
- Timestamps

This reinforced:

> **Process names provide identity. Command lines provide behavior.**

---

# DeviceFileEvents Investigation

I used:

```text
DeviceFileEvents
```

to investigate file activity.

The controlled PowerShell activity created:

```text
week17-example.html
```

File telemetry helped answer:

```text
DID THE COMMAND CHANGE THE FILESYSTEM?
        ↓
WHAT FILE?
        ↓
WHEN?
        ↓
WHICH PROCESS WAS INVOLVED?
```

---

# DeviceNetworkEvents Investigation

I used:

```text
DeviceNetworkEvents
```

to determine whether PowerShell actually created network activity.

The evidence showed communication with:

```text
example.com
```

and the remote IP:

```text
104.20.23.154
```

using:

```text
TCP 443
```

The telemetry showed:

```text
ConnectionSuccess
```

This distinction was important.

A URL appearing inside a command represents intent.

A successful `DeviceNetworkEvents` record represents observed network behavior.

```text
COMMAND CONTAINS URL
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

# Registry Investigation

I also reviewed Registry telemetry.

Registry activity can help identify:

- Persistence
- Run keys
- Configuration changes
- Application behavior
- Security-setting changes

Even when no malicious persistence is found, checking the Registry can provide useful negative evidence.

---

# Device Timeline

Device Timeline provided chronological endpoint context.

This allowed events to be reviewed around the investigation period.

Timeline analysis helps answer:

```text
WHAT HAPPENED FIRST?
        ↓
WHAT HAPPENED NEXT?
        ↓
WHAT ELSE OCCURRED NEARBY?
```

---

# Existing Incident Validation

During the investigation, existing Defender incidents appeared to have names that could potentially relate to the PowerShell activity.

However, timestamp validation showed that those incidents occurred weeks before the controlled Lab 05 activity.

I therefore excluded them.

This was an important lesson:

> **Similar names do not prove that events belong to the same investigation.**

Evidence must match the timeline.

---

# Endpoint Isolation Assessment

Because the activity included:

- Encoded PowerShell
- CMD
- WHOAMI
- File creation
- Outbound network activity

endpoint isolation had to be considered.

However, the investigation found no evidence of:

- Malicious payload execution
- Persistence
- Credential theft
- Lateral movement
- Command-and-control
- Data exfiltration
- Continued unauthorized access

The activity was known controlled testing.

Therefore:

```text
DEVICE ISOLATION
        ↓
NOT JUSTIFIED
```

This reinforced that containment decisions should be based on evidence and risk.

---

# Lab 05 Final Process Reconstruction

The investigation established:

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

# Lab 05 Final Classification

```text
Activity:
AUTHORIZED SECURITY TESTING

Malware:
NOT IDENTIFIED

Persistence:
NOT IDENTIFIED

Credential Theft:
NOT IDENTIFIED

Lateral Movement:
NOT IDENTIFIED

Command-and-Control:
NOT IDENTIFIED

Exfiltration:
NOT IDENTIFIED

Device Isolation:
NOT REQUIRED
```

---

# Lab 05 Major Lessons

## PowerShell Is Not Automatically Malicious

```text
powershell.exe
      ≠
Malware
```

PowerShell is a legitimate administrative tool.

Its behavior determines whether it deserves escalation.

---

## Encoded PowerShell Is a Signal

```text
-EncodedCommand
      ≠
Confirmed Attack
```

Encoding increases investigation interest but does not prove malicious intent.

---

## Process Ancestry Matters

```text
powershell.exe
      ↓
cmd.exe
      ↓
whoami.exe
```

provides much more context than a single process name.

---

## Negative Evidence Matters

The absence of:

- Persistence
- Credential theft
- Lateral movement
- C2
- Exfiltration

helped support the final risk decision.

---

# Lab 06 — Azure RBAC Security Monitoring, Detection & Automated Triage

## Objective

Lab 06 expanded Week 17 into cloud identity, access control, privileged activity monitoring, detection engineering, least-privilege remediation, and security automation.

The central question was:

> **Can I generate a controlled Azure permission change, prove it through telemetry, detect it with KQL, investigate the identity and cloud context, remove unnecessary access, and automate part of the response workflow?**

This lab connected:

```text
AZURE RBAC
   +
AZURE ACTIVITY
   +
MICROSOFT ENTRA ID
   +
KQL
   +
MICROSOFT SENTINEL
   +
DETECTION ENGINEERING
   +
LEAST PRIVILEGE
   +
SECURITY AUTOMATION
```

---

# Lab 06 Environment

The main resources used during the lab were:

```text
Resource Group:
rg-week17-lab6-security

Network Security Group:
nsg-week17-lab6

Test Identity:
Lab Cloud User

Analytics Rule:
Lab - Azure RBAC Role Assignment Activity

Automation Rule:
Lab - RBAC Incident Triage Automation
```

The environment was intentionally controlled so administrative activity could be safely generated, collected, investigated, detected, remediated, and verified.

---

# Lab 06 Architecture

```text
MICROSOFT ENTRA ID
        │
        │ Identity / Authentication
        ↓
   LAB CLOUD USER
        │
        │ Azure RBAC
        ↓
AZURE RESOURCE GROUP
rg-week17-lab6-security
        │
        ├── Network Security Group
        │   nsg-week17-lab6
        │
        └── Administrative Activity
                    ↓
             Azure Activity Logs
                    ↓
             Log Analytics
                    ↓
           Microsoft Sentinel
                    ↓
                  KQL
                    ↓
          RBAC Analytics Rule
                    ↓
                 Alert
                    ↓
           Automation Rule
                    ↓
         Investigation / Triage
                    ↓
       Least-Privilege Remediation
```

---

# Azure Resource Group

I used the dedicated resource group:

```text
rg-week17-lab6-security
```

Keeping the lab resources together made it easier to isolate the security exercise and filter Azure Activity events related specifically to the lab.

This also created a controlled scope for RBAC testing.

---

# Azure Network Security Group

I created:

```text
nsg-week17-lab6
```

The Network Security Group provided a cloud security resource where controlled administrative changes could be made.

A controlled inbound security rule was also used during the exercise.

Example:

```text
Name:
Deny-Lab-SSH

Service:
SSH

Destination Port:
22

Protocol:
TCP

Action:
Deny
```

The purpose was not simply to create an NSG rule.

The larger security lesson was:

```text
CLOUD CONFIGURATION CHANGE
        ↓
AZURE RECORDS ACTIVITY
        ↓
TELEMETRY BECOMES SEARCHABLE
        ↓
SECURITY TEAM CAN INVESTIGATE
```

Cloud configuration changes are security evidence.

---

# Microsoft Entra Test Identity

I used a controlled identity named:

```text
Lab Cloud User
```

The identity allowed me to test access controls and generate activity without using the main administrative identity for every action.

This made it possible to investigate the relationship between:

```text
IDENTITY
   +
AUTHENTICATION
   +
AUTHORIZATION
   +
RESOURCE ACTIVITY
```

---

# Authentication vs. Authorization

One of the important concepts reinforced by this lab was the difference between authentication and authorization.

```text
AUTHENTICATION

Who are you?
Did you successfully sign in?
```

versus:

```text
AUTHORIZATION

What are you allowed to do?
Which Azure resources can you access?
Which actions can you perform?
```

A user can authenticate successfully while still having limited permissions.

Both areas need to be investigated during cloud security incidents.

---

# Microsoft Entra Security Group

I also worked with a Microsoft Entra security group for controlled access organization.

The test user was added as a member.

This provided practice working with identity grouping rather than thinking about cloud access only as individual user accounts.

In larger environments, groups can make permission management more consistent and scalable.

---

# Azure RBAC

Azure Role-Based Access Control determines what identities are allowed to do with Azure resources.

Conceptually:

```text
IDENTITY
    +
ROLE
    +
SCOPE
    =
AZURE ACCESS
```

This means an investigation should consider all three parts.

For example:

```text
WHO:
Lab Cloud User

ROLE:
Reader / Contributor

SCOPE:
rg-week17-lab6-security
```

---

# Reader Role Baseline

The test identity had the:

```text
Reader
```

role at the lab resource-group scope.

Reader provides visibility into resources without providing the same ability to modify them as Contributor.

This created a lower-permission baseline.

```text
Lab Cloud User
      ↓
Reader
      ↓
View Resources
```

This baseline became important when the account was temporarily elevated and later returned to Reader.

---

# Controlled Contributor Assignment

For the controlled security test, the test identity temporarily received:

```text
Contributor
```

Contributor provides significantly greater ability to manage Azure resources.

The assignment was intentional.

However, the behavior itself represents an important security event because similar permission changes could occur during unauthorized activity.

This created realistic RBAC telemetry for investigation.

---

# Why Role Assignments Matter

A compromised identity may initially have limited access.

An attacker may attempt to increase that access.

Conceptually:

```text
COMPROMISED LOW-PRIVILEGE IDENTITY
        ↓
ROLE ASSIGNMENT
        ↓
INCREASED PERMISSIONS
        ↓
GREATER POTENTIAL IMPACT
```

This is why unexpected role assignments deserve monitoring.

Examples of roles that may require close attention include:

- Contributor
- Owner
- User Access Administrator
- Privileged custom roles

Not every assignment is malicious.

But privileged-access changes deserve visibility.

---

# Azure Activity Monitoring

The main cloud telemetry source was:

```text
AzureActivity
```

Azure Activity records management-plane operations involving Azure resources.

This can include:

- Resource creation
- Resource modification
- Resource deletion
- Network changes
- Administrative operations
- Role assignments
- Permission changes

Before creating a detection, I verified that the activity was actually present.

This followed the same security engineering principle used throughout Week 17:

> **Validate the telemetry before trusting the detection layer.**

---

# Resource Group Investigation with KQL

I used KQL to narrow Azure Activity to the lab resource group.

Example:

```kusto
AzureActivity
| where TimeGenerated > ago(24h)
| where ResourceGroup =~ "rg-week17-lab6-security"
| project
    TimeGenerated,
    Caller,
    CallerIpAddress,
    OperationNameValue,
    ActivityStatusValue,
    ResourceGroup
| order by TimeGenerated desc
```

This provided a focused view of activity involving the controlled environment.

---

# Important AzureActivity Fields

| Field | Investigation Value |
| --- | --- |
| `TimeGenerated` | When the activity occurred |
| `Caller` | Identity associated with the action |
| `CallerIpAddress` | Source IP associated with the request |
| `OperationNameValue` | Azure operation that occurred |
| `ActivityStatusValue` | Whether the operation succeeded or failed |
| `ResourceGroup` | Resource group associated with the activity |

Together, these fields can move an investigation from:

```text
Something changed.
```

to:

```text
A specific identity
performed a specific operation
against a specific Azure environment
at a specific time
from a specific source.
```

---

# User-Specific Azure Activity

I also filtered the Azure Activity telemetry around the controlled test identity.

Example:

```kusto
AzureActivity
| where TimeGenerated > ago(24h)
| where ResourceGroup =~ "rg-week17-lab6-security"
| where Caller =~ "<LAB-CLOUD-USER-UPN>"
| project
    TimeGenerated,
    Caller,
    CallerIpAddress,
    OperationNameValue,
    ActivityStatusValue,
    ResourceGroup
| order by TimeGenerated desc
```

The public README uses a placeholder rather than exposing the full lab user principal name.

This query demonstrated how an investigation can pivot from a resource to a specific identity.

---

# Microsoft Entra Sign-In Investigation

I also investigated authentication activity associated with `Lab Cloud User`.

The main table was:

```text
SigninLogs
```

Example:

```kusto
SigninLogs
| where UserPrincipalName =~ "<LAB-CLOUD-USER-UPN>"
| project
    TimeGenerated,
    UserPrincipalName,
    AppDisplayName,
    ResultType,
    ResultDescription,
    AuthenticationRequirement,
    ConditionalAccessStatus
| order by TimeGenerated desc
```

The results provided additional identity context, including activity involving Azure services.

---

# Why Identity Correlation Matters

An Azure administrative event should not always be investigated in isolation.

A stronger investigation can connect:

```text
SIGN-IN ACTIVITY
        +
AZURE RESOURCE ACTIVITY
        +
RBAC PERMISSIONS
```

This helps answer:

- Who authenticated?
- When did authentication occur?
- Which application was used?
- Did authentication succeed?
- What permissions did the identity have?
- What Azure actions followed?
- Were those actions expected?

This creates a much stronger investigation than looking at only one event source.

---

# Hunting Azure RBAC Role Assignments

I then focused specifically on Azure role-assignment activity.

A broad KQL hunt followed this pattern:

```kusto
AzureActivity
| where TimeGenerated > ago(24h)
| where OperationNameValue contains "ROLEASSIGNMENTS"
| project
    TimeGenerated,
    Caller,
    CallerIpAddress,
    OperationNameValue,
    ActivityStatusValue,
    ResourceGroup
| order by TimeGenerated desc
```

The purpose was to first prove that role-assignment operations were visible in the raw telemetry.

---

# RBAC Detection Logic

After validating the source activity, I narrowed the logic to successful role-assignment writes.

The core detection logic was:

```kusto
AzureActivity
| where OperationNameValue contains "ROLEASSIGNMENTS"
| where OperationNameValue contains "WRITE"
| where ActivityStatusValue =~ "Success"
```

Useful investigation fields could then be projected.

Example:

```kusto
AzureActivity
| where OperationNameValue contains "ROLEASSIGNMENTS"
| where OperationNameValue contains "WRITE"
| where ActivityStatusValue =~ "Success"
| project
    TimeGenerated,
    Caller,
    CallerIpAddress,
    OperationNameValue,
    ActivityStatusValue,
    ResourceGroup
```

---

# Why Detect Successful Writes?

The goal was to focus on role-assignment operations that actually succeeded.

A failed attempt may still be interesting.

However, a successful write confirms that a permission-changing operation completed.

Conceptually:

```text
ROLE ASSIGNMENT ATTEMPT
        ↓
SUCCESS
        ↓
ACCESS STATE MAY HAVE CHANGED
        ↓
INVESTIGATION REQUIRED
```

---

# Microsoft Sentinel Analytics Rule

After validating the KQL logic, I converted it into a Microsoft Sentinel analytics rule.

The rule was named:

```text
Lab - Azure RBAC Role Assignment Activity
```

The configuration included:

```text
Severity:
Medium

Status:
Enabled

MITRE ATT&CK Tactic:
Privilege Escalation

Rule Frequency:
Every 5 minutes

Lookback Period:
Last 10 minutes

Trigger:
More than 0 matching results
```

This changed the workflow from manual hunting to repeatable monitoring.

```text
MANUAL QUERY
        ↓
VALIDATED DETECTION LOGIC
        ↓
SENTINEL ANALYTICS RULE
        ↓
REPEATED EVALUATION
        ↓
SECURITY ALERT
```

---

# Why Medium Severity?

Role assignments can be completely legitimate administrative activity.

At the same time, unexpected role assignments can significantly increase an identity's capabilities.

Using Medium severity provided meaningful security attention without automatically treating every permission change as a confirmed critical compromise.

The investigation still determines the final risk.

---

# MITRE ATT&CK Mapping

The rule was associated with:

```text
Privilege Escalation
```

This mapping makes sense because unauthorized permission changes can increase the privileges available to an identity.

However:

```text
PRIVILEGE ESCALATION MAPPING
        ≠
CONFIRMED MALICIOUS PRIVILEGE ESCALATION
```

MITRE ATT&CK describes relevant behavior.

Evidence and context determine whether an actual attack occurred.

---

# Detection vs. Investigation

The controlled role assignment actually occurred.

Therefore:

```text
DID THE RBAC ACTIVITY HAPPEN?
YES
```

But the next question was different:

```text
WAS THE ACTIVITY MALICIOUS?
```

That required context.

Because the role assignment was intentionally generated as part of the controlled lab:

```text
Activity:
REAL

Authorized:
YES

Malicious:
NO
```

This reinforced one of the strongest Week 17 lessons:

> **A correct detection can identify real security-relevant behavior without that behavior being malicious.**

---

# Least-Privilege Remediation

After the controlled test was complete, the `Contributor` role was no longer needed.

I removed Contributor from `Lab Cloud User`.

Before remediation:

```text
Lab Cloud User
      ↓
Contributor
      ↓
Resource Management Capability
```

After remediation:

```text
Lab Cloud User
      ↓
Reader
      ↓
Lower-Permission Access
```

This was not simply cleanup.

It was a security control.

---

# Principle of Least Privilege

Least privilege means that identities should have only the permissions required for their current responsibilities.

The Lab 06 workflow applied this directly:

```text
START WITH LOWER ACCESS
        ↓
TEMPORARILY GRANT REQUIRED ACCESS
        ↓
PERFORM CONTROLLED TASK
        ↓
MONITOR ACTIVITY
        ↓
INVESTIGATE
        ↓
REMOVE ACCESS WHEN NO LONGER NEEDED
        ↓
VERIFY FINAL PERMISSIONS
```

This is stronger than simply knowing the definition of least privilege.

The lab required actually applying it.

---

# Why Contributor Removal Matters

If a user no longer needs Contributor but the role remains assigned, the environment contains unnecessary risk.

If the account is later compromised, the attacker may inherit those permissions.

Therefore:

```text
UNNECESSARY PRIVILEGE
        =
UNNECESSARY ATTACK SURFACE
```

Removing Contributor reduced the possible impact of misuse of the test identity.

---

# Final RBAC Verification

After removing Contributor, I verified that the test identity returned to:

```text
Reader
```

This final verification was important.

A remediation action should not simply be assumed successful.

The final state should be checked.

```text
REMEDIATE
    ↓
VERIFY
    ↓
DOCUMENT
```

---

# Microsoft Sentinel Automation

The final major technical part of Lab 06 introduced security automation.

I created:

```text
Lab - RBAC Incident Triage Automation
```

The automation was connected specifically to the RBAC analytics rule.

---

# Automation Condition

The automation condition was:

```text
Property:
Analytic Rule Name

Operator:
Equals

Value:
Lab - Azure RBAC Role Assignment Activity
```

This meant the automation was scoped to the intended RBAC detection rather than applying broadly to unrelated security alerts.

This is important because automation should be controlled.

---

# Automated Alert Handling

The rule used an alert-update action to support the triage workflow.

Conceptually:

```text
AZURE RBAC CHANGE
        ↓
AZURE ACTIVITY
        ↓
KQL DETECTION
        ↓
SENTINEL ANALYTICS RULE
        ↓
SECURITY ALERT
        ↓
AUTOMATION RULE
        ↓
AUTOMATED ALERT UPDATE
        ↓
INVESTIGATION
```

This introduced basic SOAR concepts into the project.

---

# Detection Before Automation

One of the most important Lab 06 lessons was the order in which security automation should be built.

```text
UNDERSTAND BEHAVIOR
        ↓
VERIFY TELEMETRY
        ↓
BUILD QUERY
        ↓
TEST QUERY
        ↓
CREATE DETECTION
        ↓
VALIDATE DETECTION
        ↓
THEN AUTOMATE
```

Automation can reduce repetitive work.

However, automation built on poor detection logic can also create unnecessary actions at greater speed.

---

# Cloud Security Investigation Model

Lab 06 connected several security layers:

```text
IDENTITY
Who is the user?
        ↓
AUTHENTICATION
Did the user successfully sign in?
        ↓
AUTHORIZATION
What is the user allowed to do?
        ↓
ADMINISTRATIVE ACTIVITY
What did the user actually do?
        ↓
DETECTION
Did the activity match security logic?
        ↓
INVESTIGATION
Was the activity expected?
        ↓
REMEDIATION
Should access be changed?
        ↓
VERIFICATION
Is the final state correct?
        ↓
AUTOMATION
Can appropriate repetitive actions be automated?
```

---

# Lab 06 Investigation Questions

A repeatable Azure RBAC investigation should answer questions such as:

1. What role-assignment operation occurred?
2. When did the activity happen?
3. Which identity performed the operation?
4. Which identity received the role?
5. Which role was assigned?
6. What Azure scope was affected?
7. Did the operation succeed?
8. What source IP was associated with the activity?
9. Did the caller recently authenticate?
10. Which application was used?
11. Was the assignment expected?
12. Was the assigned access actually required?
13. What did the identity do after receiving the role?
14. Were additional roles assigned?
15. Were other identities affected?
16. Is the elevated permission still present?
17. Should the permission be removed?
18. Should the identity be contained?
19. Does the activity require escalation?
20. Can appropriate parts of the workflow be automated?

---

# Lab 06 Incident Classification

The controlled role-assignment activity was intentionally generated.

Final classification:

```text
Activity Occurred:
YES

Detection Relevant:
YES

Authorized:
YES

Malicious:
NO

Privilege Change:
YES

Unnecessary Access Remaining:
NO

Contributor Removed:
YES

Final Role:
READER

Automation Configured:
YES
```

Final disposition:

```text
AUTHORIZED ADMINISTRATIVE SECURITY TESTING
```

---

# Lab 06 Final Results

| Control | Result |
| --- | --- |
| Dedicated Azure resource group | Created |
| Network Security Group | Created |
| Controlled cloud configuration activity | Generated |
| Test identity | Configured |
| Entra security group membership | Configured |
| Reader access | Configured |
| Temporary Contributor access | Configured for testing |
| Azure Activity telemetry | Validated |
| User-specific Azure Activity | Investigated |
| Microsoft Entra sign-in activity | Investigated |
| RBAC hunting query | Validated |
| Successful RBAC write logic | Validated |
| Sentinel RBAC analytics rule | Created |
| Analytics rule | Enabled |
| Severity | Medium |
| MITRE ATT&CK mapping | Privilege Escalation |
| Contributor access | Removed |
| Final test-user role | Reader |
| Least-privilege state | Verified |
| Sentinel automation rule | Created |
| Automation scope | RBAC analytics rule |
| Automated alert handling | Configured |
| Final lab status | Completed |

---

# Lab 06 Major Lessons

## Authentication and Authorization Are Different

```text
AUTHENTICATION
Who are you?
```

does not answer:

```text
AUTHORIZATION
What can you do?
```

Cloud investigations often need both.

---

## Privileged Changes Deserve Visibility

```text
ROLE ASSIGNMENT
        ↓
PERMISSION CHANGE
        ↓
SECURITY-RELEVANT ACTIVITY
```

This does not automatically mean an attack occurred.

It means the activity deserves context.

---

## Least Privilege Requires Action

```text
TEMPORARY ACCESS
        ↓
TASK COMPLETED
        ↓
REMOVE ACCESS
        ↓
VERIFY FINAL STATE
```

Temporary access should remain temporary.

---

## Detection Is Not a Verdict

```text
RBAC ROLE ASSIGNMENT DETECTED
        ≠
MALICIOUS PRIVILEGE ESCALATION CONFIRMED
```

The alert begins the investigation.

It does not finish it.

---

## Automation Should Be Scoped

Automation should be tied to specific, understood security conditions.

The Lab 06 automation was limited to the RBAC analytics rule instead of broadly changing unrelated alerts.

---

## Cloud and Identity Evidence Are Stronger Together

```text
SigninLogs
     +
AzureActivity
     +
RBAC State
     =
STRONGER INVESTIGATION
```

---

# What I Would Do in a Real Unexpected RBAC Incident

If the role assignment were unknown or unauthorized instead of controlled testing, I would investigate:

- Caller identity
- Target identity
- Assigned role
- Assignment scope
- Source IP address
- Recent sign-in activity
- Authentication results
- MFA context
- Conditional Access context
- Other role assignments by the caller
- Other permissions assigned to the target
- Resource changes after privilege increase
- Network-security changes
- New resources
- Credential changes
- Application activity
- Group membership changes
- Active sessions
- Persistence
- Lateral movement
- Sensitive data access
- Possible exfiltration
- Whether elevated access remains active

Potential response actions could include:

- Remove unauthorized role assignment
- Reduce access to least privilege
- Disable the affected identity when justified
- Revoke active sessions
- Reset credentials when justified
- Review MFA configuration
- Review Conditional Access
- Investigate source IP
- Hunt related Azure Activity
- Hunt related sign-in activity
- Review other privileged changes
- Preserve investigation evidence
- Escalate when malicious activity is supported

---

# Cross-Lab Detection Engineering Lessons

Week 17 developed a repeatable detection engineering process.

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
        ↓
17. Remediate
        ↓
18. Verify Final State
        ↓
19. Automate Appropriate Actions
```

The central lesson was:

> **A query that executes successfully is not automatically a good detection.**

Detection logic must accurately represent the behavior it is intended to identify.

---

# Week 17 Investigation Method

A repeatable investigation method emerged across all six labs.

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
What did the system, process, identity, or user actually do?
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
REMEDIATION
Does anything need to be changed or removed?
        ↓
VERIFICATION
Did the response produce the intended security state?
        ↓
AUTOMATION
Can an appropriate repetitive step be safely automated?
        ↓
DOCUMENTATION
Can the final conclusion be defended with evidence?
```

---

# KQL Skills Demonstrated

Week 17 provided hands-on practice with:

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

The important skill was not memorizing syntax.

It was converting investigation questions into queries.

---

# Examples of Security Questions Converted into KQL

## Which users have the most failed authentication?

```kusto
SigninLogs
| where ResultType != 0
| summarize FailedAttempts = count() by UserPrincipalName
| order by FailedAttempts desc
```

---

## Which source IPs generated authentication failures?

```kusto
SigninLogs
| where ResultType != 0
| summarize FailedAttempts = count() by IPAddress
| order by FailedAttempts desc
```

---

## Which PowerShell processes executed?

```kusto
DeviceProcessEvents
| where FileName =~ "powershell.exe"
| project
    Timestamp,
    DeviceName,
    AccountName,
    FileName,
    ProcessCommandLine
```

---

## What network activity was associated with PowerShell?

```kusto
DeviceNetworkEvents
| where InitiatingProcessFileName =~ "powershell.exe"
| project
    Timestamp,
    DeviceName,
    RemoteUrl,
    RemoteIP,
    RemotePort,
    ActionType
```

---

## Which Azure operations affected the lab resource group?

```kusto
AzureActivity
| where ResourceGroup =~ "rg-week17-lab6-security"
| project
    TimeGenerated,
    Caller,
    CallerIpAddress,
    OperationNameValue,
    ActivityStatusValue
```

---

## Which successful RBAC role-assignment writes occurred?

```kusto
AzureActivity
| where OperationNameValue contains "ROLEASSIGNMENTS"
| where OperationNameValue contains "WRITE"
| where ActivityStatusValue =~ "Success"
| project
    TimeGenerated,
    Caller,
    CallerIpAddress,
    OperationNameValue,
    ActivityStatusValue,
    ResourceGroup
```

---

# Cybersecurity Skills Demonstrated

## Security Analysis

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

---

## Incident Investigation

- Alert triage
- Incident investigation
- Scope determination
- User investigation
- Device investigation
- Process-chain reconstruction
- File investigation
- Network investigation
- Identity correlation
- Persistence checks
- Credential-theft assessment
- Lateral-movement assessment
- C2 assessment
- Exfiltration assessment
- Privileged-access review
- Containment evaluation
- Evidence-based disposition

---

## Threat Hunting

- Advanced Hunting
- KQL
- Behavior-based hunting
- PowerShell hunting
- Encoded-command hunting
- Process-chain hunting
- File telemetry hunting
- Network telemetry hunting
- Identity hunting
- Azure Activity hunting
- RBAC hunting
- Cross-table investigation

---

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
- RBAC detection
- Scheduled analytics rules
- Detection-to-automation workflows

---

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

---

## Identity Security

- Microsoft Entra ID
- Microsoft Entra security groups
- Group membership
- `SigninLogs`
- Failed-login investigation
- Source IP analysis
- Authentication timelines
- Success/failure correlation
- Authentication-result analysis
- Identity entity mapping
- Authentication vs. authorization
- Permission review

---

## Cloud Security

- Microsoft Azure
- Azure resource groups
- Azure Activity
- Azure RBAC
- Role assignments
- Reader permissions
- Contributor permissions
- Network Security Groups
- Cloud configuration changes
- Administrative activity monitoring
- Privileged-access monitoring
- Least-privilege remediation
- Cloud identity correlation

---

## SIEM Engineering

- Microsoft Sentinel
- Azure Log Analytics
- Data connectors
- Diagnostic settings
- Security telemetry ingestion
- Table validation
- Analytics rules
- Alert generation
- Incident generation
- KQL
- Automation rules
- Alert-update automation
- Automation-rule scoping

---

## Security Automation

- Sentinel automation rules
- Detection-based automation conditions
- Automated alert updates
- Scoped security automation
- Detection-to-response workflow
- Basic SOAR concepts

---

## Security Engineering

- Security environment deployment
- Endpoint onboarding
- Security service validation
- Telemetry pipeline troubleshooting
- Detection testing
- Detection troubleshooting
- Security control validation
- Cloud permission management
- Least-privilege validation
- Response capability assessment
- Automation design

---

# MITRE ATT&CK Exposure

| Mapping | Context |
| --- | --- |
| **T1110 — Brute Force** | Repeated Microsoft Entra failed sign-in detection |
| **T1496 — Resource Hijacking** | Sentinel suspicious resource deployment analytics-rule review |
| **Privilege Escalation — Tactic** | Azure RBAC role-assignment monitoring and detection |

MITRE ATT&CK was used as a standardized behavioral framework rather than proof that an attacker was responsible.

---

# Security Engineering Lessons

## Telemetry Comes Before Detection

```text
NO DATA
   ↓
NO QUERY
   ↓
NO DETECTION
   ↓
NO ALERT
   ↓
NO INVESTIGATION
```

---

## Detection Comes Before Automation

```text
UNDERSTAND BEHAVIOR
        ↓
BUILD DETECTION
        ↓
VALIDATE DETECTION
        ↓
TUNE DETECTION
        ↓
THEN AUTOMATE
```

---

## Investigation Comes Before Response

```text
SUSPICIOUS BEHAVIOR
        ↓
INVESTIGATION
        ↓
EVIDENCE
        ↓
RISK ASSESSMENT
        ↓
RESPONSE DECISION
```

---

## Remediation Should Be Verified

```text
IDENTIFY RISK
        ↓
REMEDIATE
        ↓
CHECK FINAL STATE
        ↓
VERIFY RISK WAS REDUCED
```

Lab 06 demonstrated this by removing Contributor access and then confirming that the test identity retained Reader access.

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
AUTOMATION
   ↓
RESPONSE
```

---

# What I Troubleshot

During Week 17, I worked through problems involving:

- Defender endpoint onboarding
- Defender service dependencies
- Windows endpoint capabilities
- Missing Microsoft Entra `SigninLogs`
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
- Azure RBAC role-assignment visibility
- Permission-state verification
- Sentinel automation-rule scoping

Troubleshooting was an important part of the project because real security environments do not always behave exactly as expected.

---

# Hiring Manager / Recruiter Quick View

## What I Built

I configured and operated a Microsoft cybersecurity lab containing:

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
- Azure RBAC
- Network Security Groups
- KQL hunting
- Advanced Hunting
- Custom detection logic
- Alert generation
- Incident generation
- Incident investigation
- Least-privilege remediation
- Security automation
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

- Microsoft Entra authentication
- Failed authentication
- Repeated failures
- Source IP addresses
- Authentication timing
- Failure reasons
- Success/failure sequences
- Targeted applications
- Security-group membership
- Authentication vs. authorization

### Cloud

- Azure administrative operations
- Azure Activity telemetry
- Resource activity
- Azure RBAC
- Role assignments
- Reader permissions
- Contributor permissions
- Network Security Groups
- Privileged-access changes
- Least-privilege verification

### Detection Engineering

- Detection-query development
- Thresholds
- Time windows
- False-negative troubleshooting
- Entity mapping
- MITRE ATT&CK
- Alert generation
- Incident generation
- RBAC role-assignment detection
- Privilege Escalation mapping
- Detection-to-automation workflow

### Security Response

- Initial triage
- Evidence validation
- Timeline reconstruction
- Process correlation
- File correlation
- Network correlation
- Identity correlation
- Persistence assessment
- Containment assessment
- Permission remediation
- Least-privilege verification
- Final disposition
- Automated triage support

---

# Interview-Ready Project Explanation

> I built and operated a Microsoft cybersecurity lab using Microsoft Defender XDR, Defender for Endpoint, Microsoft Sentinel, Microsoft Entra ID, Azure Log Analytics, Azure RBAC, Advanced Hunting, and KQL.
>
> I began by configuring the cloud and endpoint environment and onboarding a Windows 11 Enterprise endpoint into Defender for Endpoint. I encountered an onboarding problem involving the endpoint security services, so I validated the Windows edition, endpoint capabilities, and service dependencies before completing the onboarding.
>
> I generated controlled endpoint activity, investigated Defender detections, reviewed process lineage, and validated the underlying telemetry through Advanced Hunting.
>
> I then worked with Microsoft Defender XDR to understand the relationship between alerts, incidents, devices, users, evidence, timelines, and endpoint response actions.
>
> I expanded the environment into Microsoft Sentinel and Log Analytics, connected Azure Activity telemetry, and used KQL to filter, project, aggregate, and analyze cloud administrative activity.
>
> For the identity-security investigation, I discovered that Microsoft Entra sign-in telemetry was initially missing from Log Analytics. I configured the required logging, validated `SigninLogs` ingestion, generated controlled failed authentication attempts, and investigated the account, source IP, timestamps, applications, and failure reasons.
>
> I converted manually validated hunting logic into a scheduled Sentinel analytics rule for repeated failed sign-ins. The first version did not trigger even though the source events existed. I traced the problem to fixed five-minute time buckets that split related authentication attempts across separate groups. I corrected the KQL, validated the logic again, generated new activity, and successfully produced the expected security detection.
>
> I investigated the resulting activity and determined that the detection was valid but the behavior was authorized testing. I documented the evidence instead of treating a valid alert as automatic proof of compromise.
>
> I then performed a deeper suspicious PowerShell endpoint investigation. I generated controlled encoded PowerShell, child-process, file, and network activity and reconstructed the behavior using Advanced Hunting.
>
> `DeviceProcessEvents` showed encoded PowerShell execution and a `powershell.exe → cmd.exe → whoami.exe` process chain. `DeviceFileEvents` showed file creation. `DeviceNetworkEvents` proved that PowerShell successfully communicated with `example.com` over TCP port 443.
>
> I also reviewed Device Timeline and existing Defender incidents. Some existing incidents looked relevant based on their names, but timestamp validation showed they occurred weeks before my controlled activity, so I excluded them rather than incorrectly forcing them into the investigation.
>
> I evaluated endpoint response capabilities including isolation, Live Response, automated investigation, and investigation-package collection. Because I found no malicious payload, persistence, credential theft, lateral movement, command-and-control, exfiltration, or continuing unauthorized access, I determined that endpoint isolation was not justified.
>
> In Lab 06, I expanded the project into Azure access-control monitoring. I created a dedicated resource group and Network Security Group, worked with Microsoft Entra identity controls, assigned Azure RBAC permissions, and generated controlled role-assignment activity.
>
> I used `AzureActivity` and `SigninLogs` to investigate the cloud and identity evidence, then built and enabled the Sentinel analytics rule `Lab - Azure RBAC Role Assignment Activity` to detect successful RBAC role-assignment writes.
>
> I temporarily assigned Contributor to the controlled test identity, removed that access when it was no longer required, and verified that the final role was Reader. This gave me practical experience applying least privilege rather than only learning it as a theory.
>
> I also created `Lab - RBAC Incident Triage Automation`, a Sentinel automation rule scoped specifically to the RBAC analytics rule. This connected validated detection logic with automated alert handling.
>
> The main lesson from the project was that security investigation is not about trusting an alert title or reacting automatically to suspicious-looking behavior. It requires validating telemetry, reconstructing behavior, correlating evidence, testing detection logic, understanding identity and permissions, assessing risk, remediating unnecessary access, verifying the final state, and making defensible response decisions.

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
19. Determine benign vs. suspicious vs. malicious
        ↓
20. Contain if justified
        ↓
21. Document evidence
```

---

# Interview Question — How Would You Investigate Failed Logins?

```text
1. Confirm authentication activity
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

# Interview Question — How Would You Investigate an Unexpected Azure RBAC Role Assignment?

```text
1. Confirm role-assignment event
        ↓
2. Identify caller
        ↓
3. Identify target identity
        ↓
4. Identify assigned role
        ↓
5. Identify Azure scope
        ↓
6. Confirm operation status
        ↓
7. Review source IP
        ↓
8. Correlate SigninLogs
        ↓
9. Review authentication context
        ↓
10. Determine whether assignment was expected
        ↓
11. Review activity after privilege change
        ↓
12. Hunt for additional role assignments
        ↓
13. Determine whether access is still required
        ↓
14. Remove unauthorized or unnecessary access
        ↓
15. Verify least privilege
        ↓
16. Escalate or contain if evidence supports it
        ↓
17. Document final disposition
```

The key idea is:

> **A role assignment is a security-relevant event, but the investigation must establish whether the change was authorized, necessary, and safe.**

---

# Interview Question — What Did You Learn About Detection Engineering?

> A detection rule is not correct simply because the KQL executes without errors.
>
> During my failed-login investigation, I initially grouped authentication events using fixed five-minute time buckets. Three failures occurred within only a few minutes, but one landed on one side of the fixed boundary and two landed on the other side.
>
> The threshold therefore never reached three.
>
> I verified the source telemetry, isolated the failure to the query logic, corrected the detection, validated it again, and successfully generated the expected security detection.
>
> That demonstrated that detection engineering requires behavioral testing, not only syntax validation.

---

# Interview Question — Why Is Encoded PowerShell Suspicious?

> `-EncodedCommand` deserves investigation because encoded PowerShell can make commands harder to quickly interpret.
>
> However, encoding does not automatically prove malicious activity.
>
> Administrators, automation systems, scripts, software, and security tools may also use encoded commands.
>
> I therefore treat encoding as an investigation signal and determine what the command actually did by correlating process, file, network, user, and timeline telemetry.

---

# Interview Question — Why Did You Not Isolate the Endpoint?

> I considered isolation because the activity involved encoded PowerShell, CMD, WHOAMI, file creation, and outbound network communication.
>
> However, the investigation established that the behavior was authorized testing, and I found no evidence of malware, persistence, credential theft, lateral movement, command-and-control, exfiltration, or continued unauthorized access.
>
> Isolation therefore was not supported by the evidence.
>
> The lab reinforced that containment decisions should be based on risk and evidence rather than automatically triggered by suspicious-looking behavior.

---

# Interview Question — What Did Lab 06 Teach You About Least Privilege?

> I temporarily assigned Contributor access to a controlled Azure test identity so I could generate realistic RBAC activity and validate the monitoring workflow.
>
> After testing was complete, I removed Contributor and verified that the account retained only Reader access.
>
> This demonstrated least privilege as an operational process rather than only a definition. Access should be granted for a reason, reviewed, removed when it is no longer required, and verified afterward.

---

# Interview Question — Why Automate Only After Validating the Detection?

> Automation can make security operations faster, but it can also make bad logic act faster.
>
> In Lab 06, I first generated the RBAC activity, verified the raw `AzureActivity` telemetry, built and reviewed the detection logic, created the analytics rule, and then created an automation rule scoped specifically to the RBAC detection.
>
> This reinforced that automation should sit on top of validated telemetry and validated detection logic.

---

# Week 17 Final Findings

The six labs established that:

- A Microsoft security environment can be built from the ground up.
- Defender for Endpoint depends on working operating-system services and capabilities.
- Endpoint telemetry can be independently validated.
- Alerts and incidents are not the same thing.
- Alerts are investigation starting points.
- XDR incidents provide broader context than individual detections.
- Device and user entities provide useful investigation pivots.
- Process lineage provides more context than process names alone.
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
- Azure RBAC changes can be monitored through `AzureActivity`.
- Authentication and authorization should be investigated separately and correlated.
- Role assignments can be real security-relevant activity without being malicious.
- Successful RBAC write activity can become a scheduled Sentinel detection.
- Privilege-related detections require context before malicious classification.
- Temporary Contributor access should be removed when no longer required.
- Least privilege requires verification after remediation.
- Sentinel automation rules can be scoped to specific analytics rules.
- Detection should be validated before response automation is added.
- Cloud identity, permission, and resource activity are stronger when investigated together.

---

# Final Technical Outcome

Week 17 provided practical exposure to:

> **Telemetry Generation → Collection → Ingestion → Storage → Querying → Hunting → Detection → Alerting → Correlation → Investigation → Timeline Reconstruction → Process Analysis → File Analysis → Network Analysis → Identity Analysis → RBAC Analysis → Validation → Classification → Risk Assessment → Least-Privilege Remediation → Verification → Automation → Response → Documentation**

The six-lab progression became:

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
    ↓
REMEDIATE
    ↓
VERIFY
    ↓
AUTOMATE
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

Custom Analytics Rules:
CREATED & ENABLED

Detection Troubleshooting:
COMPLETED

Alert Generation:
VALIDATED

Incident Investigation:
COMPLETED

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

Azure RBAC Investigation:
COMPLETED

RBAC Analytics Rule:
CREATED & ENABLED

RBAC Detection Severity:
MEDIUM

MITRE ATT&CK RBAC Mapping:
PRIVILEGE ESCALATION

Temporary Contributor Access:
REMOVED

Final Lab Cloud User Role:
READER

Least-Privilege Verification:
COMPLETED

Sentinel Automation Rule:
CREATED & ENABLED

Automated Alert Handling:
CONFIGURED

Lab 06 Classification:
AUTHORIZED ADMINISTRATIVE SECURITY TESTING

Confirmed Lab 06 Compromise:
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

Lab 06 added:

```text
ROLE ASSIGNMENT
  ≠
MALICIOUS PRIVILEGE ESCALATION
```

and:

```text
AUTOMATION
  ≠
REPLACEMENT FOR INVESTIGATION
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
REMEDIATION
      ↓
VERIFICATION
      ↓
APPROPRIATE AUTOMATION
      ↓
RESPONSE
```

---

# Key Takeaway

> **Security tools generate information. Security professionals turn that information into defensible decisions.**

Week 17 strengthened my ability to move from raw endpoint, cloud, identity, process, file, network, authentication, and privileged-access telemetry to a final security conclusion using Microsoft Defender XDR, Microsoft Defender for Endpoint, Microsoft Sentinel, Microsoft Entra ID, Azure Log Analytics, Azure RBAC, Network Security Groups, Advanced Hunting, Sentinel analytics rules, Sentinel automation, and KQL.

The most important outcome was not learning where buttons are located.

It was learning how to follow the evidence:

```text
What happened?
      ↓
Who or what was involved?
      ↓
Which identity authenticated?
      ↓
What permissions did that identity have?
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
Which Azure resources changed?
      ↓
Were permissions modified?
      ↓
What happened before and after?
      ↓
What does the raw telemetry prove?
      ↓
Why did the detection trigger?
      ↓
Does the alert actually belong to this activity?
      ↓
Was the behavior authorized?
      ↓
Is there evidence of continued compromise?
      ↓
What explanation best fits all of the evidence?
      ↓
What response is actually justified?
      ↓
Does unnecessary access need to be removed?
      ↓
Was the final security state verified?
      ↓
Can part of the workflow be safely automated?
```

That is the investigation and security engineering mindset I am continuing to develop across endpoint security, identity security, cloud security, SIEM, threat hunting, detection engineering, incident investigation, incident response, access control, security automation, and security engineering.

---

# Week 17 Completion

```text
LAB 01
Microsoft Defender XDR & Defender for Endpoint
        ↓
COMPLETED

LAB 02
Microsoft Sentinel & Azure Log Analytics
        ↓
COMPLETED

LAB 03
Microsoft Entra Identity Investigation
        ↓
COMPLETED

LAB 04
Custom Detection Engineering
        ↓
COMPLETED

LAB 05
Suspicious PowerShell Endpoint Investigation & Response
        ↓
COMPLETED

LAB 06
Azure RBAC Security Monitoring, Detection & Automated Triage
        ↓
COMPLETED
```

## Final Week 17 Status

**COMPLETED**
