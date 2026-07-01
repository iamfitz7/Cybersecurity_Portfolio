# Week 14 – Incident Response Detection And Analysis 

Elastic Security Incident Response: Discovery & Enumeration Alert Investigation

## Overview

Security alerts are designed to draw attention to activity that may require investigation, but they do not automatically indicate malicious behavior. Many administrative actions performed by operating systems, software installers, and IT administrators can closely resemble attacker techniques. Because of this, analysts must investigate alerts carefully before deciding whether they represent a genuine security incident.

For this project, I investigated a high-severity **Discovery or Enumeration** alert generated in **Elastic Security**. The investigation focused on understanding why the alert was triggered, examining the surrounding process activity, validating the context of the execution, and determining whether the behavior was consistent with legitimate administrative activity or something requiring escalation.

Instead of relying only on the alert title or severity level, I analyzed the supporting evidence to build a complete picture of what occurred before making a final assessment.

---

# Objectives

During this investigation I aimed to:

- Investigate a high-severity Elastic Security detection
- Analyze process execution and parent-child relationships
- Examine command-line activity
- Review host and user context
- Differentiate expected administrative behavior from potentially malicious activity
- Practice evidence-based decision making
- Document findings in a structured incident investigation

---

# Technologies Used

- Elastic Security
- Elastic SIEM
- Elastic Detection Rules
- Windows Security Events
- Endpoint Process Telemetry
- Process Tree Analysis
- Windows Command-Line Utilities

---

# Skills Demonstrated

- Alert Investigation
- Security Event Analysis
- Detection Validation
- Process Lineage Analysis
- Parent-Child Process Investigation
- Endpoint Investigation
- Discovery & Enumeration Detection Analysis
- Evidence Collection
- Incident Documentation
- Security Decision Making
- Critical Thinking
- Technical Documentation

---

# Investigation Scenario

Elastic Security generated a **High Severity** alert for a detection rule named **Discovery or Enumeration**.

The alert identified execution of the Windows command:

```
net.exe
```

Because Windows administrative commands are frequently used by both legitimate administrators and attackers, additional investigation was required before determining whether the activity represented a security incident.

The objective was to determine whether the observed behavior was expected administrative activity or potentially malicious enumeration.

---

# Investigation Process

Rather than assuming the alert represented an attack, I approached the investigation by collecting additional context.

I reviewed:

- Detection rule information
- Alert metadata
- Host information
- User context
- Process execution
- Parent-child relationships
- Process lineage
- Alert reasoning
- Command execution details
- Detection rule logic

As additional evidence became available, I compared each observation against what would normally be expected during legitimate software installation versus malicious discovery activity.

---

# Key Evidence

The investigation identified:

**Alert Severity**

- High

**Risk Score**

- 73

**Host**

- workstation-01

**User**

- NT AUTHORITY\SYSTEM

**Primary Process**

- net.exe

**Parent Process**

- cmd.exe

**Ancestor Processes**

- msiexec.exe
- services.exe
- wininit.exe

The command executed was:

```text
net localgroup "Performance Monitor Users" "NT SERVICE\SplunkForwarder" /add
```

---

# Process Lineage

One of the most important observations was the complete process chain.

```
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

Instead of an unknown application launching administrative commands, the execution originated from Windows Installer during software installation.

This additional context significantly changed the interpretation of the alert.

---

# Analysis

Although the detection rule correctly identified behavior commonly associated with Windows discovery commands, the surrounding evidence suggested legitimate administrative activity.

The executed command added the Splunk Forwarder service account to the **Performance Monitor Users** group.

The parent-child relationship indicated the command originated during software installation rather than from an unexpected user process.

No evidence observed during the investigation suggested:

- Credential theft
- Privilege escalation
- Malware execution
- Persistence
- Lateral movement
- Unauthorized administrative activity

This demonstrates why alerts should never be evaluated in isolation.

---

# Investigation Outcome

Based on the available evidence, I determined that the observed activity was **most likely legitimate administrative behavior** associated with software installation.

Although the alert was appropriately generated by the detection rule, the surrounding context supported a benign explanation.

In a production environment, I would still verify that the software installation was authorized through change management or deployment records before closing the investigation.

---

# Lessons Learned

One of the biggest lessons from this project is that alert severity alone should never determine the outcome of an investigation.

Instead, analysts should:

- Collect supporting evidence
- Validate execution context
- Review process lineage
- Understand why the alert triggered
- Compare observations against expected system behavior
- Make decisions supported by evidence rather than assumptions

This investigation reinforced the importance of understanding **context**, not just individual events.

---

# Evidence

The repository contains selected screenshots documenting:

- Elastic Security Alerts Dashboard
- Discovery or Enumeration Detection Rule
- Detection Rule Logic
- Alert Details
- Process Lineage
- Alert Reason
- Alert Overview
- Process Information
- Investigation Evidence

Only screenshots that directly support the investigation findings have been included.

---

# Reflection

This project strengthened my understanding of how modern SIEM platforms generate behavioral detections and how analysts validate those detections using supporting evidence.

Rather than treating every alert as a confirmed incident, I learned the importance of investigating process relationships, command execution, user context, and surrounding system activity before reaching a conclusion.

Developing a structured investigation process has helped me become more confident in distinguishing between expected administrative behavior and activity that may require additional investigation.

---

## Portfolio Note

This project is part of my **Week 14 Incident Response, Detection & Analysis** portfolio series, where I document practical investigations performed using enterprise security tools. My goal is to continuously develop stronger analytical thinking, evidence-based decision making, and professional documentation skills that align with real-world Security Operations Center (SOC) workflows.
