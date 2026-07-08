# 🛡️ Week 19 — Security Alert Triage & Endpoint Investigations

## Evidence-Driven SOC Triage, Authentication Analysis, Process Lineage Investigation, Malware Prevention Validation, and Defensible Alert Classification

---

## 📖 Overview

Week 19 focuses on one of the most important responsibilities in Security Operations: **turning security alerts into evidence-based investigation decisions**.

Modern security tools can generate alerts for suspicious file activity, authentication behavior, process execution, malware detections, and other potentially dangerous events. However, an alert is not automatically proof that a security incident occurred.

A security analyst still needs to investigate.

The central focus of this week was learning how to move from:

```text
Alert
   ↓
Initial Hypothesis
   ↓
Evidence Collection
   ↓
Context Validation
   ↓
Process / Authentication Analysis
   ↓
Scope Review
   ↓
Hypothesis Comparison
   ↓
Classification
   ↓
Response Decision
```

The investigations in this week required careful analysis of:

- Alert metadata
- Detection rule logic
- Windows authentication events
- Event semantics
- File operations
- Process executable paths
- Process ancestry
- Parent-child relationships
- Command-line arguments
- Endpoint Detection and Response telemetry
- Process trees
- File hashes
- Alert correlation
- Timeline evidence
- Artifact prevalence
- Prevention evidence
- Quarantine-related information
- Negative evidence
- Investigation limitations
- Confidence levels
- Response recommendations

The goal was not to classify every alert as malicious.

The goal was to answer a more important question:

> **What conclusion is best supported by the available evidence?**

This week included investigations that reached different conclusions:

| Lab | Investigation | Final Assessment |
|---|---|---|
| Lab 01 | SVCHOST Dump Creation Alert Triage | False Positive / Benign Activity — High Confidence |
| Lab 02 | RDP Successful RemoteInteractive Logon Investigation | Benign Positive — Moderate Confidence |
| Lab 03 | Malware Prevention & Process Lineage Investigation | True Positive — Prevented Malware Activity — High Confidence |

Together, these investigations demonstrate an important part of SOC work: similar-looking alerts can require very different analytical approaches and may lead to completely different outcomes.

---

# 🎯 Week 19 Objectives

The main objectives of Week 19 were to strengthen my ability to:

- Treat security alerts as investigative starting points rather than conclusions
- Validate detection logic before making classification decisions
- Separate alert wording from underlying endpoint telemetry
- Interpret Windows authentication events
- Investigate RemoteInteractive RDP logons
- Analyze file creation and deletion activity
- Reconstruct endpoint process lineage
- Review command-line arguments for execution context
- Trace suspicious activity back to its original execution source
- Investigate malware prevention events
- Validate whether malicious activity was detected, blocked, or successfully executed
- Use file hashes as investigation and threat hunting artifacts
- Review related alerts and surrounding endpoint activity
- Interpret negative evidence carefully
- Compare competing hypotheses
- Assign defensible alert classifications
- Document confidence levels and limitations
- Recommend proportional response actions

---

# 🧠 Investigation Philosophy

The investigations in this week followed several core principles.

## 1. Alerts Are Hypotheses

An alert tells the analyst:

> Something happened that matched detection logic.

It does not automatically tell the analyst:

> A confirmed compromise occurred.

The analyst must determine what actually happened.

---

## 2. Context Changes Meaning

The same process, file extension, authentication event, or network connection can have very different meanings depending on context.

For example:

```text
.DMP File
```

could be associated with:

- Application crash diagnostics
- Troubleshooting
- Forensic collection
- Windows maintenance
- Credential dumping
- Malicious process memory collection

Similarly:

```text
Windows Event ID 4624
Logon Type 10
```

confirms successful RemoteInteractive authentication, but does not automatically establish whether the access was authorized or malicious.

---

## 3. Process Names Are Not Conclusions

A process should not be classified based only on its name.

A stronger investigation reviews:

```text
Process Name
      +
Executable Path
      +
Command Line
      +
Parent Process
      +
Grandparent Process
      +
User Context
      +
File Activity
      +
Network Activity
      +
Related Alerts
      +
Timeline Context
```

---

## 4. Event Semantics Matter

The difference between:

```text
File Creation
```

and:

```text
File Deletion
```

can completely change the interpretation of an alert.

The same applies to:

- attempted execution versus successful execution
- detection versus prevention
- authentication failure versus success
- network connection attempt versus completed communication

Understanding the actual event action is essential.

---

## 5. Negative Evidence Must Be Described Carefully

If an analyst searches an alert dataset and finds no additional alerts, the correct conclusion is:

> No additional matching alerts were identified within the searched dataset, timeframe, and investigation scope.

The correct conclusion is not:

> Nothing else happened.

Security investigations should clearly distinguish between:

- evidence of absence
- absence of evidence
- telemetry limitations
- search scope limitations

---

## 6. Confidence Should Match the Evidence

A good investigation does not only provide a classification.

It should also communicate confidence.

Examples:

```text
False Positive / Benign Activity
Confidence: High
```

```text
Benign Positive
Confidence: Moderate
```

```text
True Positive – Prevented Malware Activity
Confidence: High
```

This makes the final decision easier to understand and defend.

---

# 🔬 Lab 01 — SVCHOST Dump Creation Alert Triage & Benign Activity Validation

---

## 🛡️ Investigation Overview

This lab focused on investigating a medium-severity Elastic Security alert titled:

```text
SVCHOST Dump creation attempt on endpoints
```

At first glance, the alert name suggested potentially suspicious process dumping behavior.

Process dump files can be associated with:

- Crash diagnostics
- Application troubleshooting
- Operating system maintenance
- Digital forensics
- Debugging
- Credential access
- Process memory collection
- Malicious post-compromise activity

Because a `.DMP` file does not establish malicious intent by itself, the investigation focused on validating the actual endpoint telemetry.

The alert involved:

```text
File Name:
svchost.DMP
```

```text
File Path:
C:\Users\DELL\AppData\Local\Temp\svchost.DMP
```

The process associated with the event was:

```text
C:\Windows\System32\cleanmgr.exe
```

The alert occurred on:

```text
Host:
desktop-il6ukq9
```

under:

```text
User:
DELL
```

The alert had:

```text
Severity: Medium
Risk Score: 47
Observed File Action: Deletion
```

The central investigative question was:

> **Did the evidence support malicious SVCHOST process dumping, or was the alert associated with legitimate Windows cleanup activity involving an existing dump artifact?**

The final assessment was:

> **False Positive / Benign Activity — High Confidence**

---

## 🎯 Investigation Objectives

