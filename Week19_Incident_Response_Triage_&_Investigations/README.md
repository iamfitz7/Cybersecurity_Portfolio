# 🛡️ Week 19 — Security Alert Triage & Endpoint Investigations

## Evidence-Based SOC Triage, Authentication Review, Process Lineage Analysis, Malware Prevention Validation, and Defensible Alert Classification

---

## 📖 Overview

Week 19 focuses on one of the most important responsibilities in Security Operations: turning security alerts into clear, evidence-based investigation decisions.

Modern security tools can generate alerts for suspicious file activity, authentication behavior, process execution, malware detections, dump files, remote logons, and endpoint activity. However, an alert by itself is not proof that a system was compromised.

This week focused on learning how to move from:

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

The main goal was not to force every alert into a malicious conclusion.

The goal was to answer:

> What conclusion is best supported by the available evidence?

---

## 📁 Labs Included

| Lab | Investigation | Final Assessment |
|---|---|---|
| Lab 01 | SVCHOST Dump Creation Alert Triage | False Positive / Benign Activity — High Confidence |
| Lab 02 | RDP Successful RemoteInteractive Logon Investigation | Benign Positive — Moderate Confidence |
| Lab 03 | Malware Prevention & Process Lineage Investigation | True Positive — Prevented Malware Activity — High Confidence |
| Lab 04 | Repacked Game Installer Malware Prevention Investigation | True Positive — Malware Prevented / Blocked — High Confidence |

---

# 🎯 Week 19 Objectives

The objectives of Week 19 were to strengthen my ability to:

- Treat alerts as investigation starting points, not final conclusions
- Validate detection logic before making classification decisions
- Review endpoint telemetry carefully
- Interpret file creation, deletion, and prevention events
- Analyze Windows authentication events
- Investigate RemoteInteractive RDP logons
- Reconstruct process lineage
- Review parent-child process relationships
- Analyze command-line arguments
- Validate malware prevention activity
- Distinguish prevented malware from confirmed compromise
- Review suspicious installer chains
- Collect file hashes for additional hunting
- Use host, user, process, and hash pivots
- Compare benign and malicious hypotheses
- Assign accurate alert classifications
- Document confidence levels and limitations
- Recommend proportional response actions

---

# 🧠 Investigation Philosophy

## 1. Alerts Are Hypotheses

An alert means something matched detection logic.

It does not automatically mean:

```text
Confirmed Compromise
```

A strong analyst validates what happened before deciding what it means.

---

## 2. Context Changes Meaning

The same artifact can mean different things depending on context.

For example:

```text
.DMP file
```

could relate to:

- crash diagnostics
- troubleshooting
- memory dumping
- forensic collection
- credential dumping
- malicious process memory collection

The analyst must determine what the surrounding evidence supports.

---

## 3. Process Names Are Not Conclusions

A process should not be judged only by its name.

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

## 4. Prevention Does Not Equal Full Compromise

A malware prevention alert can be a true positive even if the endpoint security tool blocked the activity.

The analyst must separate:

```text
Was the activity malicious?
```

from:

```text
Did the malicious activity succeed?
```

---

# 🔬 Lab 01 — SVCHOST Dump Creation Alert Triage & Benign Activity Validation

---

## 🛡️ Investigation Overview

This lab focused on investigating a medium-severity Elastic Security alert titled:

```text
SVCHOST Dump creation attempt on endpoints
```

At first glance, the alert sounded suspicious because dump files can sometimes be associated with credential access, memory collection, debugging, or post-compromise activity.

The alert involved:

```text
File Name:
svchost.DMP
```

```text
File Path:
C:\Users\DELL\AppData\Local\Temp\svchost.DMP
```

The process associated with the activity was:

```text
C:\Windows\System32\cleanmgr.exe
```

The key question was:

> Did this alert show malicious dump creation, or did Windows Disk Cleanup delete an existing dump file?

The final assessment was:

