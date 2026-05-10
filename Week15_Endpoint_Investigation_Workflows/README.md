# 🖥️ Week 15 — Endpoint Investigation Workflows

This section of the repository focuses on endpoint investigation methodology, incident response thinking, SIEM correlation, EDR-style analysis, and structured case handling workflows.

The projects in this week are designed to simulate how security analysts and incident responders investigate suspicious activity across systems, validate evidence, assess scope, and make reasonable containment and escalation decisions.

Rather than focusing only on alerts or individual tools, these labs focus on:

- investigation structure
- process behavior analysis
- observable tracking
- SIEM and endpoint correlation
- evidence validation
- blast radius thinking
- containment decisions
- documentation discipline

These projects are intentionally investigation-focused and workflow-focused to better reflect how real SOC and incident response environments operate.

---

# 📁 Labs Included

- Lab 01 — Multi-Host PowerShell Investigation
- Lab 02 — Elastic + Splunk Malicious Domain Investigation

---

# 🖥️ Lab 01 — Multi-Host PowerShell Investigation

---

## 🛡️ Overview

This lab simulates a multi-host endpoint investigation involving suspicious PowerShell execution across multiple Windows workstations. The focus of this project is incident response thinking, not tools.

Rather than simply reviewing an alert, this lab follows a realistic incident response workflow:

- Alert appears
- Case is created
- Observables are tracked
- Scope expands
- Host behavior is analyzed
- Blast radius is assessed
- Containment decisions are made
- Findings are documented

This lab represents a shift from alert review to incident handling, which is a core skill in SOC and Incident Response roles.

---

## 🎯 Objectives

This lab was designed to practice real-world incident response thinking:

- Transition from alert review to incident handling
- Build a structured case investigation workflow
- Track observables and affected systems
- Analyze endpoint process behavior
- Assess blast radius and potential spread
- Make containment decisions
- Document findings in a professional case format

---

## 🧠 Why This Lab Matters

PowerShell is widely used for legitimate administration.

However, it is also commonly abused in real-world attacks.

A weak investigation might stop at:

> “PowerShell looks suspicious.”

A stronger investigation asks:

- What launched PowerShell?
- Which users were involved?
- How many hosts were affected?
- Was behavior repeated?
- Did files get written to Temp?
- Did this expand the blast radius?
- Does this require containment?

This lab focuses on thinking like an incident responder, not just identifying suspicious activity.

---

## 🚨 Mock Incident Scenario

A SIEM alert reports suspicious PowerShell activity across multiple Windows workstations.

Initial findings:

- PowerShell launched from `cmd.exe`
- Activity observed across multiple hosts
- Multiple users involved
- Files written to Temp directory
- Suspicious URL observed
- Behavior repeated across systems
- Possible lateral movement behavior

This scenario simulates:

- Multi-host compromise investigation
- PowerShell abuse detection
- Observable tracking
- Process tree analysis
- Blast radius expansion
- Containment decision making

---

## 🔍 Investigation Workflow

### 1. Alert Intake & Case Creation

- Alert reviewed
- Case created
- Severity assigned (High)
- TLP classification applied (Amber)
- Initial facts documented

---

### 2. Observable Tracking

Tracked observables included:

### URLs
- hxxps://example-malicious-site[.]com/payload
- hxxps://example-clean-site[.]com/update

### Hostnames
- workstation1
- workstation2

### Users
- userA
- userB
- Administrator

### Files
- invoice.pdf.exe
- update.txt.exe
- suspicious.dmg

---

### 3. Investigation Scope Expansion

Key investigation areas:

- Multi-host correlation
- Parent-child process relationships
- File creation behavior
- Temp directory usage
- User account involvement
- Repeated execution patterns

---

### 4. Process Chain Analysis

Observed process chain:

```text
System → cmd.exe → powershell.exe → download → file creation
```

High-risk behaviors identified:

- PowerShell launched from `cmd.exe`
- `-nop`
- `-w hidden`
- `Invoke-WebRequest`
- `OutFile`
- File creation in Temp directory
- Repeated behavior across hosts

---

### 5. Blast Radius Assessment

Indicators that expanded severity:

- Multiple hosts affected
- Multiple users involved
- Repeated execution behavior
- Suspicious file creation
- Shared suspicious URL

These factors increased risk from a single-host alert to a potential multi-host incident.

---

### 6. Containment Decision