The investigation focused on determining:

- What caused the alert?
- What file was involved?
- What was the actual file operation?
- Which process performed the activity?
- Was the process located in an expected Windows directory?
- What command-line arguments were used?
- What did the process lineage show?
- Was there evidence of suspicious dump tooling?
- Were there additional DMP-related alerts?
- Was there evidence of credential access activity?
- Was there evidence of malware execution?
- Was there evidence of suspicious follow-on activity?
- Which hypothesis best explained the observed behavior?

---

## 📊 Alert Summary

| Field | Value |
|---|---|
| Alert Name | SVCHOST Dump creation attempt on endpoints |
| Severity | Medium |
| Risk Score | 47 |
| Host | `desktop-il6ukq9` |
| User | `DELL` |
| Process | `cleanmgr.exe` |
| Process Path | `C:\Windows\System32\cleanmgr.exe` |
| File | `svchost.DMP` |
| File Path | `C:\Users\DELL\AppData\Local\Temp\svchost.DMP` |
| Observed Event Action | Deletion |
| Final Classification | False Positive / Benign Activity |
| Confidence | High |

---

## 🔄 Investigation Workflow

```text
Alert Review
      ↓
Detection Rule Review
      ↓
Alert Reason Analysis
      ↓
Event Action Validation
      ↓
Process Path Validation
      ↓
Process Lineage Analysis
      ↓
Command-Line Analysis
      ↓
Host-Specific DMP Alert Search
      ↓
Malicious vs Benign Hypothesis Comparison
      ↓
Final Classification
```

---

## 🔍 Investigation Process

### 1️⃣ Initial Alert Review

The investigation began by reviewing the alert in Elastic Security.

The alert identified:

```text
Process: cleanmgr.exe
File: svchost.DMP
User: DELL
Host: desktop-il6ukq9
```

The alert title initially raised concern because process dumping can be associated with credential access and other malicious behavior.

However, the associated process was:

```text
C:\Windows\System32\cleanmgr.exe
```

This created an alternative hypothesis:

> The alert may have been triggered by legitimate Windows cleanup activity involving an existing dump file.

No final classification was made at this stage.

The investigation continued.

---

### 2️⃣ Detection Rule Review

The detection rule was reviewed to understand why the alert was generated.

This step was important because detection rules identify patterns of interest, but the detection rule name does not automatically describe the complete meaning of the underlying activity.

The investigation separated three questions:

1. What behavior was the rule designed to detect?
2. What did the actual endpoint event show?
3. What security conclusion did the complete evidence support?

This prevented the investigation from automatically treating the phrase:

```text
Dump creation attempt
```

as proof that the observed process created a malicious memory dump.

---

### 3️⃣ Event Action Validation

One of the most important findings came from reviewing the actual event semantics.

The file involved was:

```text
C:\Users\DELL\AppData\Local\Temp\svchost.DMP
```

However, the observed event action was:

```text
Deletion
```

This materially changed the investigation.

The available evidence did not show `cleanmgr.exe` creating the dump file.

Instead, the telemetry showed:

```text
cleanmgr.exe
      ↓
Deletion of svchost.DMP
```

This became one of the strongest pieces of evidence supporting the benign activity hypothesis.

---

### 4️⃣ Process Path Validation

The executable path was:

```text
C:\Windows\System32\cleanmgr.exe
```

This path is consistent with the standard Windows Disk Cleanup utility.

However, the investigation did not stop there.

A legitimate process name and expected path do not automatically prove that activity is benign.

The investigation continued by reviewing:

- Process lineage
- Command-line arguments
- File action
- User context
- Host context
- Related alerts

---

### 5️⃣ Process Lineage Analysis

Elastic Process Analyzer was used to reconstruct the relevant process relationships.

The observed lineage included:

```text
services.exe
      ↓
svchost.exe
      ↓
cleanmgr.exe
      ↓
DismHost.exe
```

This process context was consistent with Windows service-driven maintenance and cleanup behavior.

The reviewed alert context did not show an obvious suspicious execution chain involving:

- ProcDump
- PowerShell dumping commands
- Rundll32 and `comsvcs.dll`
- Unknown dump utilities
- Suspicious unsigned binaries

The lineage supported the benign hypothesis.

---

### 6️⃣ Command-Line Analysis

The `cleanmgr.exe` command-line arguments were reviewed.

Observed arguments included:

```text
/autocleanstoragesense
```

and:

```text
/d C:
```

The execution was consistent with:

```text
C:\WINDOWS\system32\cleanmgr.exe /autocleanstoragesense /d C:
```

This was one of the strongest pieces of contextual evidence in the investigation.

The key question was not:

> Is `cleanmgr.exe` normally legitimate?

The better question was:

> What was this specific execution of `cleanmgr.exe` instructed to do?

The arguments supported automated storage cleanup activity targeting the C: drive.

When correlated with:

- the deletion event
- the Temp directory location
- the process lineage
- the absence of corroborating malicious evidence

the behavior was most consistent with legitimate cleanup activity.

---

### 7️⃣ DMP Alert Scope Validation

A host-specific search was performed:

```text
host.name : "desktop-il6ukq9" and file.extension : "DMP"
```

Within the searched Alerts data view and investigation scope, the search returned the current SVCHOST dump alert without revealing a broader pattern of matching DMP-related alerts.

This was useful negative evidence.

However, the finding was documented carefully.

The investigation supported:

> No additional matching DMP-related alerts were identified within the searched alert dataset and scope.

It did not prove:

> No other DMP activity ever occurred on the endpoint.

This distinction is important for accurate SOC documentation.

---

## ⚖️ Hypothesis Analysis

### Hypothesis A — Malicious Process Dumping

This hypothesis would be strengthened by evidence such as:

- ProcDump execution
- Suspicious PowerShell activity
- Rundll32 with `comsvcs.dll`
- Task Manager dump activity
- Unknown dump utilities
- Credential access alerts
- Suspicious parent processes
- Malware detections
- Follow-on execution
- Persistence
- Suspicious outbound network communication

The reviewed evidence did not provide sufficient corroborating support for these behaviors.

**Support Level: Weak**

---

### Hypothesis B — Legitimate Windows Cleanup Activity

This hypothesis was supported by:

- `cleanmgr.exe` performed the observed activity
- The executable path was under `C:\Windows\System32`
- The observed action was deletion
- The file was located under a Temp directory
- The command line contained `/autocleanstoragesense`
- The execution targeted the C: drive
- Process lineage was consistent with Windows maintenance behavior
- No corroborating malicious dump behavior was identified in the reviewed alert context

**Support Level: Strong**