```text
False Positive / Benign Activity — High Confidence
```

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
Event Action Validation
      ↓
Process Path Validation
      ↓
Process Lineage Analysis
      ↓
Command-Line Analysis
      ↓
DMP Alert Scope Review
      ↓
Hypothesis Comparison
      ↓
Final Classification
```

---

## 🔍 Key Investigation Steps

### 1. Alert Review

The alert identified a dump file named:

```text
svchost.DMP
```

Because dump files can sometimes contain sensitive memory contents, the alert required review.

However, the investigation did not accept the alert title as proof of malicious activity.

---

### 2. Event Action Validation

The most important field was the actual event action:

```text
Deletion
```

This changed the investigation significantly.

The available evidence showed that `cleanmgr.exe` deleted the `.DMP` file.

It did not show that `cleanmgr.exe` created a malicious memory dump.

---

### 3. Process Path Validation

The process path was:

```text
C:\Windows\System32\cleanmgr.exe
```

This path is consistent with the legitimate Windows Disk Cleanup utility.

However, the investigation did not stop there. A legitimate path is helpful context, but not enough by itself.

---

### 4. Process Lineage Analysis

The process lineage was consistent with Windows maintenance behavior:

```text
services.exe
      ↓
svchost.exe
      ↓
cleanmgr.exe
      ↓
DismHost.exe
```

No suspicious dump tools were identified in the reviewed process chain.

---

### 5. Command-Line Review

The command line included:

```text
/autocleanstoragesense /d C:
```

This supported automated Windows cleanup activity.

The evidence pointed toward Windows Storage Sense or Disk Cleanup removing a temporary dump file.

---

## ⚖️ Hypothesis Comparison

### Hypothesis A — Malicious Dump Creation

This would be supported by:

- Procdump execution
- suspicious PowerShell dumping commands
- Rundll32 with dump behavior
- unknown dump utilities
- credential access alerts
- suspicious outbound activity
- malware detections

The reviewed evidence did not support this strongly.

**Support Level: Weak**

---

### Hypothesis B — Legitimate Windows Cleanup Activity

This was supported by:

- `cleanmgr.exe`
- System32 path
- deletion action
- Temp directory target
- Storage Sense command-line arguments
- maintenance-style process lineage
- lack of corroborating malicious behavior

**Support Level: Strong**

---

## ✅ Final Assessment

| Field | Result |
|---|---|
| Classification | False Positive / Benign Activity |
| Confidence | High |
| Escalation | Not supported by available evidence |
| Primary Explanation | Windows cleanup activity |
| Malicious Dumping Evidence | Not identified in reviewed scope |

---

## 🧾 Professional Summary

This lab strengthened my ability to validate alert meaning by reviewing event action, process context, command-line arguments, and process lineage. Although the alert title suggested possible dump creation, the evidence showed a deletion event performed by Windows Disk Cleanup behavior. The best-supported conclusion was benign activity with high confidence.

---

# 🔐 Lab 02 — RDP Successful RemoteInteractive Logon Investigation

---

## 🛡️ Investigation Overview

This lab focused on investigating a successful Remote Desktop Protocol authentication alert in Elastic Security.

The alert was titled:

```text
RDP - Successful Interactive Remote Logon Detected
```

The underlying Windows authentication event involved:

```text
Event ID: 4624
Logon Type: 10
```

Logon Type 10 represents:

```text
RemoteInteractive
```

This is commonly associated with RDP activity.

The investigation focused on determining whether the activity represented:

- legitimate remote administration
- expected support activity
- compromised credentials
- unauthorized RDP access
- lateral movement
- activity requiring escalation

The final assessment was:

```text
Benign Positive — Moderate Confidence
```

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
| Final Disposition | Benign Positive |
| Confidence | Moderate |

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
Source IP Review
      ↓
Geolocation Enrichment
      ↓
Authentication Correlation
      ↓
Related Alert Review
      ↓
Process Activity Review
      ↓
Hypothesis Comparison
      ↓
Final Disposition
```

