# Week 14 – Security Operations Center (SOC) Investigation Methodology & Elastic Security Incident Response

## Overview

Week 14 focused on developing the mindset, investigative methodology, and technical workflow expected of a modern Security Operations Center (SOC) analyst.

Rather than simply learning how to navigate Elastic Security or acknowledge security alerts, this week's projects emphasized **how professional analysts think during an investigation**. Throughout these investigations, every alert was treated as an investigative hypothesis that required validation through evidence collection, contextual analysis, process reconstruction, and analytical reasoning before reaching a final disposition.

Across multiple endpoint investigations, I practiced reconstructing process execution, interpreting detection logic, analyzing command-line behavior, validating parent-child process relationships, reviewing user and host context, and distinguishing legitimate administrative activity from behavior that could indicate attacker techniques.

The primary objective was not to determine whether every alert was malicious, but to develop a repeatable investigation methodology that produces accurate, defensible, and evidence-supported conclusions regardless of the SIEM, EDR, or XDR platform being used.

This portfolio project combines three complementary investigations completed during Week 14:

- Elastic Security Discovery & Enumeration Investigation
- SOC Investigation Methodology & Evidence-Based Alert Validation Framework
- Elastic Security Suspicious PowerShell Download Investigation

Together, these projects demonstrate both the technical and analytical competencies required for effective incident triage and investigation within enterprise Security Operations Centers.

---

# Objectives

Throughout this week's investigations, I focused on developing the ability to:

- Perform structured SOC alert triage
- Validate security detections before escalating incidents
- Interpret Elastic Security detection rules
- Analyze Windows endpoint telemetry
- Investigate process execution and command-line activity
- Reconstruct complete process lineage
- Differentiate legitimate administrative behavior from malicious techniques
- Correlate evidence across multiple telemetry sources
- Apply MITRE ATT&CK concepts during investigations
- Develop repeatable investigation methodologies
- Document professional incident findings
- Produce evidence-based security assessments
- Reduce false positives through contextual analysis
- Improve analytical decision-making during incident response

---

# Technologies Used

### Security Platforms

- Elastic Security
- Elastic SIEM
- Elastic Detection Rules
- Elastic EQL
- Elastic Process Analyzer

### Endpoint & Windows

- Windows Endpoint Telemetry
- Windows Process Trees
- Windows Command-Line Utilities
- Windows Security Events
- Windows Services
- Windows Installer (MSI)

### Investigation Frameworks

- MITRE ATT&ATTCK Framework
- Security Operations Center (SOC) Methodology
- Incident Response Workflow
- Process Lineage Analysis
- Evidence Correlation
- Threat Hunting Methodology

---

# Skills Demonstrated

## Security Operations

- SOC Alert Triage
- Detection Validation
- Incident Investigation
- Security Event Analysis
- Endpoint Investigation
- Behavioral Analysis
- Threat Hunting Methodology
- Alert Prioritization
- Investigation Documentation
- Escalation Decision Making

## Endpoint Investigation

- Process Lineage Analysis
- Parent-Child Process Correlation
- Process Tree Reconstruction
- Windows Service Analysis
- Windows Administrative Tool Investigation
- Command-Line Analysis
- Process Execution Validation
- Endpoint Telemetry Analysis

## Analytical Skills

- Evidence-Based Decision Making
- Context-Driven Investigation
- Timeline Reconstruction
- Root Cause Analysis
- Confidence Assessment
- Critical Thinking
- Multi-Source Evidence Correlation
- Signal-versus-Noise Analysis
- False Positive Validation
- Investigation Planning

---

# Investigation Philosophy

One of the primary goals of this week's work was developing a professional investigation methodology that can be applied consistently across different security platforms.

Rather than assuming that a high-severity alert automatically represents malicious activity, every investigation followed several guiding principles.

## Investigation over Assumption

Security alerts identify behavior requiring investigation—not proof of compromise.

Every alert should be validated before conclusions are reached.

---

## Evidence over Intuition

Every assessment should be supported by observable evidence.

Decisions should never rely solely on instinct or assumptions.

---

## Context over Individual Indicators

Individual events rarely provide enough information.

Instead, investigations should consider:

- User context
- Host context
- Asset role
- Process ancestry
- Execution timing
- Related activity
- Detection logic
- Operating system behavior

---

## Correlation over Isolation