---

## 📌 Evidence Chain

```text
Alert Triggered
      ↓
svchost.DMP Identified
      ↓
Actual Event Action = Deletion
      ↓
Responsible Process = cleanmgr.exe
      ↓
Expected System32 Path
      ↓
Storage Sense Cleanup Arguments
      ↓
Windows Maintenance Process Lineage
      ↓
No Corroborating Malicious Dump Evidence
      ↓
Benign Activity Best Fits Available Evidence
```

---

## ✅ Final Assessment

| Assessment Field | Result |
|---|---|
| Classification | False Positive / Benign Activity |
| Confidence | High |
| Escalation | Not supported by available evidence |
| Primary Explanation | Automated Windows storage cleanup |
| Malicious Dumping Evidence | Not identified in reviewed scope |

### Assessment Rationale

The alert involved `svchost.DMP`; however, the underlying telemetry showed a deletion event associated with:

```text
C:\Windows\System32\cleanmgr.exe
```

The process executed with arguments including:

```text
/autocleanstoragesense /d C:
```

which supported automated Windows storage cleanup behavior.

The process lineage was consistent with Windows maintenance activity, and the reviewed alert context did not provide corroborating evidence of malicious dump tooling, credential access behavior, malware execution, or suspicious follow-on activity.

The strongest evidence therefore supported:

> **False Positive / Benign Activity — High Confidence**

---

## 🧠 Key Lessons

### Alert Titles Must Be Validated

A detection name describes the pattern a rule is looking for.

It does not automatically prove malicious intent.

### Event Semantics Can Change the Investigation

The difference between:

```text
Creation
```

and:

```text
Deletion
```

was critical to this investigation.

### Command Lines Provide Intent Context

The arguments:

```text
/autocleanstoragesense /d C:
```

helped explain what the process was doing.

### Legitimate Processes Still Require Validation

The investigation did not close the alert simply because `cleanmgr.exe` is a Windows utility.

The conclusion came from correlated evidence.

### Good Triage Prevents Unnecessary Escalation

Strong SOC work includes identifying malicious activity, but it also includes confidently identifying benign activity when the evidence supports it.

---

## 🛠️ Skills Demonstrated

- Elastic Security alert triage
- Detection rule analysis
- EDR telemetry review
- File event interpretation
- Process path validation
- Process lineage reconstruction
- Command-line analysis
- KQL filtering
- Host-level scope review
- False-positive identification
- Hypothesis-driven investigation
- Negative evidence interpretation
- Confidence assessment
- Professional incident documentation

---

# 🔐 Lab 02 — RDP Successful RemoteInteractive Logon Investigation

---

## 🛡️ Investigation Overview

This lab focused on investigating and triaging a successful Remote Desktop Protocol authentication alert in Elastic Security.

The alert was titled:

```text
RDP - Successful Interactive Remote Logon Detected
```

The underlying Windows authentication activity involved:

```text
Windows Security Event ID: 4624
Logon Type: 10
```

Logon Type 10 represents:

```text
RemoteInteractive
```

This is commonly associated with Remote Desktop Protocol activity.

The investigation was not limited to confirming that an RDP authentication occurred.

The more important question was:

> **Did the successful remote authentication represent legitimate remote access, compromised credentials, lateral movement, or activity requiring escalation?**

The investigation followed an evidence-driven methodology.

The following factors were treated as investigative leads rather than automatic proof of compromise:

- Successful authentication
- External source IP
- Administrator account usage
- Source geolocation
- RemoteInteractive logon type

The final assessment was:

> **Benign Positive — Moderate Confidence**

The detection correctly identified successful RDP authentication activity. However, the available investigation telemetry did not provide sufficient corroborating evidence of malicious post-authentication behavior.

---

## 🎯 Investigation Objectives

The investigation focused on:

- Reviewing the initial RDP alert
- Understanding the detection rule logic
- Validating Windows Event ID 4624
- Confirming Logon Type 10
- Identifying the source IP
- Identifying the destination host
- Identifying the authenticated account
- Reviewing privileged account context
- Reviewing source geolocation
- Reviewing network organization information
- Pivoting on the source IP
- Pivoting on the user
- Pivoting on the host
- Reviewing authentication events
- Reviewing related alerts
- Reviewing process activity
- Reviewing network activity
- Searching for evidence of brute-force behavior
- Searching for suspicious post-authentication activity
- Comparing legitimate and malicious access hypotheses
- Making a defensible disposition

---

## 📊 Alert Summary

| Field | Value |
|---|---|
| Alert Name | RDP - Successful Interactive Remote Logon Detected |
| Severity | Medium |
| Risk Score | 47 |
| Event Category | Authentication |
| Event Action | Logged-in |
| Host | `workstation-01` |
| User | `Administrator` |
| Process | `C:\Windows\System32\svchost.exe` |
| Source IP | `142.114.44.227` |
| Source Country | Canada |
| Source Region | Ontario |
| Source Organization | Bell Canada |
| Windows Event ID | 4624 |
| Logon Type | 10 |
| Logon Process Name | User32 |
| Final Disposition | Benign Positive |
| Confidence | Moderate |

---

## 🧠 Why Successful RDP Logons Require Investigation

RDP is widely used for:

- Remote administration
- Help desk support
- Server maintenance
- Infrastructure management
- Remote troubleshooting
- Work-from-home access

However, attackers also use RDP for:

- Initial access
- Valid account abuse
- Lateral movement
- Persistence
- Remote execution
- Post-compromise access

A successful RDP event can therefore represent several possibilities:

```text
Legitimate Administration
```

```text
Expected Remote Support
```

```text
Unauthorized Use of Valid Credentials
```

```text
Brute Force Followed by Success
```

```text
Password Spraying Followed by Success
```

```text
Internal Lateral Movement
```

```text
External Access to Exposed RDP
```

The investigation needed to determine which explanation best matched the available evidence.

---

## 🔄 Investigation Workflow

```text
RDP Alert
      ↓
Detection Rule Review
      ↓
Event ID Validation
      ↓
Logon Type Validation
      ↓
User Attribution
      ↓
Host Identification
      ↓
Source IP Analysis
      ↓
Geolocation Enrichment
      ↓
Network Organization Review
      ↓
Timeline Investigation
      ↓
Source IP Pivot
      ↓
User Pivot
      ↓
Host Pivot
      ↓
Authentication Correlation
      ↓
Related Alert Review
      ↓
Process Activity Review
      ↓
Network Activity Review
      ↓
Hypothesis Comparison
      ↓
Final Disposition
```

---

## 🔍 Investigation Process

### 1️⃣ Detection Rule Analysis