---

## 🔍 Key Investigation Areas

### 1. Detection Rule Review

The rule logic focused on:

```text
event.code: 4624
winlog.event_data.LogonType: 10
```

This confirmed that the alert was intended to detect successful RemoteInteractive logons.

---

### 2. Authentication Validation

The investigation confirmed:

```text
Event ID 4624 = Successful logon
Logon Type 10 = RemoteInteractive logon
```

This proved that RDP-style authentication occurred.

It did not prove that the access was malicious.

---

### 3. Account Review

The authenticated account was:

```text
Administrator
```

This increased concern because privileged accounts can perform high-impact actions.

However, administrator usage alone does not prove compromise.

---

### 4. Source IP Review

The source IP was:

```text
142.114.44.227
```

Available enrichment showed:

```text
Canada
Ontario
Bell Canada
```

This was useful context, but geolocation was not treated as proof of malicious activity.

---

### 5. Post-Authentication Review

The investigation looked for evidence such as:

- repeated failed logons before success
- brute force behavior
- multiple account attempts
- suspicious PowerShell activity
- command shell execution
- service creation
- credential dumping
- malware execution
- persistence
- suspicious outbound connections

The reviewed evidence did not provide enough support for confirmed malicious post-authentication activity.

---

## ✅ Final Assessment

| Field | Result |
|---|---|
| Disposition | Benign Positive |
| Confidence | Moderate |
| Detection Validity | Detection functioned correctly |
| Authentication | Successful RemoteInteractive logon confirmed |
| Malicious Follow-On Evidence | Not sufficiently identified in reviewed scope |
| Escalation | Not supported by available evidence |

---

## 🧾 Professional Summary

This lab strengthened my ability to investigate RDP authentication events without overreacting to successful logons. The detection correctly identified RemoteInteractive activity, but the reviewed evidence did not show enough malicious follow-on behavior to escalate it as a confirmed incident. The final classification was a benign positive with moderate confidence because additional business context would improve certainty.

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
- What file was detected?
- What was the file hash?
- Did file creation succeed?
- Was the malicious activity prevented?
- What additional review was needed?

The final classification was:

```text
True Positive — Prevented Malware Activity
```

Confidence:

```text
High
```

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
| Final Classification | True Positive — Prevented Malware Activity |
| Confidence | High |

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
Process Tree Review
        ↓
Prevention Validation
        ↓
Impact Assessment
        ↓
Final Classification
```

---

## 🔍 Key Investigation Areas

### 1. Suspicious File Review

The detected file was:

```text
cls-lolzx_x86.exe
```

The file was located in a temporary directory:

```text
C:\Users\DELL\AppData\Local\Temp\
```

Temporary locations are commonly used by installers, but also by malware droppers and payload staging behavior.

---

### 2. SHA-256 Collection

The SHA-256 hash was collected for future hunting and enrichment:

```text
77b920a22f93f87d2624b58f729712ebacaf8b605437c2678ca4216a28f3b8b2
```

This hash can be used for:

- SIEM searches
- EDR searches
- threat intelligence enrichment
- environment-wide hunting
- IOC tracking

---

### 3. Process Lineage Review

The process chain showed:

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
attempted creation of cls-lolzx_x86.exe
```

This showed the activity originated from an interactive user session and moved through an installer chain.

---

### 4. Prevention Validation

Elastic classified the file activity as malicious and prevented the file creation attempt.

This meant the activity was a true positive, but the immediate malicious action was blocked.

---

## ✅ Final Classification

| Field | Result |
|---|---|
| Classification | True Positive |
| Outcome | Prevented Malware Activity |
| Confidence | High |
| File Creation Attempt | Confirmed |
| Endpoint Malware Classification | Confirmed |
| Prevention | Confirmed |
| Further Review | Recommended |

---

## 🧾 Professional Summary

