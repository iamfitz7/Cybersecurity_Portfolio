# Week 14 – Security Operations Center (SOC) Investigation Methodology & Elastic Security Incident Response

## Overview

Week 14 focused on developing professional SOC investigation skills through Elastic Security alert triage, process lineage analysis, detection validation, PowerShell investigation, and evidence-based decision making.

This README separates each lab into its own section so the full week reads like a complete portfolio case study while still making each individual investigation clear.

---

# Labs Completed

- Lab 1: Elastic Security Alert Triage and Process Lineage Investigation
- Lab 2: SOC Investigation Methodology & Evidence-Based Alert Validation Framework
- Lab 3: Elastic Security Suspicious PowerShell Download & Execution Analysis

---

# Lab 1 – Elastic Security Alert Triage and Process Lineage Investigation

## Lab Focus

This lab focused on investigating a high-severity Elastic Security alert related to Windows discovery and enumeration activity.

The main goal was to determine whether the alert represented malicious reconnaissance or legitimate administrative activity.

## Scenario

Elastic Security generated a high-severity alert for a rule named:

```text
Discovery or Enumeration
```

The alert involved execution of:

```text
net.exe
```

Because `net.exe` can be used by both administrators and attackers, the alert required deeper investigation before reaching a conclusion.

## What I Investigated

I reviewed:

- Detection rule details
- Alert severity
- Risk score
- Alert reason
- Affected host
- User context
- Process execution
- Parent-child process relationships
- Command-line arguments
- Process lineage
- Related endpoint evidence

## Key Evidence

```text
Host: workstation-01
User: NT AUTHORITY\SYSTEM
Primary Process: net.exe
Parent Process: cmd.exe
Ancestor Processes: msiexec.exe, services.exe, wininit.exe
```

The command executed was:

```text
net localgroup "Performance Monitor Users" "NT SERVICE\SplunkForwarder" /add
```

## Process Lineage

```text
wininit.exe
    ↓
services.exe
    ↓
msiexec.exe
    ↓
msiexec.exe
    ↓
cmd.exe
    ↓
net.exe
    ↓
net1.exe
```

## Analysis

The alert initially appeared suspicious because `net.exe` is commonly associated with Windows discovery, enumeration, and local group modification activity.

However, process lineage showed that the activity originated from `msiexec.exe`, which indicated Windows Installer activity.

The command-line arguments showed that the Splunk Forwarder service account was being added to the local Performance Monitor Users group. This is consistent with legitimate Splunk Forwarder installation or configuration activity because the service may require permission to collect Windows performance data.

## Malicious Indicators Checked

I looked for supporting evidence of:

- Credential theft
- Privilege escalation
- Malware execution
- Persistence
- Lateral movement
- Unauthorized administrator group changes
- Suspicious downloads
- Encoded PowerShell
- Scheduled task creation
- Registry modification

No supporting malicious indicators were observed.

## Outcome

The alert was assessed as likely benign administrative activity related to Splunk Forwarder installation or configuration.

In a production environment, I would verify the installation against change management records before closing the alert.

## Skills Demonstrated

- Elastic Security alert triage
- Process lineage analysis
- Parent-child process correlation
- Windows administrative command investigation
- Detection validation
- Endpoint investigation
- False positive validation
- Evidence-based decision making
- Incident documentation

---

# Lab 2 – SOC Investigation Methodology & Evidence-Based Alert Validation Framework

## Lab Focus

This lab focused on developing a repeatable SOC investigation methodology that can be applied across SIEM, EDR, and XDR platforms.

The goal was to move beyond tool usage and build a professional investigation framework based on evidence, context, and defensible conclusions.

## Purpose

The purpose of this lab was to create a structured workflow for approaching security alerts as investigative hypotheses rather than confirmed incidents.

This methodology can be applied to platforms such as:

- Elastic Security
- Splunk
- Microsoft Defender XDR
- Microsoft Sentinel
- CrowdStrike Falcon
- IBM QRadar
- Google Chronicle

## Investigation Principles

The framework emphasized:

```text
Investigation over assumption
Evidence over intuition
Context over isolated indicators
Correlation over individual events
Documentation over memory
Defensible conclusions over confident guesses
```

## Methodology Developed

The investigation framework included:

1. Understand the alert
2. Review the detection rule
3. Translate detection logic into plain language
4. Identify the investigative question
5. Establish host and user context
6. Reconstruct the timeline
7. Analyze process lineage
8. Review command-line activity
9. Correlate supporting evidence
10. Compare malicious and benign explanations
11. Document assumptions and unknowns
12. Assign confidence level
13. Produce final disposition
14. Recommend next actions

## Key Questions Used During Investigation

For each alert, I practiced asking:

- What exactly triggered this alert?
- What behavior did the detection rule identify?
- What evidence supports malicious activity?
- What evidence supports legitimate activity?
- What assumptions am I making?
- What telemetry is missing?
- What would change my conclusion?
- Is the activity expected for this user, host, or service?
- Does the process lineage make sense?
- Does the command line explain the behavior?

## Example Process Validation

This methodology was applied to Windows process behavior such as:

```text
System
smss.exe
winlogon.exe
userinit.exe
explorer.exe
cmd.exe
ipconfig.exe
```

Instead of treating command execution as suspicious by default, I practiced validating whether the process chain matched expected Windows behavior.

## Commands Evaluated

The framework included analysis of common Windows discovery utilities such as:

```text
ipconfig.exe
whoami.exe
net.exe
systeminfo.exe
```

These utilities can be used by both administrators and attackers, so the methodology focused on execution context instead of process names alone.

## Documentation Standards Created

Each investigation should document:

- Trigger
- Scope
- Timeline
- Evidence reviewed
- Evidence discovered
- Impact assessment
- MITRE ATT&CK mapping
- Confidence level
- Decision
- Recommended actions
- Unknowns
- Lessons learned

## Outcome

This lab produced a reusable SOC investigation framework focused on disciplined analysis, structured evidence collection, contextual reasoning, and professional documentation.

The framework strengthens future investigations by reducing assumptions, improving escalation quality, and making conclusions more defensible.

## Skills Demonstrated

- SOC investigation methodology
- Alert triage framework development
- Detection validation
- Timeline reconstruction
- Context-driven analysis
- Confidence assessment
- Escalation decision making
- Investigation documentation
- Analytical reasoning
- Security operations workflow development

---

# Lab 3 – Elastic Security Suspicious PowerShell Download & Execution Analysis

## Lab Focus

This lab focused on investigating suspicious PowerShell download and execution activity in Elastic Security.

The main goal was to determine whether the PowerShell behavior represented malicious execution, legitimate administration, or activity requiring additional investigation.

## Scenario

Elastic Security generated a high-severity alert for:

```text
Suspicious PowerShell Download and Execution Activity
```

PowerShell is frequently used by attackers because it can download files, execute commands, and perform actions without requiring additional malware. However, PowerShell is also a legitimate administrative tool, so the alert required careful validation.

## Detection Logic Reviewed

I reviewed Elastic detection logic involving:

```text
powershell.exe
Invoke-WebRequest
DownloadString
Invoke-Expression
IEX
Suspicious parent processes
```

The goal was to understand what behavior Elastic detected before making assumptions about intent.

## What I Investigated

I reviewed:

- Alert metadata
- Detection rule logic
- Alert reason
- Severity
- Risk score
- Host context
- User context
- Agent status
- Process lineage
- Parent-child relationships
- Command-line arguments
- Related alerts
- Host insights
- Threat Intelligence matches
- Available endpoint telemetry

## Process Lineage

```text
explorer.exe
    ↓
cmd.exe
    ↓
powershell.exe
    ↓
powershell.exe
```

## PowerShell Behavior Observed

The observed PowerShell activity included:

```text
Invoke-WebRequest
OutFile
Nested PowerShell execution
```

This indicated that PowerShell was used to download content and save it to a file location.

## Analysis

The alert correctly identified PowerShell behavior that can be associated with malicious activity.

However, the investigation separated two important concepts:

```text
A detection firing correctly does not automatically mean compromise occurred.
```

The process chain suggested interactive execution because the activity originated from `explorer.exe` and moved through `cmd.exe` before launching PowerShell.

This required additional context to determine whether the behavior was part of authorized administrative activity, lab activity, or suspicious user-driven execution.

## Malicious Indicators Checked

I reviewed available evidence for signs of:

- Persistence
- Privilege escalation
- Credential access
- Lateral movement
- Command-and-control activity
- Threat Intelligence matches
- Suspicious parent processes
- Additional related alerts
- Malware execution