The investigation began by reviewing the rule responsible for generating the alert.

The detection logic focused on:

```text
winlog.channel:"Security"
and event.code:"4624"
and winlog.event_data.LogonType:"10"
```

This established that the alert was designed to identify:

```text
Successful Windows Authentication
      +
RemoteInteractive Logon
```

The rule correctly identified activity that deserved investigation.

However, the rule did not determine whether the access was authorized.

That required additional context.

---

### 2️⃣ Authentication Event Validation

The underlying authentication event was reviewed.

The investigation confirmed:

```text
Event ID: 4624
```

which represents a successful Windows logon.

The event also showed:

```text
Logon Type: 10
```

which represents RemoteInteractive access.

This confirmed that the detection logic matched the underlying authentication telemetry.

At this stage, the investigation had confirmed:

> A successful RemoteInteractive authentication occurred.

It had not yet confirmed:

> The authentication was malicious.

---

### 3️⃣ Account Review

The authenticated account was:

```text
Administrator
```

The use of a privileged account increased the importance of the investigation.

Administrator account activity can have significant impact because privileged credentials may allow:

- Software installation
- Security control modification
- Service creation
- User creation
- Credential access
- Persistence
- Lateral movement
- Sensitive file access

However, privileged account usage alone is not proof of malicious behavior.

The investigation therefore continued into source, host, timeline, and surrounding activity.

---

### 4️⃣ Source IP Analysis

The source IP was:

```text
142.114.44.227
```

Available enrichment showed:

```text
Country: Canada
Region: Ontario
Organization: Bell Canada
```

Geolocation and organization information provided useful context.

However, geography alone was not treated as a classification decision.

An IP address from another country or region can be associated with:

- Legitimate travel
- VPN infrastructure
- Residential ISP access
- Remote workers
- Corporate remote access
- Compromised infrastructure
- Unauthorized access

The investigation therefore used enrichment as context rather than proof.

---

### 5️⃣ Timeline and Entity Investigation

The alert was reviewed in the context of:

- the source IP
- the Administrator account
- `workstation-01`
- surrounding authentication events
- related alerts
- process activity
- network activity

This helped determine whether the RDP event was part of a larger suspicious sequence.

The investigation focused on identifying behavior such as:

- repeated failed logons before success
- multiple account attempts
- suspicious process execution after authentication
- PowerShell activity
- command shell execution
- service creation
- persistence
- suspicious file activity
- suspicious network communication
- additional alerts from the same endpoint

The reviewed evidence did not provide sufficient corroborating support for a malicious post-authentication sequence.

---

## ⚖️ Hypothesis Analysis

### Hypothesis A — Compromised Credentials / Unauthorized RDP Access

This hypothesis would be strengthened by:

- Numerous failed logons before success
- Password spraying patterns
- Multiple accounts attempted from the same source
- Impossible travel evidence
- Unusual access time
- Suspicious process execution after authentication
- PowerShell or command shell abuse
- Service creation
- Credential dumping
- Persistence activity
- Malware execution
- Suspicious outbound communication

Within the reviewed investigation scope, sufficient corroborating evidence was not identified to confidently support this hypothesis.

---

### Hypothesis B — Legitimate Remote Administrative Access

This hypothesis was supported by:

- Successful authentication telemetry
- RemoteInteractive logon behavior
- No sufficiently corroborating malicious post-authentication behavior in the reviewed scope
- No investigation evidence strong enough to justify escalation as confirmed unauthorized access

However, the investigation did not have enough business context to classify the activity with high confidence.

Examples of missing contextual validation could include:

- Change ticket information
- Administrator work schedule
- Approved remote access records
- VPN logs
- Identity provider logs
- Asset owner confirmation

For this reason, the confidence level remained:

```text
Moderate
```

rather than:

```text
High
```

---

## ✅ Final Assessment

| Assessment Field | Result |
|---|---|
| Disposition | Benign Positive |
| Confidence | Moderate |
| Detection Validity | Detection functioned correctly |
| Authentication | Successful RemoteInteractive logon confirmed |
| Malicious Follow-On Evidence | Not sufficiently identified in reviewed scope |
| Escalation | Not supported by available evidence |

### Assessment Rationale

The alert accurately detected a successful RemoteInteractive authentication event involving:

```text
Event ID 4624
Logon Type 10
```

The event involved the Administrator account on `workstation-01` from source IP `142.114.44.227`.

The event deserved investigation because it involved:

- RDP activity
- successful authentication
- privileged account usage
- an external source IP

However, the reviewed telemetry did not provide sufficient corroborating evidence of:

- brute-force activity
- password spraying
- suspicious process execution
- malware execution
- credential access
- persistence
- malicious network behavior
- other clear post-authentication compromise activity

The detection was therefore considered valid, but the activity was not escalated as a confirmed incident based on the available evidence.

Final disposition:

> **Benign Positive — Moderate Confidence**

---

## 🧠 Key Lessons

### A Successful Authentication Is Not the End of the Investigation

The analyst must determine what happened:

```text
Before Authentication
During Authentication
After Authentication
```

### Privileged Account Usage Raises Risk but Does Not Prove Compromise

Administrator activity deserves careful investigation, but classification must still be evidence-based.

### Geolocation Is Context, Not Proof

Country and ISP information can help guide an investigation, but should not be treated as a final verdict.

### Post-Authentication Behavior Is Critical

A successful login becomes more concerning when followed by:

- PowerShell
- command shell activity
- credential dumping
- service creation
- persistence
- suspicious network communication

### Business Context Matters

Technical telemetry can show that access occurred.

Business context may be required to confirm whether the access was expected.

---

## 🛠️ Skills Demonstrated

- Windows authentication investigation
- RDP alert triage
- Event ID 4624 analysis
- Logon Type 10 interpretation
- Privileged account investigation
- Source IP analysis
- Geolocation enrichment
- Network organization enrichment
- Elastic Timeline analysis
- Entity pivoting
- Authentication correlation
- Related alert review
- Hypothesis testing
- Evidence-based disposition
- Confidence assessment
- Investigation limitation documentation

---

# 🦠 Lab 03 — Malware Prevention Alert, Process Lineage & Endpoint Response Investigation

---

## 🛡️ Investigation Overview

This lab focused on investigating a high-severity Malware Prevention Alert in Elastic Security.

The alert was connected to a suspicious file creation attempt involving:

```text
setup.exe
      ↓
setup.tmp
      ↓
cls-lolzx_x86.exe
```

The investigation focused on answering:

- What caused the alert?
- Which endpoint was affected?
- Which user was involved?
- What process caused the activity?
- What launched that process?
- Where did the execution chain begin?
- What file was detected?
- What was the file hash?
- Did the file creation succeed?
- Was the malicious activity prevented?
- What additional activity followed?
- What could be proven?
- What still required additional investigation?

The investigation found that an installer process launched a temporary setup process that attempted to create a file identified by Elastic Endpoint as malicious.

Elastic Endpoint prevented the malicious file creation attempt.

The final classification was:

> **True Positive — Prevented Malware Activity**

**Confidence: High**

---

## 🎯 Investigation Objectives

The investigation focused on:

- Triaging a high-severity endpoint alert
- Identifying the affected user and host
- Reviewing the alert reason
- Identifying the suspicious file
- Reviewing the file path
- Collecting the SHA-256 hash
- Identifying the responsible process
- Identifying the parent process
- Tracing process ancestry
- Identifying the original installer source
- Reviewing command-line arguments
- Reconstructing the execution chain
- Reviewing related process activity
- Reviewing prevention evidence
- Reviewing quarantine-related information
- Determining whether the file creation succeeded
- Separating confirmed facts from assumptions
- Making an evidence-based classification
- Recommending additional response actions

---

## 🛠️ Tools and Technologies Used

| Tool / Technology | Investigation Purpose |
|---|---|
| Elastic Security | Alert investigation and endpoint analysis |
| Elastic Endpoint | Malware detection and prevention evidence |
| Elastic Defend | Endpoint protection context |
| Process Analyzer | Process relationship reconstruction |
| Elastic Timeline | Chronological event review |
| Windows Process Telemetry | Execution analysis |
| File Event Telemetry | File creation investigation |
| SHA-256 Analysis | IOC collection and hunting |
| Command-Line Analysis | Execution context validation |
| Process Tree Analysis | Parent-child and ancestry investigation |

---

## 📊 Alert Summary

| Field | Value |
|---|---|
| Alert Name | Malware Prevention Alert |
| Severity | High |
| Risk Score | 73 |
| Host | `desktop-jlklrc9` |
| User | `DELL` |
| Main Process | `setup.tmp` |
| Parent Process | `setup.exe` |
| Detected File | `cls-lolzx_x86.exe` |
| Event Action | Creation |
| Event Code | `malicious_file` |
| Operating System | Windows 11 Pro |
| Final Classification | True Positive — Prevented Malware Activity |
| Confidence | High |

The selected alert occurred at:

```text
July 6, 2026 at 12:21:49.020
```

---

## 🔄 Investigation Workflow

```text
Malware Prevention Alert
        ↓
Alert Detail Review
        ↓
Host and User Identification
        ↓
Suspicious File Analysis
        ↓
SHA-256 Collection
        ↓
Event Classification Review
        ↓
Responsible Process Investigation
        ↓
Parent Process Investigation
        ↓
Process Ancestry Reconstruction
        ↓
Original Installer Source Identification
        ↓
Command-Line Analysis
        ↓
Process Tree Review
        ↓
Prevention Validation
        ↓
Post-Execution Activity Review
        ↓
Impact Assessment
        ↓
Final Classification
```

---

## 🔍 Investigation Process

### 1️⃣ Initial Alert Review

The investigation began with a high-severity Malware Prevention Alert.

The alert had:

```text
Severity: High
Risk Score: 73
```

The initial evidence connected:

```text
Process: setup.tmp
Parent Process: setup.exe
Detected File: cls-lolzx_x86.exe
User: DELL
Host: desktop-jlklrc9
```

This created the initial investigation relationship:

```text
setup.exe
      ↓
setup.tmp
      ↓
Attempted Creation of cls-lolzx_x86.exe
```

The alert appeared suspicious, but additional investigation was needed before making a final classification.

---

### 2️⃣ Affected Host and User Identification

The activity was associated with:

```text
User:
DELL
```

and:

```text
Host:
desktop-jlklrc9
```

Identifying the affected user and endpoint created the starting point for scope analysis.

In a production environment, these entities could be used to investigate:

- Other alerts from the endpoint
- Other suspicious activity from the user
- Authentication activity
- Related file executions
- Similar activity across the environment
- Suspicious network connections

---

### 3️⃣ Suspicious File Analysis

The detected file was:

```text
cls-lolzx_x86.exe
```

The file path was:

```text
C:\Users\DELL\AppData\Local\Temp\is-1II3F.tmp\cls-lolzx_x86.exe
```

The location was relevant because temporary directories are commonly used by:

- Legitimate installers
- Software updaters
- Application unpacking processes
- Malware droppers
- Payload staging
- Defense evasion activity

The path alone was not used as proof of malicious behavior.

It was correlated with the endpoint malware classification, process chain, hash, and prevention result.

---

### 4️⃣ SHA-256 Collection

The investigation identified the SHA-256 hash:

```text
77b920a22f93f87d2624b58f729712ebacaf8b605437c2678ca4216a28f3b8b2
```

This hash is a useful investigation artifact because it can support:

- Threat intelligence enrichment
- SIEM searches
- EDR searches
- Environment-wide threat hunting
- IOC tracking
- Detection engineering
- Identification of similar activity on other endpoints

A production investigation could search:

```text
file.hash.sha256:
77b920a22f93f87d2624b58f729712ebacaf8b605437c2678ca4216a28f3b8b2
```

across available endpoint and SIEM telemetry.

---

### 5️⃣ Event Classification Review

The endpoint event contained fields including:

```text
event.action: creation
```

```text
event.category: malware
```

```text
event.category: intrusion_detection
```

```text
event.category: file
```

```text
event.code: malicious_file
```

These fields showed that the endpoint security product classified the event as malicious file activity.

This strengthened confidence that the alert represented a real security event rather than only a generic suspicious process pattern.

---

### 6️⃣ Process Lineage Analysis

Process lineage analysis was one of the most important parts of the investigation.

The process tree showed:

```text
winlogon.exe
      ↓
userinit.exe
      ↓
explorer.exe
      ↓
setup.exe
      ↓
setup.tmp
```

Additional `WerFault.exe` activity appeared after the temporary setup process.

The wider visible chain was approximately:

```text
winlogon.exe
      ↓
userinit.exe
      ↓
explorer.exe
      ↓
setup.exe
      ↓
setup.tmp
      ↓
WerFault.exe
```

The first portion:

```text
winlogon.exe
      ↓
userinit.exe
      ↓
explorer.exe
```

was consistent with an interactive Windows user session.

The investigation became more significant when the chain moved into:

```text
explorer.exe
      ↓
setup.exe
      ↓
setup.tmp
```