This lab strengthened my ability to investigate malware prevention events by tracing process lineage, validating file activity, collecting hashes, and separating true positive detection from confirmed compromise. The evidence showed malicious file creation activity that was prevented by Elastic Endpoint.

---

# 🧬 Lab 04 — Repacked Game Installer Malware Prevention Investigation

---

## 🛡️ Investigation Overview

This lab focused on a high-confidence Elastic Defend malware prevention alert involving a user-launched repacked game installer.

Elastic Defend prevented a suspicious file creation attempt involving:

```text
cls-lolzx_x86.exe
```

File path:

```text
C:\Users\DELL\AppData\Local\Temp\is-1IJ3F.tmp\cls-lolzx_x86.exe
```

The parent execution chain was:

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
cls-lolzx_x86.exe
```

The source installer was:

```text
D:\Games\Grand Theft Auto V [FitGirl Lolly Repack]\setup.exe
```

This mattered because cracked or repacked game installers are a common malware delivery path. A repacked installer spawning temporary executables that Elastic classifies as malware is highly suspicious.

The final classification was:

```text
True Positive — Malware Prevented / Blocked
```

This was not classified as confirmed full compromise because available evidence did not prove successful malware execution, persistence, C2, lateral movement, or data theft.

---

## 🎯 Investigation Objectives

The main goals of this investigation were to:

- Review the high-severity Malware Prevention Alert
- Identify the affected host and user
- Validate the suspicious file creation attempt
- Review file name, file path, and SHA-256 hash evidence
- Confirm whether Elastic Defend prevented or quarantined the file
- Analyze the parent-child process chain
- Identify the original installer source
- Determine whether the alert represented a true positive or false positive
- Pivot on the host, user, file hash, and installer process
- Determine whether additional activity appeared on the endpoint
- Make a careful final classification without overstating impact

---

## 📊 Alert Summary

| Field | Value |
|---|---|
| Alert Type | Malware Prevention Alert |
| Severity | High |
| Risk Score | 73 |
| Host | `desktop-jlklrc9` |
| User | `DELL` |
| Main Process | `setup.tmp` |
| Parent Process | `setup.exe` |
| Suspicious File | `cls-lolzx_x86.exe` |
| Suspicious File Path | `C:\Users\DELL\AppData\Local\Temp\is-1IJ3F.tmp\cls-lolzx_x86.exe` |
| Source Installer | `D:\Games\Grand Theft Auto V [FitGirl Lolly Repack]\setup.exe` |
| Final Classification | True Positive — Malware Prevented / Blocked |
| Confirmed Full Compromise | Not proven from available evidence |

---

## 🔄 Investigation Workflow

```text
Malware Prevention Alert
        ↓
Alert Overview Review
        ↓
File Evidence Review
        ↓
Quarantine / Prevention Validation
        ↓
Process Lineage Analysis
        ↓
Parent Installer Investigation
        ↓
Host Pivot
        ↓
User Pivot
        ↓
File Hash Pivot
        ↓
Installer Chain Pivot
        ↓
Impact Assessment
        ↓
Final Classification
```

---

## 🔍 Key Evidence Reviewed

### 1. Malware Prevention Alert Overview

The alert showed:

- Severity: `High`
- Risk Score: `73`
- Host: `desktop-jlklrc9`
- User: `DELL`
- Process: `setup.tmp`
- Suspicious file: `cls-lolzx_x86.exe`

Recommended screenshot:

```text
01_Malware_Prevention_Alert_Overview.png
```

---

### 2. Malicious File and Quarantine Evidence

The suspicious file was:

```text
cls-lolzx_x86.exe
```

Located at:

```text
C:\Users\DELL\AppData\Local\Temp\is-1IJ3F.tmp\cls-lolzx_x86.exe
```

Important fields reviewed:

- `file.name`
- `file.path`
- `file.hash.sha256`
- `file.Ext.quarantine_result`
- `file.Ext.quarantine_message`
- `file.Ext.quarantine_path`
- `file.Ext.malware_classification.score`
- `file.Ext.malware_classification.threshold`

This evidence mattered because it showed the file was classified as malicious and prevented/quarantined successfully.

Recommended screenshot:

```text
02_Malicious_File_And_Quarantine_Evidence.png
```

---

### 3. Process Lineage Analysis

The process lineage showed:

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
cls-lolzx_x86.exe
```

