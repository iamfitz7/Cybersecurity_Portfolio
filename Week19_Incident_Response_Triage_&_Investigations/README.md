# Week 19 – Triage and Investigations

## Lab 1 – SVCHOST Dump Creation Alert Triage and Benign Activity Validation

## Overview

Security alerts are investigative starting points, not automatic proof that malicious activity occurred.

In this lab, I investigated a medium-severity alert in Elastic Security titled **SVCHOST Dump creation attempt on endpoints**. At first glance, the alert name suggested potentially suspicious process dumping activity. Process dump files can be associated with legitimate crash diagnostics, system troubleshooting, application failures, forensic activity, and malicious credential access techniques. Because the existence of a `.DMP` file does not establish malicious intent by itself, the investigation focused on validating the underlying endpoint telemetry before making a classification decision.

The alert involved a file named:

`svchost.DMP`

located at:

`C:\Users\DELL\AppData\Local\Temp\svchost.DMP`

The process associated with the observed file event was:

`C:\Windows\System32\cleanmgr.exe`

The alert was generated on host:

`desktop-il6ukq9`

under user:

`DELL`

The alert had:

- Severity: Medium
- Risk Score: 47
- Observed file action: Deletion

The central investigative question was:

> Did the available evidence support malicious SVCHOST process dumping, or was the alert associated with legitimate Windows cleanup activity involving an existing dump artifact?

The investigation required reviewing the alert reason, understanding the detection rule, validating the actual file event semantics, examining process lineage, analyzing command-line arguments, checking host context, and searching for additional DMP-related alert activity.

The final assessment was:

**False Positive / Benign Activity – High Confidence**

The available evidence showed that the observed endpoint event was a deletion of `svchost.DMP` by the Windows Disk Cleanup utility `cleanmgr.exe`. The process executed with command-line arguments consistent with automated Storage Sense cleanup activity, and the process lineage supported normal Windows maintenance behavior. No corroborating alert-level evidence was identified to support malicious dump creation, credential access activity, suspicious dump tooling, malware execution, or follow-on compromise.

---

## Investigation Objectives

The objectives of this investigation were to:

- Review and triage the SVCHOST dump alert.
- Identify the affected host and user.
- Determine the actual file operation associated with `svchost.DMP`.
- Distinguish the alert title from the underlying endpoint telemetry.
- Review the detection rule logic.
- Validate the process responsible for the file activity.
- Analyze the process executable path.
- Examine process command-line arguments.
- Reconstruct the relevant process lineage.
- Determine whether the behavior was consistent with legitimate Windows cleanup activity.
- Search for broader DMP-related alert activity on the affected endpoint.
- Evaluate alternative malicious hypotheses.
- Reach a defensible classification based on correlated evidence.

---

## Alert Summary

| Field | Value |
|---|---|
| Alert Name | SVCHOST Dump creation attempt on endpoints |
| Severity | Medium |
| Risk Score | 47 |
| Host | desktop-il6ukq9 |
| User | DELL |
| Process | cleanmgr.exe |
| Process Path | C:\Windows\System32\cleanmgr.exe |
| File | svchost.DMP |
| File Path | C:\Users\DELL\AppData\Local\Temp\svchost.DMP |
| Observed Event Action | Deletion |
| Final Classification | False Positive / Benign Activity |
| Confidence | High |

---

## Investigation Methodology

This investigation followed an evidence-driven triage methodology.

The investigation did not begin with the assumption that the alert title accurately described malicious activity. Instead, the alert was treated as a hypothesis requiring validation.

The workflow followed this general structure:

1. Review the alert.
2. Identify the host, user, process, file, and severity.
3. Review the detection rule logic.
4. Inspect the alert reason.
5. Determine the actual event action.
6. Validate the process executable path.
7. Examine process lineage.
8. Review process command-line arguments.
9. Search for additional related DMP alerts.
10. Consider alternative explanations.
11. Compare the malicious and benign hypotheses.
12. Document the final assessment with an explicit confidence level.

