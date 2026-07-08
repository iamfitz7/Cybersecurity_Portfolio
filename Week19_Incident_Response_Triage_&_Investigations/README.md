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


# Week 19 – Triage and Investigations

## Lab 4 – Malware Prevention Alert Investigation

---

## Overview

Security tools can detect and block malicious activity, but an alert still needs to be investigated before an analyst can understand what happened.

In this lab, I investigated a **high-severity Malware Prevention Alert** in Elastic Security. The alert was connected to a suspicious file creation attempt involving an installer process, a temporary setup process, and a file that Elastic Endpoint identified as malicious.

The investigation focused on answering several important questions:

- What caused the alert?
- Which user and endpoint were involved?
- What process attempted to create the detected file?
- What was the parent process?
- Where did the original installer come from?
- What did the process tree show?
- Was the malicious file successfully created?
- Did the endpoint security control prevent the activity?
- Should the alert be classified as a True Positive or False Positive?

Instead of making a decision based only on the alert name or severity, I reviewed the alert details, endpoint fields, file information, SHA-256 hash, process relationships, command-line data, process tree, timeline, quarantine evidence, and prevention result.

The investigation found that a temporary setup process attempted to create a file that Elastic Endpoint identified as malicious. The activity was traced back through the installer process chain, and the endpoint security control prevented the malicious file creation attempt.

**Final Classification: True Positive – Prevented Malware Activity**

---

## Objectives

The main objectives of this investigation were to:

- Triage a high-severity endpoint security alert
- Identify the affected user and endpoint
- Review the alert reason and detection details
- Identify the suspicious file involved in the alert
- Collect the file's SHA-256 hash
- Review the file path and temporary directory activity
- Identify the process responsible for the file creation attempt
- Identify the parent process
- Review process ancestry
- Trace the activity back to the original installer
- Review command-line and process argument information
- Reconstruct the process execution chain
- Review quarantine and prevention evidence
- Determine whether the alert represented real malicious activity
- Make an evidence-based final classification
- Identify possible next steps for a larger incident response investigation

---

## Tools and Technologies Used

- Elastic Security
- Elastic Endpoint
- Elastic Defend
- Endpoint Detection and Response telemetry
- Process Analyzer
- Process Tree Analysis
- Elastic Timeline
- Windows process telemetry
- File event telemetry
- SHA-256 hash analysis
- Command-line analysis
- Endpoint alert triage

---

## Alert Summary

The investigation started with a high-severity Malware Prevention Alert.

The main alert details were:

| Field | Value |
|---|---|
| Alert Name | Malware Prevention Alert |
| Severity | High |
| Risk Score | 73 |
| User | DELL |
| Host | desktop-jlklrc9 |
| Main Process | setup.tmp |
| Parent Process | setup.exe |
| Detected File | cls-lolzx_x86.exe |
| Event Action | creation |
| Event Code | malicious_file |
| Final Classification | True Positive – Prevented Malware Activity |

The alert showed that `setup.tmp`, with `setup.exe` as its parent process, was connected to an attempted file creation event involving `cls-lolzx_x86.exe`.

At this point, the alert appeared suspicious, but additional investigation was needed before making a final decision.

---

## Investigation Process

### 1. Initial Alert Review

I started by reviewing the Malware Prevention Alert in Elastic Security.

The alert was marked as:

- **Severity:** High
- **Risk Score:** 73
- **Status:** Open at the time of investigation

The alert reason connected the following activity:

- Process: `setup.tmp`
- Parent process: `setup.exe`
- File: `cls-lolzx_x86.exe`
- User: `DELL`
- Host: `desktop-jlklrc9`

This gave me the first basic relationship to investigate:

`setup.exe` → `setup.tmp` → attempted creation of `cls-lolzx_x86.exe`

![Malware Prevention Alert Overview](Screenshots/01_Malware_Prevention_Alert_Overview.png)

---

### 2. Affected Host and User Identification

The next step was identifying the user and endpoint connected to the activity.

The investigation showed:

- **User:** DELL
- **Host:** desktop-jlklrc9

I also reviewed the process executable path and suspicious file path.

This was important because identifying the affected user and endpoint creates the starting point for scope analysis.

In a real SOC environment, I could use this information to search for:

- Other alerts from the same endpoint
- Other suspicious activity involving the same user
- Similar file activity across other endpoints
- Additional executions of the same process
- Related authentication activity
- Related network connections

![Affected Host User and File Paths](Screenshots/02_Affected_Host_User_and_File_Paths.png)

---

### 3. Suspicious File Analysis

The file connected to the malware detection was:

`cls-lolzx_x86.exe`

The file was located under a temporary directory:

`C:\Users\DELL\AppData\Local\Temp\is-1II3F.tmp\cls-lolzx_x86.exe`

The investigation also identified the following SHA-256 hash:

`77b920a22f93f87d2624b58f729712ebacaf8b605437c2678ca4216a28f3b8b2`

The hash is an important investigation artifact because it can be used for:

- Threat intelligence research
- SIEM searches
- EDR searches
- Environment-wide threat hunting
- Identifying the same file on other endpoints
- Supporting future detection rules

The alert also contained quarantine-related evidence connected to the detected file.

![Malicious File Hash and Quarantine Evidence](Screenshots/03_Malicious_File_Hash_and_Quarantine_Evidence.png)

---

### 4. Event Classification Review

I reviewed the event fields to understand how the endpoint security product classified the activity.

The event included:

- `event.action: creation`
- `event.category: malware`
- `event.category: intrusion_detection`
- `event.category: file`
- `event.code: malicious_file`

These fields showed that the detection involved an attempted file creation event that the endpoint security product classified as malicious.

This evidence increased confidence that the alert represented a real security event rather than only a general suspicious process pattern.

![Malicious File Event Classification](Screenshots/05_Malicious_File_Event_Classification.png)

---

### 5. Prevention Validation

One of the most important parts of the investigation was determining whether the file creation attempt succeeded.

The Elastic Timeline event message connected:

- The user
- The endpoint
- The temporary setup process
- The parent installer
- The detected file
- The source installer
- The file hash
- The prevention result

The evidence showed that the endpoint was prevented from creating the malicious file.

This was an important difference in the impact assessment.

The alert was still a **True Positive** because the malicious activity was real, but the security control successfully prevented the specific file creation attempt.

![Alert Timeline and Prevention Message](Screenshots/09_Alert_Timeline_and_Prevention_Message.png)

---

## Process Lineage Analysis

Process lineage analysis was one of the most important parts of this investigation.

The process tree showed the following high-level execution chain:

`winlogon.exe`

↓

`userinit.exe`

↓

`explorer.exe`

↓

`setup.exe`

↓

`setup.tmp`

The tree also showed `WerFault.exe` activity after the temporary setup process.

The full visible chain was approximately:

`winlogon.exe` → `userinit.exe` → `explorer.exe` → `setup.exe` → `setup.tmp` → `WerFault.exe`

The first part of the chain is connected to a normal interactive Windows user session.

The investigation became more important when the process chain moved into:

`explorer.exe` → `setup.exe` → `setup.tmp`

The malware prevention event was connected to this installer activity.

![Full Process Ancestry Chain](Screenshots/06_Full_Process_Ancestry_Chain.png)

---

## Original Installer Source Analysis

I continued tracing the process activity backward to understand where the installer came from.

The process evidence connected the activity to the following source:

`D:\Games\Grand Theft Auto V [FitGirl Lolly Repack]\setup.exe`

The command-line and process argument information helped connect the temporary `setup.tmp` process to the original installer.

This created the following investigation chain:

`User Session`

↓

`explorer.exe`

↓

`setup.exe`

↓

`setup.tmp`

↓

`cls-lolzx_x86.exe` file creation attempt

↓

`Elastic Endpoint Prevention`

The source path added important context to the investigation. However, I did not make the final decision based only on the directory name.

The final decision was based on the complete evidence chain.

![Original Installer Source Path](Screenshots/12_Original_Installer_Source_Path.png)

---

## Command-Line Analysis

The command-line information helped explain the connection between the temporary setup process and the original installer.

Looking only at a process such as:

`C:\Users\DELL\AppData\Local\Temp\...\setup.tmp`

would not provide the complete story.

By reviewing the process arguments and command-line information, I was able to connect the temporary execution back to the original `setup.exe` process.