This showed the alert did not appear randomly. The activity came from a user session where a setup installer launched a temporary process that attempted to create a suspicious executable.

Recommended screenshot:

```text
03_Process_Lineage_Setup_Execution_Chain.png
```

---

### 4. Parent Installer Evidence

The parent installer was identified as:

```text
D:\Games\Grand Theft Auto V [FitGirl Lolly Repack]\setup.exe
```

Fields reviewed included:

- `process.parent.executable`
- `process.parent.name`
- `process.parent.hash.sha256`
- `process.parent.command_line`

This was a major part of the conclusion because cracked or repacked software is a common malware delivery path.

Recommended screenshot:

```text
04_Parent_Installer_FitGirl_Repack_Evidence.png
```

---

## 🧪 Investigation Pivots Performed

### Host Pivot

Affected host:

```text
desktop-jlklrc9
```

KQL searches:

```kql
host.name : "desktop-jlklrc9"
```

```kql
host.name : "desktop-jlklrc9" and event.category : "process"
```

```kql
host.name : "desktop-jlklrc9" and event.category : "network"
```

Purpose:

- Determine whether the endpoint had additional suspicious activity
- Review process and network activity
- Identify possible follow-up behavior

---

### File Hash Pivot

KQL search:

```kql
host.name : "desktop-jlklrc9" and file.hash.sha256 : "77b920a22f93f87d2624b58f729712ebacaf8b605437c2678ca4216a28f3b8b2"
```

Purpose:

- Determine whether the same file/hash appeared again
- Support endpoint scoping
- Identify repeated malicious file activity

---

### User Pivot

Affected user:

```text
DELL
```

KQL search:

```kql
user.name : "DELL" and host.name : "desktop-jlklrc9"
```

Purpose:

- Determine whether the same user had other related suspicious activity
- Review possible repeated risky behavior

---

### Installer Chain Pivot

KQL searches:

```kql
process.parent.name : "setup.exe" and host.name : "desktop-jlklrc9"
```

```kql
process.name : "setup.tmp" and host.name : "desktop-jlklrc9"
```

Purpose:

- Identify repeated alerts from the same installer chain
- Determine whether `setup.exe` or `setup.tmp` appeared in other suspicious activity

---

## 📌 Evidence Chain

```text
User Session
      ↓
explorer.exe
      ↓
setup.exe
      ↓
setup.tmp
      ↓
attempted creation of cls-lolzx_x86.exe
      ↓
Elastic malware classification
      ↓
quarantine / prevention evidence
      ↓
True Positive — Malware Prevented
```

---

## 📊 Key Findings

- Elastic Defend generated a high-severity Malware Prevention Alert.
- Risk score was `73`.
- The affected host was `desktop-jlklrc9`.
- The affected user was `DELL`.
- The suspicious process was `setup.tmp`.
- The parent process was `setup.exe`.
- The suspicious file was `cls-lolzx_x86.exe`.
- The suspicious file was created under a temporary directory.
- The source installer was:

```text
D:\Games\Grand Theft Auto V [FitGirl Lolly Repack]\setup.exe
```

- The process lineage showed a user-launched installer chain.
- Elastic classified the file as malicious.
- Elastic prevented/quarantined the malicious file creation attempt.
- No confirmed evidence showed successful malware execution, persistence, C2, lateral movement, or data theft.

---

## ✅ Final Classification

| Field | Value |
|---|---|
| Classification | True Positive |
| Outcome | Malware Prevented / Blocked |
| Confidence | High |
| Full Compromise Confirmed | No |
| Endpoint Review Recommended | Yes |
| User Education Recommended | Yes |
| Suspicious Installer Removal Recommended | Yes |