Decision:

**Isolate affected hosts and escalate**

Justification:

- Multi-host behavior
- Suspicious PowerShell execution
- File creation in Temp directories
- Potential lateral movement indicators

---

## 📊 Key Findings

- Suspicious PowerShell execution occurred across multiple hosts
- Parent-child chain showed `cmd.exe` launching `powershell.exe`
- Files written to Temp directory
- Suspicious URL identified
- Additional host impacted
- Potential lateral movement behavior observed
- Incident escalated for containment

---

## ⚠️ Risks & Limitations

- PowerShell may be legitimate administrative activity
- Multi-host behavior increases suspicion but does not confirm compromise
- Investigation based on simulated environment
- Real environments include additional telemetry and data sources

---

## 🛠️ Skills Demonstrated

- Incident response workflow development
- Case creation and severity classification
- Observable tracking and timeline building
- Process chain analysis
- Blast radius assessment
- Containment decision-making
- Documentation discipline

---

## 🎯 Why This Lab Is Valuable

This lab demonstrates:

- Incident response thinking
- Structured investigation workflow
- Realistic SOC/IR decision-making
- Professional documentation practices

This is not a tool-focused lab.

This is a thinking and workflow lab, which is often more valuable.

---

## 🧾 Professional Summary

This lab simulated a multi-host PowerShell investigation to practice incident response workflows. The project focused on transforming an alert into a structured case, tracking observables, analyzing endpoint behavior, expanding scope, and making containment decisions.

The investigation emphasized structured thinking, documentation, and proportional response — core skills used in real-world SOC and incident response environments.

---

# 🌐 Lab 02 — Elastic + Splunk Malicious Domain Investigation

---

## 🛡️ Project Overview

This project focuses on investigating suspicious domain activity using SIEM correlation, endpoint telemetry, threat intelligence validation, and memory dump analysis.

The investigation started from a malicious domain access alert and expanded into:

- process analysis
- user correlation
- endpoint investigation
- evidence validation
- memory analysis
- investigation documentation
- response decision-making

The goal of this project was not simply to review alerts, but to understand how multiple sources of evidence connect together during a realistic investigation workflow.

This project focused heavily on:

- investigation thinking
- signal vs noise analysis
- endpoint reasoning
- evidence validation
- structured investigative decision-making

---

## 🎯 Investigation Focus Areas

- SIEM log correlation
- Domain reputation validation
- Endpoint process investigation
- EDR telemetry analysis
- Suspicious process behavior
- Process ID validation
- Memory dump review
- Threat investigation workflow
- Case management documentation
- Incident response decision-making

---

## 🛠️ Tools & Technologies Used

| Tool | Purpose |
|---|---|
| Elastic Security | Endpoint investigation and process analysis |
| Splunk | SIEM correlation and log analysis |
| VirusTotal | Threat intelligence and domain validation |
| Hive / TheHive | Case management and investigation tracking |
| Windows Task Manager | Process and PID validation |
| Visual Studio Code | Memory dump string analysis |

---

## 🔍 Key Investigation Areas

### 1. Malicious Domain Validation

The investigation began with suspicious domain alerts involving:

- `br-icloud.com.br`
- `signin.eby.de.zukruygxctzmmqi.civpro.co.za`

VirusTotal was used to validate domain reputation and confirm that multiple vendors flagged the domains as malicious or phishing-related.

---

### 2. SIEM Correlation

Splunk searches were used to:

- identify affected users
- review DNS request activity
- correlate suspicious domains
- review process command-line activity
- identify suspicious hashes
- connect endpoint activity to users

This helped move the investigation beyond a simple alert and into evidence-based analysis.

---

### 3. Endpoint Investigation

Elastic Security telemetry was used to investigate suspicious process activity involving:

- `svchost.exe`
- unusual execution paths
- suspicious command-line behavior
- endpoint process relationships
- suspicious endpoint activity on `workstation-01`

The investigation focused on understanding whether the process behavior matched normal Windows activity or suspicious execution patterns.

---

### 4. Process Validation

Task Manager was used to validate:

- suspicious process IDs
- active `svchost.exe` instances
- administrator execution context
- running process behavior

This helped confirm endpoint evidence observed in SIEM and EDR telemetry.

---

### 5. Memory Dump Review

A memory dump was reviewed using Visual Studio Code to search for suspicious strings and indicators.