Individual events often appear suspicious when viewed alone.

Correlating multiple sources of evidence provides significantly greater investigative confidence.

---

## Documentation over Memory

Every investigation should clearly document:

- What was observed
- What was validated
- What remains unknown
- Why conclusions were reached

---

## Defensible Conclusions

Professional SOC investigations should produce conclusions that can be defended using evidence rather than confidence.

---

# Standard Investigation Workflow

Throughout Week 14, every investigation followed a structured workflow.

## Phase 1 — Understand the Detection

Before reviewing endpoint activity, I examined:

- Detection rule
- Detection logic
- Alert reason
- MITRE ATT&CK mapping
- Alert severity
- Risk score
- Timestamp
- Host
- User
- Asset information

Understanding **why** a detection fired provides important investigative context before reviewing telemetry.

---

## Phase 2 — Establish Initial Questions

Rather than immediately assuming compromise, I developed investigative questions such as:

- Why did the detection trigger?
- What behavior was actually observed?
- Who initiated the activity?
- Which endpoint was affected?
- What process executed?
- What parent process launched it?
- Is the behavior expected?
- What evidence supports malicious activity?
- What evidence supports legitimate activity?

---

## Phase 3 — Analyze Process Execution

Each investigation involved reconstructing complete process lineage.

Instead of evaluating isolated processes, I examined:

- Parent-child relationships
- Process ancestry
- Command execution
- Windows process lifecycle
- Interactive versus service execution
- Operating system behavior

This approach provides significantly more context than process names alone.

---

## Phase 4 — Command-Line Analysis

Process names rarely provide sufficient context.

Each investigation included detailed command-line review to determine:

- What action occurred
- Which arguments were supplied
- Intended functionality
- Administrative purpose
- Potential attacker objectives

---

## Phase 5 — Context Validation

Observed behavior was evaluated alongside:

- User account
- Host role
- Installation activity
- Administrative operations
- Windows services
- Related alerts
- Endpoint telemetry

---

## Phase 6 — Evidence Correlation

Rather than relying on one observation, I correlated multiple sources of evidence including:

- Alert metadata
- Detection rule logic
- Process execution
- Parent-child relationships
- Process ancestry
- Command-line arguments
- Windows service behavior
- Endpoint telemetry

---

## Phase 7 — Evidence-Based Assessment

Rather than forcing binary conclusions, investigations documented:

- Confirmed observations
- Unknowns
- Assumptions
- Confidence level
- Recommended next steps

---

# Lab 1 — Discovery & Enumeration Investigation

## Scenario

Elastic Security generated a **High Severity Discovery or Enumeration** detection after observing execution of Windows administrative commands.

The investigation centered around determining whether the observed behavior represented attacker reconnaissance or legitimate Windows administration.

---

## Investigation Highlights

During the investigation I:

- Reviewed Elastic detection logic
- Examined alert metadata
- Validated severity and risk score
- Investigated host context
- Reviewed user context
- Analyzed process execution
- Reconstructed complete process lineage
- Examined Windows Installer activity
- Reviewed command-line arguments
- Correlated endpoint evidence

The investigation revealed execution of:

```text
net localgroup "Performance Monitor Users" "NT SERVICE\SplunkForwarder" /add
```

---

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

Rather than indicating attacker discovery activity, process lineage demonstrated software installation initiated through Windows Installer.

---

## Findings

The investigation found no supporting evidence of:

- Credential theft
- Privilege escalation
- Persistence
- Lateral movement
- Malware execution
- Unauthorized administrative activity

Instead, the evidence supported legitimate Splunk Forwarder installation.

---

## Final Assessment

The alert represented a legitimate administrative event associated with software installation.

Although the detection correctly identified behavior matching its rule logic, contextual evidence strongly supported benign activity.

---

# Lab 2 — SOC Investigation Methodology Framework

Rather than investigating a single alert, this project focused on developing a standardized investigation methodology applicable across SIEM, EDR, and XDR platforms.

The framework emphasized:

- Structured investigations
- Contextual reasoning
- Evidence correlation
- Confidence assessments
- Reproducible workflows
- Defensible conclusions

Key concepts included:

- Alerts represent investigative hypotheses
- Context determines meaning
- Evidence outweighs assumptions
- Documentation improves consistency
- Confidence levels communicate uncertainty professionally

This methodology now serves as the investigative foundation used throughout every future project.