---

## 🧠 Analyst Reasoning

This alert should not be closed as benign.

The evidence showed that a repacked game installer launched temporary executable activity and attempted to create a file that Elastic classified as malicious.

At the same time, the investigation should not overstate the activity as a confirmed full compromise because the available evidence showed prevention/quarantine, not successful malware execution.

The most accurate conclusion is:

> Elastic Defend generated a high-severity malware prevention alert after a user-launched repacked game installer attempted to create an executable from a temporary directory. The file was classified as malicious by Elastic’s malware model and was successfully prevented/quarantined. No confirmed post-execution impact has been proven yet, but the source installer and execution chain are suspicious enough to classify this as a true positive prevention event.

---

## 🚨 Why This Activity Was Suspicious

This activity was suspicious because several indicators aligned:

```text
Repacked Game Installer
      ↓
Temporary Setup Process
      ↓
Suspicious Executable Creation
      ↓
Elastic Malware Classification
      ↓
Quarantine / Prevention
```

The strongest suspicious factors were:

- unofficial/repacked software source
- execution from a game repack installer path
- temporary executable creation
- malware classification by Elastic
- prevention/quarantine evidence
- clear process chain from installer to suspicious file

---

## ⚠️ What Was Not Proven

This investigation did **not** prove:

- successful malware execution
- confirmed endpoint compromise
- persistence
- credential theft
- command-and-control activity
- lateral movement
- data exfiltration

This distinction matters because professional SOC documentation should avoid overstating impact.

The evidence supported:

```text
True Positive — Malware Prevented / Blocked
```

not:

```text
Confirmed Full Compromise
```

---

## 🛡️ Recommended Response Actions

Recommended actions include:

1. Keep the alert classified as a true positive prevention event.
2. Confirm the malicious file was quarantined or blocked successfully.
3. Remove the suspicious repacked game installer from the endpoint.
4. Search the endpoint for the same SHA-256 hash.
5. Search for repeated `setup.exe` or `setup.tmp` activity.
6. Review the endpoint timeline before and after the alert.
7. Review related process, file, registry, and network activity.
8. Monitor for follow-up alerts from `desktop-jlklrc9`.
9. Educate the user about the risk of cracked or repacked software.
10. Run a full endpoint security scan.
11. Hunt for similar installer paths or hashes across other endpoints.
12. Escalate if additional post-execution activity is discovered.

---

## 📸 Screenshot Evidence

| Screenshot | Description |
|---|---|
| `01_Malware_Prevention_Alert_Overview.png` | Shows the high-severity Malware Prevention Alert, risk score, host, user, process, and suspicious file |
| `02_Malicious_File_And_Quarantine_Evidence.png` | Shows file name, file path, SHA-256 hash, malware classification, and quarantine/prevention fields |
| `03_Process_Lineage_Setup_Execution_Chain.png` | Shows process chain from user session to installer and temporary setup process |
| `04_Parent_Installer_FitGirl_Repack_Evidence.png` | Shows the suspicious parent installer path and parent process evidence |

---

## 🛠️ Skills Demonstrated

- Malware prevention alert triage
- Elastic Defend investigation
- Endpoint process lineage analysis
- Parent-child process analysis
- Temporary file execution review
- Malware classification validation
- Quarantine/prevention evidence review
- KQL pivoting
- Host-based investigation
- User-based investigation
- File hash pivoting
- Installer chain analysis
- True Positive classification
- Impact assessment
- Professional SOC documentation
- Evidence-based response recommendation

---

## 📚 Lessons Learned

### 1. Malware Prevention Still Requires Investigation

Even if a file is blocked, the analyst still needs to determine:

- what caused the attempt
- which user was involved
- where the file came from
- whether the endpoint shows other suspicious activity
- whether the same hash appears elsewhere