and then connected to the malicious file creation attempt.

---

### 7️⃣ Original Installer Source Analysis

The process activity was traced back to:

```text
D:\Games\Grand Theft Auto V [FitGirl Lolly Repack]\setup.exe
```

The source path added important context.

However, the final classification was not based only on the directory name.

The conclusion was based on the complete evidence chain:

```text
Interactive User Session
        ↓
Explorer Execution
        ↓
Installer Execution
        ↓
Temporary Setup Process
        ↓
Malicious File Creation Attempt
        ↓
Elastic Endpoint Detection
        ↓
Prevention
```

---

### 8️⃣ Command-Line Analysis

Command-line and process argument information helped connect the temporary process to the original installer.

Looking only at:

```text
C:\Users\DELL\AppData\Local\Temp\...\setup.tmp
```

would not provide the complete investigation story.

The investigation correlated:

- executable path
- command line
- process arguments
- parent process
- grandparent process
- user
- host
- execution time
- file activity
- process tree

This helped reconstruct how the activity developed.

---

### 9️⃣ Prevention Validation

One of the most important questions was:

> Did the malicious file creation succeed?

Timeline and prevention evidence showed that Elastic Endpoint prevented the malicious file creation attempt.

This changed the impact assessment.

The correct conclusion was not:

```text
False Positive
```

because the malicious activity was real.

The correct conclusion was:

```text
True Positive
      +
Prevented Activity
```

These two facts can exist together.

---

## 🧬 Reconstructed Execution Chain

```text
Windows Interactive Session
        ↓
winlogon.exe
        ↓
userinit.exe
        ↓
explorer.exe
        ↓
setup.exe
        ↓
setup.tmp
        ↓
Attempted Creation:
cls-lolzx_x86.exe
        ↓
Elastic Endpoint Detection
        ↓
Elastic Endpoint Prevention
        ↓
Additional WerFault.exe Activity Observed
```

---

## ⏱️ Investigation Timeline

### Stage 1 — Interactive User Session

A Windows user session was active through:

```text
winlogon.exe → userinit.exe → explorer.exe
```

### Stage 2 — Installer Execution

The user session was connected to:

```text
setup.exe
```

### Stage 3 — Temporary Setup Process

The installer launched:

```text
setup.tmp
```

from a temporary AppData directory.

### Stage 4 — Malicious File Creation Attempt

The temporary setup process attempted to create:

```text
cls-lolzx_x86.exe
```

### Stage 5 — Endpoint Detection

Elastic Endpoint classified the file event as malicious.

### Stage 6 — Prevention

The endpoint security control prevented the malicious file creation attempt.

### Stage 7 — Additional Process Activity

Additional `WerFault.exe` activity appeared after the setup process and was documented as an area requiring further investigation in a production environment.

---

## 🔎 Post-Execution Activity Review

The process tree showed two `WerFault.exe` processes after `setup.tmp`.

`WerFault.exe` is a legitimate Windows Error Reporting process.

For that reason, it was not classified as malicious based only on its process name.

However, the process tree showed related:

- File activity
- Network activity
- Registry activity

A deeper production investigation should review:

- Executable paths
- Digital signatures
- Command lines
- DNS requests
- Network destinations
- Files created
- Files accessed
- Registry modifications
- Timing relative to the malware alert

This is an important investigation principle:

> A legitimate process name should be validated through behavior and context.

---

## 📌 Key Indicators and Artifacts

### Alert Information

```text
Alert: Malware Prevention Alert
Severity: High
Risk Score: 73
```

### Endpoint Information

```text
User: DELL
Host: desktop-jlklrc9
```

### Process Information

```text
Main Process: setup.tmp
Parent Process: setup.exe
Additional Observed Process: WerFault.exe
```

### File Information

```text
File Name: cls-lolzx_x86.exe
Event Action: creation
Event Code: malicious_file
```

### SHA-256

```text
77b920a22f93f87d2624b58f729712ebacaf8b605437c2678ca4216a28f3b8b2
```

### Source Installer

```text
D:\Games\Grand Theft Auto V [FitGirl Lolly Repack]\setup.exe
```

---

## 📊 Key Findings

### Finding 1 — High-Severity Malware Alert

A high-severity Malware Prevention Alert was generated with a risk score of 73.

### Finding 2 — Affected User and Host Identified

The activity was connected to:

```text
User: DELL
Host: desktop-jlklrc9
```

### Finding 3 — Suspicious File Identified

The detected file was:

```text
cls-lolzx_x86.exe
```

### Finding 4 — Process Relationship Established

The investigation showed:

```text
setup.exe
      ↓
setup.tmp
      ↓
malicious file creation attempt
```

### Finding 5 — SHA-256 Collected

The file hash was collected for potential environment-wide hunting and threat intelligence enrichment.

### Finding 6 — Source Installer Identified

The process activity was traced back to the original installer source.

### Finding 7 — Prevention Confirmed

Elastic Endpoint prevented the malicious file creation attempt.

### Finding 8 — Process Tree Reconstructed

The investigation reconstructed the activity from the interactive user session through the installer and temporary setup process.

### Finding 9 — Additional Child-Process Activity Observed

`WerFault.exe` activity appeared after the setup process and was documented for further review.

---

## ⚖️ Detection vs Impact

One of the most important lessons from this investigation was understanding the difference between:

```text
Was the detection real?
```

and:

```text
What impact occurred?
```

The detection was a True Positive because the malicious file creation activity was real.

However, the endpoint security control prevented the specific malicious file creation attempt.

Therefore:

```text
Detection: True Positive
```

and:

```text
Immediate Outcome: Prevented
```

can both be correct.

---

## ✅ Final Classification

| Assessment Field | Result |
|---|---|
| Classification | True Positive — Prevented Malware Activity |
| Confidence | High |
| File Creation Attempt | Confirmed |
| Endpoint Malware Classification | Confirmed |
| Prevention | Confirmed |
| Further Scope Review | Recommended |

### Classification Rationale

The classification was supported by multiple evidence sources:

- High-severity Malware Prevention Alert
- `malicious_file` event classification
- Specific attempted file creation
- Identified suspicious file name
- SHA-256 hash
- Parent-child process relationships
- Source installer path
- Command-line context
- Process tree evidence
- Quarantine-related information
- Endpoint prevention result

The investigation did not rely on one alert field or one piece of evidence.

The final decision was based on an evidence chain.

---

## 🚨 Impact Assessment

The available evidence showed that the malicious file creation attempt was prevented.

This reduced the immediate impact compared with a scenario where the file was successfully created and executed.

However:

> **Successful prevention does not automatically prove that the endpoint is completely clean.**