---

# Lab 3 — Suspicious PowerShell Download Investigation

## Scenario

Elastic Security generated a high-severity PowerShell detection involving suspicious download activity.

The investigation focused on determining whether the observed PowerShell behavior represented legitimate administration, laboratory activity, or malicious execution.

---

## Investigation Highlights

The investigation included:

- Detection rule review
- Elastic EQL analysis
- Process Analyzer review
- Parent-child validation
- Command-line interpretation
- Endpoint telemetry review
- Threat Intelligence review
- Timeline reconstruction

Observed PowerShell activity included:

- Invoke-WebRequest
- OutFile
- Nested PowerShell execution

---

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

The investigation validated the observed execution chain before considering alternative explanations.

---

## Findings

Available telemetry showed:

- PowerShell downloading content
- Interactive execution
- No persistence
- No privilege escalation
- No credential theft
- No lateral movement
- No confirmed command-and-control activity

Rather than immediately classifying the activity as malicious, the investigation concluded that additional evidence would be required before confirming compromise.

---

# Investigation Methodology Applied Across All Labs

Every investigation throughout Week 14 consistently followed the same analytical workflow:

1. Understand the detection
2. Validate detection logic
3. Establish investigative questions
4. Review alert metadata
5. Analyze process lineage
6. Examine command-line activity
7. Correlate endpoint evidence
8. Evaluate alternative explanations
9. Assess confidence level
10. Produce a defensible conclusion

This repeatable methodology reduces confirmation bias while improving investigation quality.

---

# Key Lessons Learned

The most valuable lessons from Week 14 extended beyond individual Elastic Security investigations.

I learned that:

- Detection rules identify behaviors—not confirmed attacks.
- Alert severity should never replace investigation.
- Parent-child process relationships often provide more value than executable names.
- Command-line arguments frequently explain why a process executed.
- Context determines whether behavior is expected or suspicious.
- Evidence should always outweigh assumptions.
- Confidence levels improve professional communication.
- Unknowns should be documented rather than ignored.
- Well-documented investigations are reproducible and defensible.

---

# Evidence

The repository contains screenshots documenting the complete investigation workflow, including:

- Elastic Security Dashboard
- Detection Rules
- Detection Logic
- Alert Details
- Alert Reason
- Process Analyzer
- Parent-Child Relationships
- Process Trees
- Timeline Evidence
- Command-Line Analysis
- Investigation Findings
- Endpoint Context
- Security Assessment

Only screenshots directly supporting the investigation findings have been included.

---

# Overall Outcome

Week 14 represented a significant progression from learning security tools to developing professional investigative judgment.

Rather than simply acknowledging alerts, I practiced evaluating detections through structured evidence collection, contextual analysis, process reconstruction, and disciplined reasoning before reaching conclusions.

The combined investigations strengthened my ability to:

- Perform professional SOC investigations
- Validate behavioral detections
- Reconstruct endpoint activity
- Interpret Windows process execution
- Analyze command-line behavior
- Correlate multiple evidence sources
- Differentiate benign administrative behavior from attacker techniques
- Produce evidence-based incident assessments
- Document investigations suitable for Security Operations Center workflows

---

# Reflection

The greatest takeaway from this week's work was recognizing that effective SOC analysts are not defined by how many tools they know, but by how consistently they apply sound investigative judgment.

Security platforms continuously evolve, but disciplined analytical thinking remains transferable across SIEM, EDR, and XDR technologies.

Developing a repeatable investigation methodology has strengthened my confidence in approaching security incidents objectively, validating detections through evidence, and communicating findings professionally.

---

# Portfolio Note

This repository represents **Week 14** of my cybersecurity portfolio, focusing on Security Operations Center (SOC) investigations using Elastic Security.

Rather than showcasing isolated tool usage, these projects demonstrate a structured investigation methodology centered on evidence-based decision-making, contextual analysis, process lineage reconstruction, behavioral interpretation, and professional incident documentation.

The skills developed throughout this week are directly transferable across enterprise security platforms—including Elastic Security, Splunk Enterprise Security, Microsoft Defender XDR, Microsoft Sentinel, CrowdStrike Falcon, IBM QRadar, Google Chronicle, and other SIEM, EDR, and XDR solutions—and reflect the analytical thinking expected of modern SOC analysts, incident responders, and security engineers.