The review identified suspicious domain references connected to the investigation, helping support earlier findings from SIEM and endpoint telemetry.

---

## 🧠 Investigation Thinking & Decision-Making

One important part of this project was learning how to avoid jumping to conclusions too early.

For example:

- `svchost.exe` by itself is not automatically malicious
- many Windows systems normally run multiple `svchost.exe` processes
- process names alone are not enough evidence

The investigation focused on:

- execution path
- command-line behavior
- user association
- process IDs
- endpoint telemetry
- memory evidence
- supporting domain reputation

This helped separate normal system behavior from suspicious activity.

---

## 📚 Important Lessons Learned

### Signal vs Noise Matters

A large amount of endpoint and SIEM data can become distracting during investigations.

The important skill is identifying which evidence actually supports the concern.

---

### Validation Is Critical

The project reinforced that alerts should not automatically be trusted without validation.

Evidence was confirmed using:

- VirusTotal
- SIEM searches
- endpoint telemetry
- Task Manager validation
- memory dump analysis

---

### Multiple Data Sources Improve Investigations

No single tool provided the full picture.

The investigation required combining:

- threat intelligence
- SIEM logs
- endpoint telemetry
- process evidence
- memory artifacts

to understand the situation properly.

---

## ⚠️ Common Beginner Mistake

One common beginner mistake is assuming that a known Windows process automatically indicates compromise.

For example:

- seeing `svchost.exe`
- immediately assuming malware

A better approach is understanding:

- where the process executed from
- which user launched it
- the command-line behavior
- the associated network activity
- whether the process connects to other suspicious evidence

Context matters more than process names alone.

---

## 🔧 Recommended Improvements

Some realistic improvements for investigations like this include:

- better investigation checklists
- clearer SIEM correlation workflows
- stronger endpoint monitoring
- centralized documentation standards
- improved alert prioritization
- more structured evidence tracking

---

## 📸 Screenshots Included

### Case Management & Alert Review
- `01_Malicious_Domain_Case_Overview.png`
- `02_Hive_Investigation_Task_Creation.png`

### Threat Intelligence Validation
- `03_VirusTotal_Suspicious_Signin_Domain.png`
- `04_VirusTotal_BR_ICLOUD_Domain_Reputation.png`

### SIEM Correlation
- `05_Splunk_Malicious_Domain_Search_Query.png`
- `06_Splunk_Domain_User_Correlation_Statistics.png`
- `07_Splunk_User_Domain_Correlation_Results.png`
- `08_CrowdStrike_Process_Hash_Investigation_John_Doe.png`
- `09_Splunk_svchost_Hash_Analysis.png`
- `10_Splunk_BR_ICLOUD_Process_Correlation.png`
- `11_Splunk_Workstation_Correlation_Analysis.png`

### Endpoint Investigation
- `12_Elastic_Security_Process_Graph_View.png`
- `13_Elastic_Security_svchost_Process_Investigation.png`

### Process Validation
- `14_Task_Manager_PID_1148_Validation.png`
- `15_Task_Manager_PID_6192_Validation.png`

### Memory Dump Analysis
- `16_VSCode_Memory_Dump_String_Analysis.png`

### Case Documentation
- `17_Hive_Task_Log_Hash_Validation.png`
- `18_Hive_EDR_Investigation_Task_Log.png`
- `19_Final_Hive_Case_Status_and_Tasks.png`

---

## 🧭 MITRE ATT&CK Concepts Observed

| Technique | Description |
|---|---|
| T1059 | Command and Scripting Interpreter |
| T1071 | Application Layer Protocol |
| T1105 | Ingress Tool Transfer |
| T1204 | User Execution |

---

## 🛠️ Skills Demonstrated

- SIEM investigation workflows
- Endpoint telemetry analysis
- Threat intelligence validation
- Domain reputation investigation
- Process validation and PID analysis
- Memory artifact analysis
- Investigation documentation
- Signal vs noise analysis
- Evidence correlation across tools
- Incident response reasoning

---

## 🧾 Professional Summary

This project strengthened my understanding of how modern investigations connect SIEM data, endpoint telemetry, threat intelligence, and evidence validation into one investigation workflow.

The most valuable part of the project was learning how to think through uncertainty, identify meaningful signals, and make calm, reasonable decisions based on available evidence.

It also reinforced the importance of validating findings across multiple data sources instead of relying on a single alert or tool.