This methodology was important because:

**An alert name is not the same as an incident conclusion.**

The investigation needed to answer what actually happened, which process performed the activity, and whether the surrounding context supported a malicious or benign explanation.

---

## Investigation Walkthrough

### 1. Initial Alert Review

The investigation began with a medium-severity Elastic Security alert titled:

`SVCHOST Dump creation attempt on endpoints`

The alert carried a risk score of 47.

The initial alert reason identified:

- Process: `cleanmgr.exe`
- File: `svchost.DMP`
- User: `DELL`
- Host: `desktop-il6ukq9`

The alert title initially raised the possibility of suspicious process dump activity.

However, the associated process was `cleanmgr.exe`, located at:

`C:\Windows\System32\cleanmgr.exe`

This introduced an immediate alternative hypothesis: the alert may have been generated from legitimate Windows cleanup activity involving an existing dump file.

At this stage, no final classification was made.

The investigation continued to determine whether the evidence supported malicious dump creation or benign cleanup behavior.

---

### 2. Detection Rule Review

The alert rule was reviewed to understand why the detection fired.

The rule query included logic matching a host and DMP-related activity involving `svchost.DMP`.

Reviewing the detection logic was important because detection rules identify patterns of interest, but the rule name alone does not establish the intent or complete context of the activity.

The investigation therefore separated:

- What the rule was designed to detect.
- What the underlying telemetry actually showed.
- What security conclusion the evidence supported.

This prevented the investigation from assuming that a rule titled as a dump creation attempt necessarily proved that the observed event represented malicious dump creation.

---

### 3. Alert Reason Analysis

The alert reason provided a major investigative finding.

The observed event involved:

`svchost.DMP`

at:

`C:\Users\DELL\AppData\Local\Temp\svchost.DMP`

The process associated with the event was:

`cleanmgr.exe`

Most importantly, the underlying event represented file deletion activity.

This materially changed the interpretation of the alert.

The evidence did not show the investigated process creating the dump file. Instead, it showed `cleanmgr.exe` acting on the file through a deletion event.

This distinction became central to the final assessment.

---

### 4. Process Path Validation

The process executable path was reviewed:

`C:\Windows\System32\cleanmgr.exe`

The location was consistent with the standard Windows Disk Cleanup utility.

The process path alone was not treated as sufficient proof of benign behavior. Process names and paths must be interpreted alongside additional evidence such as:

- Process arguments.
- Parent-child relationships.
- File operation type.
- User context.
- Host activity.
- Related alerts.
- Surrounding behavior.

The investigation therefore continued into process lineage and command-line analysis.

---

### 5. Process Lineage Analysis

Elastic Process Analyzer was used to reconstruct the relevant process chain.

The observed lineage included:

`services.exe`

↓

`svchost.exe`

↓

`cleanmgr.exe`

↓

`DismHost.exe`

This process context was important.

The chain was consistent with Windows service-driven maintenance and cleanup behavior. The investigation did not identify a suspicious execution chain involving unknown binaries or obvious dump-related utilities in the reviewed alert context.

The lineage supported the benign hypothesis but was not used in isolation.

The conclusion was based on the combined evidence.

---

### 6. Command-Line Argument Analysis

The `cleanmgr.exe` process arguments were reviewed.

The observed execution included:

`C:\WINDOWS\system32\cleanmgr.exe`

with arguments including:

`/autocleanstoragesense`

`/d`

`C:`

These arguments strongly supported automated cleanup activity on the C: drive.

This was one of the strongest pieces of contextual evidence in the investigation.

The question was no longer simply:

> Is cleanmgr.exe a legitimate Windows process?

The more important question was:

> What was this specific cleanmgr.exe execution doing?

The command-line context supported automatic storage cleanup rather than intentional process dumping.

When combined with the deletion event involving a `.DMP` file in the user's Temp directory, the evidence was consistent with Windows cleanup behavior encountering and removing an existing dump artifact.