This showed why command-line analysis is important during endpoint investigations.

A process name alone may not provide enough information. An analyst should also review:

- Full executable path
- Command line
- Process arguments
- Parent process
- Grandparent process
- User
- Host
- Execution time
- File activity
- Network activity
- Child processes

![Process Command Line Evidence](Screenshots/13_Process_Command_Line_Evidence.png)

---

## Full Process Tree Review

The process tree gave a clear visual view of the execution activity.

The main process chain was:

`winlogon.exe`

↓

`userinit.exe`

↓

`explorer.exe`

↓

`setup.exe`

↓

`setup.tmp`

The process tree helped connect the alert back to the user's Windows session and original installer activity.

It also showed additional child-process activity involving `WerFault.exe`.

![Complete Process Tree Overview](Screenshots/15_Complete_Process_Tree_Overview.png)

---

## Post-Execution Activity

The process tree showed two `WerFault.exe` processes after the `setup.tmp` activity.

`WerFault.exe` is a legitimate Windows Error Reporting process. Because of this, I did not classify it as malicious based only on its name.

However, the process tree showed that the `WerFault.exe` processes had related:

- File activity
- Network activity
- Registry activity

This would be an important area for additional investigation in a production environment.

A deeper investigation could review:

- Network destinations
- DNS requests
- Files created or accessed
- Registry activity
- Process command lines
- Digital signatures
- Executable paths
- Timing compared to the original alert

![WerFault Child Process Activity](Screenshots/18_WerFault_Child_Process_Activity.png)

---

## Investigation Timeline

Based on the available evidence, the activity can be summarized in the following order:

### Stage 1 – User Session

A normal Windows user session was active through:

`winlogon.exe` → `userinit.exe` → `explorer.exe`

### Stage 2 – Installer Execution

The user session was connected to the execution of `setup.exe`.

### Stage 3 – Temporary Setup Activity

The installer started `setup.tmp` from a temporary AppData directory.

### Stage 4 – Malicious File Creation Attempt

`setup.tmp` attempted to create:

`cls-lolzx_x86.exe`

### Stage 5 – Endpoint Detection

Elastic Endpoint identified the file event as malicious.

### Stage 6 – Prevention

The endpoint security control prevented the malicious file creation attempt.

### Stage 7 – Additional Process Activity

Additional `WerFault.exe` activity appeared after the setup process activity and was documented for further investigation.

---

## Key Indicators and Artifacts

### Alert Information

- Alert: Malware Prevention Alert
- Severity: High
- Risk Score: 73
- Final Classification: True Positive – Prevented Malware Activity

### Endpoint Information

- User: DELL
- Host: desktop-jlklrc9

### Process Information

- Main Process: `setup.tmp`
- Parent Process: `setup.exe`
- Additional Observed Process: `WerFault.exe`

### File Information

- File Name: `cls-lolzx_x86.exe`
- Event Action: `creation`
- Event Code: `malicious_file`

### SHA-256

`77b920a22f93f87d2624b58f729712ebacaf8b605437c2678ca4216a28f3b8b2`

### Source Installer

`D:\Games\Grand Theft Auto V [FitGirl Lolly Repack]\setup.exe`

---

## Key Findings

### Finding 1 – High-Severity Malware Alert

A high-severity Malware Prevention Alert was generated with a risk score of 73.

### Finding 2 – Specific User and Endpoint Identified

The activity was connected to user `DELL` on host `desktop-jlklrc9`.

### Finding 3 – Suspicious File Identified

The alert was connected to the attempted creation of `cls-lolzx_x86.exe`.

### Finding 4 – Process Relationship Identified

The investigation showed that `setup.tmp` was connected to the file event and had `setup.exe` as its parent process.

### Finding 5 – File Hash Collected

A SHA-256 hash was available for the detected file and could be used for additional threat hunting.

### Finding 6 – Source Installer Identified

The process activity was traced back to an installer located under an unofficial game repack directory.

### Finding 7 – Prevention Confirmed

Elastic Endpoint prevented the malicious file creation attempt.

### Finding 8 – Process Tree Reconstructed

The process tree showed the activity from the Windows user session through the installer and temporary setup process.

### Finding 9 – Additional Child-Process Activity Observed