No immediate evidence confirmed compromise based on the available telemetry.

## Outcome

The investigation concluded that the PowerShell activity required continued investigation rather than immediate confirmation of compromise.

The alert was valid because the behavior matched the detection rule, but the available evidence was not enough to conclusively determine malicious activity.

## Skills Demonstrated

- Elastic Security investigation
- PowerShell investigation
- Detection rule analysis
- Elastic EQL interpretation
- Command-line analysis
- Process lineage analysis
- LOLBAS recognition
- Endpoint investigation
- Behavioral detection analysis
- Evidence correlation
- Threat Intelligence review
- SOC triage methodology
- Evidence-based decision making

---

# Overall Week 14 Takeaways

Across all three labs, I strengthened my ability to investigate alerts using a disciplined SOC workflow.

The biggest takeaway was that security alerts should be treated as starting points for investigation, not final conclusions.

This week reinforced that professional analysts must be able to:

- Understand why alerts fired
- Validate detection logic
- Reconstruct process activity
- Analyze command-line behavior
- Correlate endpoint evidence
- Consider multiple explanations
- Document unknowns
- Assign confidence levels
- Make defensible decisions

---

# Final Reflection

Week 14 helped me shift from simply learning security tools to developing stronger investigative judgment.

Elastic Security provided the technical platform, but the most important skill developed was the ability to think like an analyst: carefully, logically, and evidence-first.

These labs strengthened my ability to distinguish between suspicious-looking activity and confirmed malicious behavior by validating context, process lineage, command intent, and supporting telemetry before making final conclusions.

---

---

# Lab 4 – Elastic Security Investigation: LSASS Memory Dump Creation (Credential Access)

## Lab Focus

This lab focused on performing a complete Security Operations Center (SOC) investigation into a high-severity **LSASS Memory Dump Creation** alert generated by Elastic Security.

The primary objective was to determine whether creation of an LSASS memory dump represented legitimate administrative troubleshooting or potential credential access activity requiring escalation.

Rather than assuming the alert represented malicious credential theft, I applied the structured investigation methodology developed throughout Week 14 by validating the detection through evidence collection, process lineage analysis, file activity review, registry analysis, contextual correlation, and evidence-based decision making.

---

## Scenario

Elastic Security generated a **High Severity** alert for:

```text
LSASS Memory Dump Creation
```

The alert mapped to:

```text
MITRE ATT&CK

Credential Access

OS Credential Dumping

LSASS Memory
```

Because the LSASS process stores Windows authentication material—including NTLM hashes, Kerberos tickets, and other credential data—the creation of an LSASS memory dump represents high-risk behavior that requires careful investigation before determining intent.

---

## What I Investigated

Throughout the investigation, I reviewed:

- Alert severity
- Risk score
- Detection reason
- MITRE ATT&CK mapping
- Host information
- User context
- Process metadata
- Parent-child process relationships
- Process lineage
- File creation events
- Dump file location
- Related file activity
- Registry modifications
- Process signatures
- Timeline activity
- Interactive execution context
- Additional endpoint telemetry

Rather than focusing on the alert title alone, I evaluated whether the surrounding evidence supported malicious credential dumping or legitimate administrative activity.

---

## Initial Alert Details

**Severity**

```text
High
```

**Risk Score**

```text
73
```

**Host**

```text
WIN-SOSRVGBV993
```

**User**

```text
Administrator
```

**Primary Process**

```text
Taskmgr.exe
```

**Process Path**

```text
C:\Windows\System32\Taskmgr.exe
```

**MITRE Technique**

```text
Credential Access

OS Credential Dumping

LSASS Memory
```

---

## Process Lineage

Using the Elastic Process Analyzer, I reconstructed the complete execution chain.

```text
explorer.exe
        ↓
Taskmgr.exe
        ↓
lsass.DMP created
```

The parent-child relationship indicated that Task Manager was launched from an interactive desktop session rather than from PowerShell, Command Prompt, scheduled tasks, or another suspicious parent process.

This execution context became an important factor during the investigation because it suggested direct user interaction rather than automated malware execution.

---

## File Activity Analysis

The investigation confirmed creation of the following dump files:

```text
lsass.DMP
```

located within:

```text
C:\Users\Administrator\AppData\Local\Temp\3\
```

Additional dump-related activity included:

```text
svchost.DMP
```

Rather than assuming malicious intent based solely on dump file creation, I evaluated whether the files were subsequently:

- Copied
- Renamed
- Archived
- Deleted
- Transferred
- Used by additional suspicious processes

Based on the available telemetry, no supporting evidence indicated credential extraction or exfiltration following dump creation.

---

## Registry Analysis

The investigation included review of registry modifications associated with Task Manager execution.

Observed modifications occurred under:

```text
Software\Microsoft\Windows\CurrentVersion\Internet Settings\ZoneMap
```

Modified values included:

- AutoDetect
- ProxyBypass
- UNCAsIntranet
- IntranetName

Rather than indicating persistence, these registry values relate to Windows Internet Settings and proxy configuration.

Importantly, no evidence was observed involving common persistence locations such as:

- Run
- RunOnce
- Services
- Winlogon
- Scheduled Tasks
- Image File Execution Options
- AppInit_DLLs

This demonstrated the importance of evaluating **what registry locations actually control**, rather than assuming every registry modification is suspicious.

---

## Supporting Evidence Reviewed

To strengthen the investigation, I evaluated whether additional indicators commonly associated with credential theft were present.

I specifically searched for evidence of:

- Mimikatz execution
- PowerShell abuse
- Encoded PowerShell
- Privilege escalation
- Credential extraction
- Lateral movement
- Persistence mechanisms
- Scheduled task creation
- Registry persistence
- Command-and-control activity
- File staging
- Archive creation
- Network exfiltration

None of these indicators were confirmed using the available endpoint telemetry.

---

## Analysis

One of the most important lessons from this investigation was distinguishing between **confirmed behavior** and **confirmed malicious intent**.

The investigation confirmed that:

- Task Manager created an LSASS memory dump.
- The executable was Microsoft-signed.
- The process originated from an interactive Administrator session.
- LSASS dump files were successfully created.
- Registry modifications were unrelated to persistence.

However, the available evidence did **not** confirm:

- Credential extraction
- Credential theft
- Malware execution
- Lateral movement
- Persistence
- Data exfiltration
- Attacker presence

This distinction is critical during professional incident response because security alerts identify suspicious behavior—not attacker intent.

---

## Investigation Outcome

Based on the available evidence, I concluded that the investigation confirmed **credential access behavior** through LSASS memory dump creation, but did not provide sufficient evidence to confirm malicious credential theft.

Because LSASS dump creation is inherently high-risk, the activity warrants escalation and validation of administrative authorization before final disposition.

In a production SOC environment, I would recommend:

- Verifying whether the dump was intentionally created for troubleshooting
- Confirming administrator authorization
- Reviewing change management records
- Determining whether the dump file was subsequently accessed or transferred
- Continuing investigation if additional suspicious activity emerged

---

## Skills Demonstrated

- Elastic Security investigation
- LSASS credential dumping investigation
- Credential Access analysis
- MITRE ATT&CK mapping
- Process lineage reconstruction
- Parent-child process correlation
- File activity analysis
- Registry investigation
- Windows process validation
- Interactive session analysis
- Endpoint telemetry interpretation
- Threat hunting methodology
- Evidence correlation
- Alert validation
- Incident response workflow
- Confidence-based decision making
- Professional incident documentation

---

## Key Lessons Learned

This investigation reinforced several important principles of professional SOC investigations:

- High-severity alerts identify behaviors requiring investigation rather than confirmed compromise.
- Legitimate Windows utilities can perform actions that closely resemble attacker techniques.
- LSASS memory dump creation is a high-risk event that always requires contextual validation.
- Registry modifications should be evaluated based on their function rather than their existence.
- Evidence should distinguish confirmed observations from assumptions.
- Professional incident reports should communicate both what is known and what remains unknown.

Most importantly, this investigation strengthened my ability to produce evidence-based conclusions that remain technically accurate, defensible, and appropriate for escalation within enterprise Security Operations Centers.

---

# Portfolio Note

This project is part of my cybersecurity portfolio documenting hands-on SOC investigations, endpoint analysis, alert triage, and incident response methodology.

The skills demonstrated in this week are transferable across Elastic Security, Splunk, Microsoft Defender XDR, Microsoft Sentinel, CrowdStrike Falcon, QRadar, Chronicle, and other enterprise SIEM, EDR, and XDR platforms.