A complete incident response investigation should still review:

- Earlier installer activity
- Other files created by the installer
- Additional hashes
- Persistence mechanisms
- Scheduled tasks
- New services
- Registry Run keys
- PowerShell activity
- Command shell activity
- Credential access attempts
- Suspicious network communication
- Similar alerts on other endpoints

The available evidence supported a prevented malware event.

Additional investigation would still be appropriate in a production environment.

---

## 🛡️ Recommended Response Actions

If this event occurred in a production environment, reasonable response actions would include:

1. Confirm that the malicious file remains blocked or quarantined.
2. Search the environment for the SHA-256 hash.
3. Search for `cls-lolzx_x86.exe` across available telemetry.
4. Identify other endpoints that executed the same installer.
5. Review earlier alerts involving the affected endpoint.
6. Review the complete endpoint timeline before and after the alert.
7. Investigate `WerFault.exe` file, network, and registry activity.
8. Review common persistence locations.
9. Review recently created files in temporary directories.
10. Run a complete endpoint security scan.
11. Review DNS, proxy, firewall, and EDR network telemetry.
12. Remove untrusted software according to organizational policy.
13. Escalate if additional malicious activity is identified.

---

## 🧠 Key Lessons

### Alert Triage Must Continue Beyond the Alert Name

The alert showed that malware prevention occurred.

The investigation still needed to determine:

- what process caused the activity
- what file was involved
- what launched the process
- where the activity began
- whether prevention succeeded
- what happened afterward

### Process Lineage Can Explain the Entire Story

The process tree connected:

```text
User Session
      ↓
Installer
      ↓
Temporary Process
      ↓
Malicious File Creation Attempt
      ↓
Prevention
```

### File Hashes Are Valuable Investigation Artifacts

The SHA-256 hash can support:

- threat hunting
- IOC searches
- threat intelligence
- environment-wide scoping

### Prevention Does Not Eliminate the Need for Investigation

A blocked payload may be only one part of a larger execution chain.

The analyst still needs to investigate the source and surrounding activity.

---

## 🛠️ Skills Demonstrated

- SOC alert triage
- Elastic Security investigation
- Elastic Endpoint analysis
- Malware prevention investigation
- EDR telemetry analysis
- File event investigation
- Process lineage reconstruction
- Parent-child process analysis
- Process ancestry analysis
- Process tree investigation
- Timeline reconstruction
- Command-line analysis
- File path analysis
- SHA-256 collection
- IOC identification
- Quarantine validation
- Prevention validation
- True Positive classification
- Impact assessment
- Response recommendation development
- Evidence-based incident investigation

---

# 📊 Cross-Lab Investigation Comparison

| Investigation Area | Lab 01 | Lab 02 | Lab 03 |
|---|---:|---:|---:|
| Alert Triage | ✅ | ✅ | ✅ |
| Detection Rule Review | ✅ | ✅ | ✅ |
| File Event Analysis | ✅ | — | ✅ |
| Authentication Analysis | — | ✅ | — |
| Process Lineage Analysis | ✅ | Context Review | ✅ |
| Command-Line Analysis | ✅ | Context Review | ✅ |
| Source IP Analysis | — | ✅ | — |
| Geolocation Enrichment | — | ✅ | — |
| Hash Collection | — | — | ✅ |
| Prevention Validation | — | — | ✅ |
| Hypothesis Comparison | ✅ | ✅ | ✅ |
| Confidence Assessment | ✅ | ✅ | ✅ |
| Response Recommendations | Scope-Based | Monitor / Validate | Hunt / Contain if Expanded |

---

# 🧠 Week 19 Key Analytical Lessons

## 1. The Alert Is the Beginning

Across all three investigations, the alert provided a starting point.

The investigation still needed to determine:

```text
What happened?
Who was involved?
What system was affected?
What process or authentication event was involved?
What does the surrounding evidence show?
What conclusion can actually be supported?
```

---

## 2. Similar Alerts Can Produce Different Outcomes

Week 19 included:

```text
False Positive / Benign Activity
```

```text
Benign Positive
```

and:

```text
True Positive – Prevented Malware Activity
```

This demonstrates why analysts should not force every investigation toward the same conclusion.

---

## 3. Process Context Is More Important Than Process Names

A process name should be investigated through:

```text
Executable Path
      +
Command Line
      +
Parent Process
      +
Grandparent Process
      +
User Context
      +
File Activity
      +
Network Activity
      +
Timeline
```

---

## 4. Authentication Success Does Not Automatically Mean Compromise

A successful RDP logon confirms that authentication occurred.

The analyst must still investigate:

- source
- account
- host
- surrounding failures
- post-authentication behavior
- business context

---

## 5. Prevention and Detection Are Different Questions

A True Positive can still represent prevented activity.

The analyst should separately determine:

```text
Was the activity malicious?
```

and:

```text
Did the malicious action succeed?
```

---

## 6. Negative Evidence Must Be Scoped

A search that returns no results should be documented precisely.

Good investigation language:

> No additional matching activity was identified within the searched dataset and investigation scope.

Overstated language:

> Nothing else happened.

---

## 7. Confidence Is Part of the Conclusion

A classification without confidence can hide uncertainty.

Week 19 used:

```text
High Confidence
```

when multiple evidence sources strongly supported the conclusion.

It used:

```text
Moderate Confidence
```

when the technical evidence supported a likely conclusion but additional business or identity context would improve certainty.

---

# 🛠️ Week 19 Technical Skills Demonstrated

## Security Operations

- Alert triage
- Alert validation
- Detection logic review
- Alert classification
- Confidence assessment
- Escalation reasoning
- Incident documentation

## Endpoint Investigation

- EDR telemetry analysis
- Process lineage reconstruction
- Process tree analysis
- Parent-child process review
- Command-line analysis
- File event analysis
- Prevention validation
- Quarantine evidence review

## Windows Security Analysis

- Event ID 4624 analysis
- Logon Type 10 interpretation
- RemoteInteractive authentication analysis
- Privileged account investigation
- Windows process context review

## Threat Hunting Foundations

- Host-based pivoting
- User-based pivoting
- Source IP pivoting
- SHA-256 IOC collection
- File name searches
- Scope validation
- Artifact prevalence thinking

## Analytical Skills

- Hypothesis development
- Signal-versus-noise analysis
- Benign-versus-malicious comparison
- Negative evidence interpretation
- Telemetry limitation awareness
- Impact assessment
- Evidence-based decision-making

---

# 🔧 Tools and Technologies