`WerFault.exe` activity appeared after the setup process and could be investigated further for complete incident scope.

---

## Final Classification

**True Positive – Prevented Malware Activity**

**Confidence Level: High**

The alert was classified as a True Positive because several pieces of evidence supported the detection:

- High-severity Malware Prevention Alert
- `malicious_file` event classification
- Specific attempted file creation
- Identified file name
- SHA-256 hash
- Clear process relationship
- Source installer path
- Command-line evidence
- Process tree evidence
- Quarantine-related information
- Endpoint prevention result

The investigation did not rely on one field or one screenshot.

The final decision was based on several pieces of evidence that supported each other.

---

## Impact Assessment

The available evidence showed that the malicious file creation attempt was prevented.

This means the immediate impact appears limited compared to a situation where the file was successfully created and executed.

However, successful prevention does not automatically prove that the endpoint is completely clean.

A full incident response investigation should still review:

- Earlier installer execution
- Other created files
- Additional malicious hashes
- Persistence mechanisms
- Scheduled tasks
- Services
- Registry Run keys
- Suspicious PowerShell activity
- Command shell activity
- Credential access attempts
- Suspicious network connections
- Similar alerts on other endpoints

The evidence in this lab supports a prevented malware event, but additional investigation would be needed in a production environment to confirm the full scope.

---

## Recommended Response Actions

If this alert occurred in a production environment, I would recommend:

1. Confirm that the malicious file remains blocked or quarantined.
2. Search the environment for the SHA-256 hash.
3. Search for the file name `cls-lolzx_x86.exe`.
4. Search for other endpoints that executed the same installer.
5. Review earlier alerts involving the affected endpoint.
6. Review the complete endpoint timeline before and after the detection.
7. Investigate the `WerFault.exe` file, network, and registry activity.
8. Review common persistence locations.
9. Review recently created files in temporary directories.
10. Run a complete endpoint security scan.
11. Review network telemetry for suspicious outbound communication.
12. Remove untrusted software according to organizational policy.
13. Escalate the incident if additional malicious behavior is discovered.

---

## Skills Practiced

This investigation gave me hands-on practice with:

- SOC alert triage
- Elastic Security
- Elastic Endpoint
- Malware prevention investigation
- Endpoint telemetry analysis
- File event analysis
- Process lineage analysis
- Parent-child process analysis
- Process tree investigation
- Timeline reconstruction
- Command-line analysis
- File path analysis
- SHA-256 hash collection
- IOC identification
- Quarantine validation
- Prevention validation
- True Positive classification
- Impact assessment
- Incident response thinking
- Evidence-based investigation

---

## Lessons Learned

This lab helped me understand that a security alert should be treated as the beginning of an investigation.

The alert title showed that malware prevention activity occurred, but I still needed to determine:

- What process caused the alert
- What file was involved
- What parent process launched the activity
- Where the original installer came from
- What the command line showed
- What the process tree showed
- Whether the file creation succeeded
- What happened after the alert

Another important lesson was understanding the difference between **detection** and **impact**.

The alert was a True Positive because the malicious activity was real.

At the same time, the endpoint security control successfully prevented the specific malicious file creation attempt.

Both facts can be true:

**The detection was real.**

**The prevention control worked.**

This is important in SOC investigations because a True Positive does not always mean that an attacker or malicious program successfully completed its goal.

---

## Conclusion

This investigation started with a high-severity Malware Prevention Alert and developed into a deeper endpoint investigation.

I identified the affected user and host, reviewed the detected file, collected the SHA-256 hash, analyzed the file path, reviewed quarantine evidence, traced the process ancestry, examined command-line information, identified the source installer, reconstructed the process tree, and reviewed post-execution process activity.

The evidence showed that `setup.tmp`, connected to installer activity, attempted to create `cls-lolzx_x86.exe` inside a temporary directory.

Elastic Endpoint identified the file event as malicious and prevented the file creation attempt.

Based on the complete evidence chain, the final classification was:

**True Positive – Prevented Malware Activity**

This lab strengthened my understanding of how a SOC analyst can move from an initial endpoint alert to a supported investigation decision by connecting alert data, file evidence, process relationships, command-line information, timeline activity, and endpoint response results.