The investigation did not claim to establish how the dump file was originally created because the available alert-level evidence did not prove its original creation mechanism.

---

### 7. DMP Alert Scope Validation

A host-specific DMP search was performed using:

`host.name : "desktop-il6ukq9" and file.extension : "DMP"`

The filtered Alerts data view returned the current SVCHOST dump alert without revealing a broader DMP-alert pattern in the searched alert scope.

This was useful negative evidence.

However, the result was interpreted carefully.

The search supported the statement:

> No additional matching DMP-related alerts were identified in the searched alert dataset and scope.

It did not prove that no other file activity had ever occurred on the endpoint.

This distinction is important because:

**No alert found does not necessarily mean no underlying activity occurred.**

The investigation remained limited to what could be supported by the available telemetry and searches.

---

## Hypothesis Analysis

### Hypothesis 1: Malicious Process Dumping

This hypothesis would be strengthened by evidence such as:

- Suspicious dump utility execution.
- ProcDump activity.
- PowerShell-based dumping.
- Rundll32 and comsvcs.dll abuse.
- Suspicious Task Manager dumping.
- Unknown binaries creating dump files.
- Credential access alerts.
- Malicious parent processes.
- Follow-on execution.
- Persistence.
- Malware detections.
- Suspicious network communication.

The reviewed evidence did not provide corroborating support for these behaviors in the investigated alert context.

Therefore, the malicious dumping hypothesis had weak support.

---

### Hypothesis 2: Legitimate Windows Cleanup Activity

This hypothesis was supported by:

- The observed process was `cleanmgr.exe`.
- The executable path was `C:\Windows\System32\cleanmgr.exe`.
- The underlying file action was deletion.
- The affected file was located in a temporary directory.
- The command line included `/autocleanstoragesense`.
- The command line targeted the C: drive.
- The process lineage was consistent with Windows maintenance activity.
- The reviewed alert scope did not reveal corroborating suspicious DMP-alert activity.
- No supporting evidence of malicious dumping or follow-on compromise was identified during the performed investigation.

The benign cleanup hypothesis provided the best explanation for the available evidence.

---

## Evidence Summary

### Evidence 1 – Alert Context

Elastic generated a medium-severity alert with risk score 47 involving `svchost.DMP`.

### Evidence 2 – Actual File Action

The underlying telemetry represented deletion of the dump file rather than confirmed creation by the observed process.

### Evidence 3 – Process Identity

The file operation was associated with:

`C:\Windows\System32\cleanmgr.exe`

### Evidence 4 – Command-Line Context

The process executed with arguments including:

`/autocleanstoragesense /d C:`

This was consistent with automated storage cleanup activity.

### Evidence 5 – Process Lineage

The process analyzer showed Windows service and maintenance-related process relationships involving:

`svchost.exe → cleanmgr.exe → DismHost.exe`

### Evidence 6 – Scope Review

The performed host-specific DMP alert search did not identify a broader pattern of matching dump-related alerts within the searched alert scope.

### Evidence 7 – Lack of Corroborating Malicious Evidence

The investigation did not identify supporting alert evidence of malicious dump tooling, credential access activity, malware execution, or suspicious follow-on behavior.

---

## Final Assessment

**Classification:** False Positive / Benign Activity

**Confidence:** High

### Assessment Rationale

The alert was triggered by activity involving `svchost.DMP`; however, the underlying endpoint event represented deletion of the file by `C:\Windows\System32\cleanmgr.exe`.

The process executed with command-line arguments including:

`/autocleanstoragesense /d C:`

which supported automated Windows storage cleanup activity.

The process lineage was consistent with Windows maintenance behavior, and the reviewed alert context did not provide corroborating evidence of malicious dump creation, credential access tooling, malware execution, or follow-on compromise.

Based on the available evidence, legitimate Windows cleanup activity provided the strongest explanation for the observed behavior.

The alert was therefore classified as:

**False Positive / Benign Activity – High Confidence**

---

## Key Investigative Lessons

### 1. Alert Titles Must Be Validated

A detection rule name describes what a detection is looking for. It does not automatically prove that the underlying activity was malicious.

### 2. Event Semantics Matter

The difference between file creation and file deletion can completely change the interpretation of an alert.

### 3. Process Names Alone Are Not Enough

A legitimate process name should not automatically result in closure.

The process path, arguments, lineage, event action, and surrounding context should be correlated.

### 4. Command-Line Arguments Provide Intent Context

The arguments:

`/autocleanstoragesense /d C:`

provided strong context that the observed process execution was associated with storage cleanup activity.

### 5. Negative Evidence Must Be Described Precisely

A search returning no additional alerts does not prove that activity never occurred.

The correct conclusion is limited to the searched dataset, timeframe, and telemetry scope.

### 6. Good Triage Prevents Unnecessary Escalation

The purpose of SOC analysis is not to classify every unusual event as malicious.

The analyst's responsibility is to determine which explanation best fits the available evidence and document that conclusion clearly.

---

## Investigation Workflow

`Alert Review`

↓

`Detection Rule Review`

↓

`Alert Reason Analysis`

↓

`Event Action Validation`

↓

`Process Path Review`

↓

`Process Lineage Analysis`

↓

`Command-Line Argument Validation`

↓

`DMP Alert Scope Search`

↓

`Hypothesis Comparison`

↓

`False Positive / Benign Activity`

↓

`High Confidence`

---

## Skills Demonstrated

- Security alert triage
- Elastic Security investigation
- EDR telemetry analysis
- KQL filtering
- Detection rule analysis
- File event analysis
- Process lineage reconstruction
- Command-line argument analysis
- Host-level scope validation
- False-positive identification
- Evidence correlation
- Hypothesis-driven investigation
- Negative evidence interpretation
- Incident documentation
- Defensible classification and confidence assessment

---

## Tools Used

- Elastic Security
- Elastic Alerts
- Elastic Process Analyzer
- KQL
- Endpoint telemetry
- Process lineage analysis
- File event analysis

---

## Conclusion

This investigation demonstrated the importance of validating what actually occurred rather than relying on the wording of a security alert.

Although the alert title suggested an SVCHOST dump creation attempt, the underlying evidence showed a deletion event involving `svchost.DMP` and `cleanmgr.exe`. Further analysis of the process path, process lineage, command-line arguments, and host-specific DMP alert scope supported legitimate Windows cleanup activity as the best explanation for the observed event.

The most important lesson from this investigation was that effective alert triage requires more than recognizing suspicious keywords.

It requires understanding:

- What happened.
- Which process performed the activity.
- What the process was instructed to do.
- Where the file was located.
- What occurred before and after the event.
- Whether other evidence supports or contradicts the initial hypothesis.
- What the available telemetry can and cannot prove.

The final classification was based on correlated evidence rather than the alert title alone:

**False Positive / Benign Activity – High Confidence**

# Week 19 Lab 2 – RDP Successful RemoteInteractive Logon Investigation

## Overview

This lab focused on the investigation and triage of a successful Remote Desktop Protocol (RDP) authentication alert in Elastic Security.

The alert, titled **RDP - Successful Interactive Remote Logon Detected**, identified a successful Windows authentication event associated with **Windows Security Event ID 4624** and **Logon Type 10**, which represents a RemoteInteractive logon session.

The purpose of this investigation was not simply to confirm that an RDP authentication occurred. The primary analytical objective was to determine whether the successful remote authentication represented legitimate remote administrative activity, unauthorized access, compromised credentials, or activity requiring escalation.

The investigation followed an evidence-driven SOC methodology. Instead of treating the alert title, severity, source geography, or privileged account usage as automatic proof of compromise, the alert was treated as an investigative hypothesis.

The investigation included:

- Detection rule analysis
- Alert validation
- Windows authentication event analysis
- Logon Type validation
- Privileged account analysis
- Host investigation
- Source IP analysis
- Geolocation enrichment
- Network organization enrichment
- Elastic Security Timeline analysis
- Entity investigation
- Alert correlation review
- Prevalence review
- Authentication event correlation
- Source IP pivoting
- Host and user pivoting
- Process activity review
- Network activity review
- Hypothesis testing
- Evidence-based alert disposition

The investigation confirmed that a successful RemoteInteractive authentication occurred against the monitored Windows endpoint. However, the available telemetry did not provide sufficient corroborating evidence of malicious post-authentication behavior.

The final assessment was:

**Disposition: Benign Positive**

**Confidence: Moderate**

The detection itself functioned correctly and accurately identified successful RDP authentication activity. However, based on the evidence available within the investigation scope, the event did not meet the threshold for escalation as a confirmed security incident.

---

## Investigation Scenario

Remote Desktop Protocol is widely used for legitimate remote administration, troubleshooting, and infrastructure management. However, attackers can also abuse RDP after obtaining valid credentials.

A successful RDP authentication can represent several possible scenarios:

1. Legitimate remote administration
2. Expected help desk or system maintenance activity
3. Unauthorized access using stolen credentials
4. Brute-force activity followed by successful authentication
5. Password spraying followed by account compromise
6. Lateral movement between internal systems
7. External remote access to an exposed RDP service
8. Persistence through continued use of compromised credentials

Because legitimate and malicious RDP activity can generate similar authentication events, a successful RDP alert cannot be accurately classified from the alert title alone.

The analyst must determine:

> What happened, which account was used, which system was accessed, where the connection originated, and whether surrounding activity supports a legitimate or malicious explanation?

This question guided the investigation.

---

## Investigation Objectives

The primary objectives of the investigation were to:

1. Review the alert and identify the primary entities involved.
2. Understand the detection logic responsible for generating the alert.
3. Validate the underlying Windows authentication event.
4. Confirm whether the event represented successful RemoteInteractive authentication.
5. Identify the source IP address associated with the connection.
6. Identify the affected endpoint.
7. Identify the account used for authentication.
8. Determine whether the account was privileged.
9. Review available source geolocation and network ownership context.
10. Investigate the source IP for additional activity.
11. Investigate the host and account for surrounding activity.
12. Review relevant authentication events.
13. Search for suspicious process activity.
14. Search for suspicious network activity.
15. Identify additional correlated alerts or suspicious behavior.
16. Compare competing investigative hypotheses.
17. Reach a defensible final disposition based on available evidence.

---

## Alert Summary

The investigation began with the following alert:

| Field | Value |
|---|---|
| Alert Name | RDP - Successful Interactive Remote Logon Detected |
| Severity | Medium |
| Risk Score | 47 |
| Event Category | Authentication |
| Event Action | Logged-in |
| Host | workstation-01 |
| User | Administrator |
| Process | C:\Windows\System32\svchost.exe |
| Source IP | 142.114.44.227 |
| Source Country | Canada |
| Source Region | Ontario |
| Source Organization | Bell Canada |
| Windows Event ID | 4624 |
| Logon Type | 10 |
| Logon Process Name | User32 |

The initial alert evidence established that the event involved:

- A successful authentication
- A remote interactive logon type
- A privileged Administrator account
- A Windows workstation
- An external source IP address

These characteristics justified further investigation.

However, none of these characteristics independently established malicious intent.

---

## Phase 1 – Detection Rule Analysis

The investigation began by reviewing the detection rule responsible for generating the alert.

The rule description indicated that it was designed to detect Windows Security Event ID 4624 with Logon Type 10.

The detection logic was:


winlog.channel:"Security" and event.code:"4624" and winlog.event_data.LogonType:"10"


# Week 19 – Lab 3: Malware Prevention Alert Investigation

## Elastic Security Malware Detection, Process Lineage Analysis, and Prevention Validation

---

## Overview