### 2. Prevention Does Not Equal Full Compromise

A true positive malware prevention event means malicious activity was real, but it does not automatically mean the attacker succeeded.

### 3. Source Context Matters

The source installer path was important because cracked or repacked software is commonly associated with malware delivery.

### 4. Process Lineage Tells the Story

The process chain explained how the suspicious file creation attempt occurred.

### 5. Accurate Classification Matters

The best classification was not benign and not confirmed compromise.

The best classification was:

```text
True Positive — Malware Prevented / Blocked
```

---

## 🧾 Professional Summary

This lab investigated a high-severity Elastic Defend malware prevention alert involving a suspicious file creation attempt from a repacked game installer. The investigation traced the activity from the Windows user session through `explorer.exe`, `setup.exe`, and `setup.tmp`, ending with an attempted creation of `cls-lolzx_x86.exe` in a temporary directory.

Elastic classified the file as malicious and prevented/quarantined the activity. The source installer path, temporary executable creation, malware classification, and process lineage all supported a true positive classification.

The final conclusion was that this was a **True Positive — Malware Prevented / Blocked** event. No confirmed full compromise was proven from the available evidence, but the endpoint should be reviewed, the suspicious installer should be removed, and the user should be educated about the risks of cracked or repacked software.

This project strengthened my ability to investigate malware prevention alerts, validate endpoint evidence, analyze process lineage, separate prevention from compromise, and make careful SOC classification decisions.

---

# 📊 Cross-Lab Investigation Comparison

| Investigation Area | Lab 01 | Lab 02 | Lab 03 | Lab 04 |
|---|---:|---:|---:|---:|
| Alert Triage | ✅ | ✅ | ✅ | ✅ |
| Detection Rule / Alert Review | ✅ | ✅ | ✅ | ✅ |
| File Event Analysis | ✅ | — | ✅ | ✅ |
| Authentication Analysis | — | ✅ | — | — |
| Process Lineage Analysis | ✅ | Context Review | ✅ | ✅ |
| Command-Line Analysis | ✅ | Context Review | ✅ | ✅ |
| Source IP Analysis | — | ✅ | — | — |
| Geolocation Enrichment | — | ✅ | — | — |
| Hash Collection | — | — | ✅ | ✅ |
| Prevention Validation | — | — | ✅ | ✅ |
| Quarantine Review | — | — | ✅ | ✅ |
| Host Pivoting | ✅ | ✅ | ✅ | ✅ |
| User Pivoting | Context Review | ✅ | ✅ | ✅ |
| Hypothesis Comparison | ✅ | ✅ | ✅ | ✅ |
| Confidence Assessment | ✅ | ✅ | ✅ | ✅ |
| Response Recommendations | Scope-Based | Monitor / Validate | Hunt / Review | Remove Installer / Hunt / Educate |

---

# 🧠 Week 19 Key Analytical Lessons

## 1. The Alert Is the Beginning

Every investigation started with an alert, but the alert was not treated as the conclusion.

The important questions were:

```text
What happened?
Who was involved?
What system was affected?
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

```text
True Positive — Prevented Malware Activity
```

```text
True Positive — Malware Prevented / Blocked
```

This shows why SOC analysts must avoid forcing every alert into the same category.

---

## 3. Event Semantics Matter

The difference between:

```text
creation
```

and:

```text
deletion
```

or between:

```text
malware executed successfully
```

and:

```text
malware was prevented
```

can completely change the final assessment.

---

## 4. Process Lineage Tells the Investigation Story

Process lineage helped explain how activity happened.

Examples:

```text
services.exe → svchost.exe → cleanmgr.exe
```

```text
winlogon.exe → userinit.exe → explorer.exe → setup.exe → setup.tmp
```

These chains helped separate normal behavior from suspicious behavior.

---

## 5. True Positive Does Not Always Mean Full Compromise

In the malware prevention labs, malicious activity was real, but prevention reduced the confirmed impact.

That distinction matters.

The correct language was:

```text
True Positive — Malware Prevented / Blocked
```

not:

```text
Confirmed Full Endpoint Compromise
```

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
- Parent-child process review
- Command-line analysis
- File event analysis
- Prevention validation
- Quarantine evidence review
- Installer chain analysis

## Windows Security Analysis

- Event ID 4624 analysis
- Logon Type 10 interpretation
- RemoteInteractive authentication analysis
- Privileged account review
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
├── Lab03_Malware_Prevention_Investigation/
│   ├── Technical_Analysis/
│   │   └── Week19_Lab3_Malware_Prevention_Writeup.md
│   ├── Case_Study/
│   │   └── Week19_Lab3_Malware_Prevention_Case_Study.md
│   └── Screenshots/
│
└── Lab04_Repacked_Game_Installer_Malware_Prevention/
    ├── Technical_Analysis/
    │   └── Week19_Lab4_Repacked_Game_Installer_Malware_Prevention_Writeup.md
    ├── Case_Study/
    │   └── Week19_Lab4_Repacked_Game_Installer_Case_Study.md
    └── Screenshots/
```

---

# 📈 Investigation Decision Matrix

| Lab | Initial Concern | Strongest Evidence | Final Decision |
|---|---|---|---|
| Lab 01 | Possible malicious SVCHOST dump activity | Deletion event + `cleanmgr.exe` + Storage Sense arguments + maintenance lineage | False Positive / Benign Activity |
| Lab 02 | Possible unauthorized RDP access | Confirmed 4624 Type 10, but insufficient malicious follow-on evidence | Benign Positive |
| Lab 03 | Malicious file creation attempt | `malicious_file` classification + process chain + hash + prevention evidence | True Positive — Prevented Malware Activity |
| Lab 04 | Repacked installer attempting malicious file creation | Repacked game installer path + temp executable + Elastic malware classification + quarantine/prevention evidence | True Positive — Malware Prevented / Blocked |

---

# 🧾 Professional Week 19 Summary

Week 19 focused on developing stronger alert triage and endpoint investigation skills through four investigations with different outcomes.

The first investigation involved an SVCHOST dump-related alert that initially appeared suspicious. By reviewing the actual file action, responsible process, executable path, process lineage, and command-line arguments, I determined that the observed activity was most consistent with legitimate Windows cleanup behavior.

The second investigation involved successful RemoteInteractive authentication. I validated Windows Event ID 4624 and Logon Type 10, reviewed the Administrator account, source IP, geolocation context, host activity, and surrounding evidence. The detection was valid, but the reviewed telemetry did not provide enough corroborating evidence of malicious post-authentication activity to justify escalation as a confirmed incident.

The third investigation involved a high-severity malware prevention event. I traced the activity from the Windows user session through the installer process chain, identified the temporary process responsible for the malicious file creation attempt, collected the SHA-256 hash, reviewed the source process, reconstructed the process tree, and validated that Elastic Endpoint prevented the malicious file creation attempt.

The fourth investigation focused on a repacked game installer that attempted to create a suspicious executable from a temporary directory. The source installer path, process lineage, malicious file classification, and quarantine/prevention evidence supported a true positive prevention decision without overstating the case as confirmed full compromise.

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

> A strong analyst does not investigate to prove the alert correct.  
> A strong analyst investigates to determine which conclusion is best supported by the evidence.

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

```text
Malware Prevention Alert
```

and:

```text
Repacked Installer Malware Block
```

are starting points.

The analyst still needs to understand:

- event semantics
- process context
- authentication context
- timeline
- execution chain
- scope
- supporting evidence
- missing evidence
- telemetry limitations
- business impact

The four investigations showed four important outcomes:

```text
Benign Activity
```

```text
Valid Detection Without Sufficient Malicious Corroboration
```

```text
Confirmed Malicious Activity That Was Prevented
```

```text
True Positive Malware Prevention Without Confirmed Full Compromise
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