| Tool / Technology | Purpose |
|---|---|
| Elastic Security | Alert triage and investigation |
| Elastic Endpoint | Endpoint malware detection and prevention |
| Elastic Defend | Endpoint protection telemetry |
| Elastic Process Analyzer | Process lineage reconstruction |
| Elastic Timeline | Chronological evidence correlation |
| KQL | Alert and telemetry filtering |
| Windows Security Logs | Authentication event analysis |
| Windows Event ID 4624 | Successful logon investigation |
| EDR Telemetry | Process, file, and endpoint behavior analysis |
| SHA-256 Analysis | IOC collection and threat hunting |
| Process Tree Analysis | Execution chain reconstruction |
| Command-Line Analysis | Process intent and execution context |

---

# 📁 Suggested Repository Structure

```text
Week19_Triage_And_Investigations/
│
├── README.md
│
├── Lab01_SVCHOST_Dump_Alert_Triage/
│   ├── Technical_Analysis/
│   │   └── Week19_Lab1_SVCHOST_Dump_Alert_Triage_Writeup.md
│   ├── Case_Study/
│   │   └── Week19_Lab1_SVCHOST_Dump_Alert_Case_Study.md
│   └── Screenshots/
│
├── Lab02_RDP_RemoteInteractive_Logon_Investigation/
│   ├── Technical_Analysis/
│   │   └── Week19_Lab2_RDP_RemoteInteractive_Logon_Writeup.md
│   ├── Case_Study/
│   │   └── Week19_Lab2_RDP_Logon_Case_Study.md
│   └── Screenshots/
│
└── Lab03_Malware_Prevention_Investigation/
    ├── Technical_Analysis/
    │   └── Week19_Lab3_Malware_Prevention_Writeup.md
    ├── Case_Study/
    │   └── Week19_Lab3_Malware_Prevention_Case_Study.md
    └── Screenshots/
```

---

# 📈 Investigation Decision Matrix

| Lab | Initial Concern | Strongest Evidence | Final Decision |
|---|---|---|---|
| SVCHOST Dump Alert | Possible malicious process dumping | Deletion event + `cleanmgr.exe` + Storage Sense arguments + maintenance lineage | False Positive / Benign Activity |
| RDP RemoteInteractive Logon | Possible unauthorized remote access | Confirmed 4624 Type 10, but insufficient malicious follow-on evidence | Benign Positive |
| Malware Prevention Alert | Malicious file creation attempt | `malicious_file` classification + process chain + hash + prevention evidence | True Positive — Prevented |

---

# 🧾 Professional Week 19 Summary

Week 19 focused on developing stronger alert triage and endpoint investigation skills through three investigations with different outcomes.

The first investigation involved an SVCHOST dump-related alert that initially appeared suspicious. By reviewing the actual file action, responsible process, executable path, process lineage, and command-line arguments, I determined that the observed activity was most consistent with legitimate Windows cleanup behavior.

The second investigation involved successful RemoteInteractive authentication. I validated Windows Event ID 4624 and Logon Type 10, reviewed the Administrator account, source IP, geolocation context, host activity, and surrounding evidence. The detection was valid, but the reviewed telemetry did not provide enough corroborating evidence of malicious post-authentication activity to justify escalation as a confirmed incident.

The third investigation involved a high-severity malware prevention event. I traced the activity from the Windows user session through the installer process chain, identified the temporary process responsible for the malicious file creation attempt, collected the SHA-256 hash, reviewed the original installer source, reconstructed the process tree, and validated that Elastic Endpoint prevented the malicious file creation attempt.

Together, these projects strengthened my ability to move from:

```text
Alert
```

to:

```text
Evidence
```

to:

```text
Context
```

to:

```text
Defensible Decision
```

The most important lesson from Week 19 was:

> **A strong analyst does not investigate to prove the alert correct. A strong analyst investigates to determine which conclusion is best supported by the evidence.**

---

# ⚠️ Investigation Limitations

These investigations were completed in a controlled cybersecurity training environment.

In production environments, additional evidence could include:

- Identity provider logs
- VPN logs
- Firewall telemetry
- Proxy logs
- DNS logs
- Packet captures
- Full endpoint timelines
- Memory analysis
- Sandbox analysis
- Threat intelligence enrichment
- Change management records
- Help desk tickets
- Asset owner confirmation
- Privileged access management logs

The conclusions in these projects were limited to the available evidence and investigation scope.

This is intentional.

Professional security investigations should clearly separate:

```text
What is confirmed
```

from:

```text
What is suspected
```

and:

```text
What still requires validation
```

---

# 🚀 Career-Relevant Skills Demonstrated

These investigations demonstrate practical experience relevant to:

- SOC Analyst
- Cybersecurity Analyst
- Security Operations Analyst
- Incident Response Analyst
- Blue Team Analyst
- Junior Threat Hunter
- Endpoint Security Analyst
- Detection and Response Analyst

The work demonstrates hands-on practice with:

- Alert triage
- Windows security telemetry
- EDR investigation
- Authentication analysis
- Malware prevention validation
- Process lineage analysis
- File event investigation
- Command-line analysis
- IOC collection
- Scope assessment
- Hypothesis-driven analysis
- Incident classification
- Confidence-based decision-making
- Security documentation

---

# 📚 Final Learning Reflection

Week 19 reinforced that security investigation is not about reacting to suspicious words on a screen.

Terms such as:

```text
SVCHOST Dump
```

```text
Successful RDP Logon
```

and:

```text
Malware Prevention Alert
```

are starting points.

The analyst still needs to understand:

- the event semantics
- the process context
- the authentication context
- the timeline
- the execution chain
- the scope
- the supporting evidence
- the missing evidence
- the limitations of the telemetry

The three investigations showed three different outcomes:

```text
Benign Activity
```

```text
Valid Detection Without Sufficient Malicious Corroboration
```

```text
Confirmed Malicious Activity That Was Prevented
```

That difference is one of the most valuable lessons from this week.

Security Operations requires analysts to remain open to multiple explanations, test those explanations against the evidence, and document the final decision clearly enough that another analyst can understand and defend the reasoning.

---

# 👨‍💻 Author

**Fitzgerald Afari-Minta**

Cybersecurity | Security Operations | Incident Response | Threat Investigation | Endpoint Security | Detection & Response

---

## ⚠️ Disclaimer

All investigations documented in this repository were completed in controlled educational, lab, or cybersecurity training environments.

The purpose of this repository is to demonstrate:

- cybersecurity investigation methodology
- SOC alert triage
- incident response thinking
- endpoint investigation skills
- evidence correlation
- professional security documentation

No production systems or unauthorized environments were targeted during these projects.