Malware alerts provide an important starting point for an investigation, but the alert title alone does not explain the full story.

A security analyst still needs to answer important questions:

- What activity caused the alert?
- Which endpoint was affected?
- Which user was involved?
- What process caused the activity?
- What launched that process?
- Where did the original activity begin?
- What file was involved?
- Were other alerts connected to the same process chain?
- Did the suspicious activity continue?
- Did the security control only detect the activity, or did it prevent it?
- What can be proven from the available evidence?
- What still remains unknown?

In this investigation, I analyzed a **Malware Prevention Alert** in Elastic Security involving a suspicious file creation attempt on a Windows endpoint.

Instead of making a decision based only on the alert name or severity level, I investigated the activity by reviewing:

- Alert details
- Detection context
- Host information
- User information
- Process metadata
- Parent-child process relationships
- Process ancestry
- Original execution source
- Suspicious file information
- SHA-256 hash information
- Process Analyzer activity
- Related alerts
- Artifact prevalence
- Prevention evidence
- Quarantine-related evidence
- Timeline activity

The investigation showed that an unofficial game repack installer started a temporary setup process that attempted to create a file identified by Elastic Security as malicious.

Based on the available evidence, the final classification was:

> **True Positive – Prevented Malware Activity**

The investigation also showed an important lesson: even when a security product prevents malicious activity, the analyst should still determine what caused the activity, how far the execution chain progressed, and whether additional endpoint investigation is needed.

---

# Investigation Objectives

The main objectives of this investigation were to:

- Understand why the Malware Prevention Alert was generated.
- Validate the activity behind the detection.
- Identify the affected host.
- Identify the user connected to the activity.
- Determine which process caused the alert.
- Identify the parent process.
- Trace the full process ancestry.
- Find the original source of execution.
- Investigate the suspicious file creation attempt.
- Collect the available SHA-256 hash.
- Review process relationships in Elastic Security Process Analyzer.
- Review related alerts.
- Review artifact prevalence.
- Determine whether the activity was detected or prevented.
- Reconstruct the event timeline.
- Separate confirmed facts from assumptions.
- Determine the most reasonable final classification.
- Recommend reasonable response actions.

---

# Alert Summary

| Field | Value |
|---|---|
| Alert Name | Malware Prevention Alert |
| Severity | High |
| Risk Score | 73 |
| Host | `desktop-jlklrc9` |
| User | `DELL` |
| Process | `setup.tmp` |
| Parent Process | `setup.exe` |
| Suspicious File | `cls-lolzx_x86.exe` |
| Operating System | Windows 11 Pro |
| Final Classification | True Positive – Prevented Malware Activity |

The selected alert occurred on:

`July 6, 2026 at 12:21:49.020`

The alert was connected to suspicious file activity involving the following process:

`setup.tmp`

The suspicious file connected to the alert was:

`cls-lolzx_x86.exe`

At the beginning of the investigation, I did not immediately assume that I understood the full activity.

The alert provided the starting point, but I still needed to determine:

1. What exactly happened?
2. What process caused the alert?
3. What launched that process?
4. Where did the activity originally begin?
5. What file was involved?
6. Did the suspicious file successfully execute?
7. Did Elastic Security prevent the activity?
8. Were other alerts connected to the same event?
9. Was there evidence of additional suspicious activity?
10. What response actions would be reasonable?

---

# Investigation Methodology

I followed this investigation workflow:

```text
Alert
   ↓
Review Alert Details
   ↓
Identify Host and User
   ↓
Investigate Process
   ↓
Investigate Parent Process
   ↓
Trace Process Ancestry
   ↓
Find Original Execution Source
   ↓
Investigate Suspicious File
   ↓
Collect File Hash
   ↓
Review Process Tree
   ↓
Review Related Alerts
   ↓
Check Artifact Prevalence
   ↓
Validate Prevention Evidence
   ↓
Reconstruct Timeline
   ↓
Make Final Classification
   ↓
Recommend Response Actions
