# 🖥️ Week 15 — Endpoint Investigation Workflows

This section focuses on **endpoint investigation methodology**, **incident response thinking**, **SIEM correlation**, **EDR-style endpoint analysis**, **authentication review**, **malware execution investigation**, and **structured case documentation**.

The labs in this week are designed to show how SOC analysts and incident responders investigate suspicious activity across systems, validate evidence, connect multiple telemetry sources, assess scope, and make careful escalation or containment decisions.

Rather than focusing only on alerts or individual tools, these labs focus heavily on:

- Investigation structure
- Process behavior analysis
- Observable tracking
- SIEM and EDR correlation
- Authentication analysis
- Service creation review
- Remote execution indicators
- Malware execution analysis
- Signal vs noise analysis
- Blast radius thinking
- Documentation discipline
- Professional investigation workflows

These projects are intentionally workflow-focused to better reflect how real SOC and incident response environments operate.

---

## 📁 Labs Included

| Lab | Project |
|---|---|
| Lab 01 | Multi-Host PowerShell Investigation |
| Lab 02 | Elastic + Splunk Malicious Domain Investigation |
| Lab 03 | SMB & RDP Port Scanning / Lateral Movement Investigation |
| Lab 04 | Suspicious File Drop & Malware Execution Analysis |

---

# 🖥️ Lab 01 — Multi-Host PowerShell Investigation

## 🛡️ Overview

This lab simulates a **multi-host endpoint investigation** involving suspicious PowerShell execution across multiple Windows workstations.

The focus of this project is **incident response thinking**, not tool usage.

Instead of simply reviewing one alert, this lab follows a realistic investigation workflow:

- Alert appears
- Case is created
- Observables are tracked
- Scope expands
- Host behavior is analyzed
- Blast radius is assessed
- Containment decisions are made
- Findings are documented

This lab represents the shift from simply reviewing alerts to actually handling incidents.

---

## 🎯 Objectives

- Transition from alert review to incident handling
- Build a structured case investigation workflow
- Track observables and affected systems
- Analyze endpoint process behavior
- Assess blast radius and possible spread
- Make containment decisions
- Document findings in a professional case format

---

## 🧠 Why This Lab Matters

PowerShell is commonly used for legitimate administration, but it is also heavily abused in real-world attacks.

A weaker investigation might stop at:

> “PowerShell looks suspicious.”

A stronger investigation asks:

- What launched PowerShell?
- Which users were involved?
- How many hosts were affected?
- Was behavior repeated?
- Did files get written to Temp?
- Did this expand the blast radius?
- Does this require containment?

This project focuses on learning how incident responders think through uncertainty instead of immediately assuming compromise.

---

## 🚨 Mock Incident Scenario

A SIEM alert reports suspicious PowerShell activity across multiple Windows workstations.

Initial findings included:

- PowerShell launched from `cmd.exe`
- Activity observed across multiple hosts
- Multiple users involved
- Files written to Temp directories
- Suspicious URL observed
- Repeated behavior across systems
- Possible lateral movement behavior

This scenario simulates:

- Multi-host compromise investigation
- PowerShell abuse detection
- Observable tracking
- Process tree analysis
- Blast radius expansion
- Containment decision-making

---

## 🔍 Investigation Workflow

### 1️⃣ Alert Intake & Case Creation

- Alert reviewed
- Case created
- Severity assigned as High
- TLP classification applied as Amber
- Initial facts documented

### 2️⃣ Observable Tracking

#### URLs

- `hxxps://example-malicious-site[.]com/payload`
- `hxxps://example-clean-site[.]com/update`

#### Hostnames

- `workstation1`
- `workstation2`

#### Users

- `userA`
- `userB`
- `Administrator`

#### Files

- `invoice.pdf.exe`
- `update.txt.exe`
- `suspicious.dmg`

### 3️⃣ Investigation Scope Expansion

Key investigation areas:

- Multi-host correlation
- Parent-child process relationships
- File creation behavior
- Temp directory usage
- User account involvement
- Repeated execution patterns

### 4️⃣ Process Chain Analysis

Observed process chain:

```text
System → cmd.exe → powershell.exe → download → file creation
```

High-risk behaviors reviewed:

- PowerShell launched from `cmd.exe`
- `-nop`
- `-w hidden`
- `Invoke-WebRequest`
- `OutFile`
- File creation in Temp directory
- Repeated behavior across hosts

### 5️⃣ Blast Radius Assessment

Indicators that increased severity:

- Multiple hosts affected
- Multiple users involved
- Repeated execution behavior
- Suspicious file creation
- Shared suspicious URL

These factors increased risk from a single-host alert to a potential multi-host incident.

### 6️⃣ Containment Decision

**Decision:** Isolate affected hosts and escalate.

**Justification:**

- Multi-host behavior observed
- Suspicious PowerShell execution
- Temp directory file creation
- Potential lateral movement indicators
- Shared suspicious infrastructure

---

## 📊 Key Findings

- Suspicious PowerShell execution occurred across multiple hosts
- Parent-child chain showed `cmd.exe` launching `powershell.exe`
- Files were written to Temp directories
- Suspicious URL identified
- Additional host impact increased severity
- Potential lateral movement behavior required containment thinking

---

## ⚠️ Risks & Limitations

- PowerShell may still represent legitimate administrative activity
- Multi-host behavior increases suspicion but does not automatically confirm compromise
- Investigation is based on a simulated environment
- Real enterprise environments contain significantly more telemetry sources

---

## 🛠️ Skills Demonstrated

- Incident response workflow development
- Case creation and severity classification
- Observable tracking
- Timeline building
- Process chain analysis
- Blast radius assessment
- Containment decision-making
- Documentation discipline

---

## 🧾 Professional Summary

This lab simulated a multi-host PowerShell investigation to practice incident response workflows.

The project focused on transforming an alert into a structured case, tracking observables, analyzing endpoint behavior, expanding scope, and making containment decisions.

The investigation emphasized structured thinking, documentation, and proportional response — core skills used in real-world SOC and incident response environments.

---

# 🌐 Lab 02 — Elastic + Splunk Malicious Domain Investigation

## 🛡️ Project Overview

This project focused on investigating suspicious domain activity using:

- SIEM correlation
- Endpoint telemetry
- Threat intelligence validation
- Process investigation
- Memory dump review
- Case management workflows

The investigation began with malicious domain access alerts and expanded into process analysis, endpoint investigation, evidence validation, and investigative decision-making across multiple tools.

The goal was not simply to review alerts, but to understand how multiple evidence sources connect together during realistic investigations.

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
- Incident response reasoning

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

### 1️⃣ Malicious Domain Validation

Domains investigated:

- `br-icloud.com.br`
- `signin.eby.de.zukruygxctzmmqi.civpro.co.za`

VirusTotal was used to:

- Validate reputation
- Confirm phishing indicators
- Identify vendor detections
- Support investigative decisions

### 2️⃣ SIEM Correlation

Splunk searches were used to:

- Identify affected users
- Review DNS request activity
- Correlate suspicious domains
- Review process command-line behavior
- Identify suspicious hashes
- Connect endpoint activity to users

This transformed the investigation from simple alert review into evidence-based analysis.

### 3️⃣ Endpoint Investigation

Elastic Security telemetry was used to investigate suspicious process activity involving:

- `svchost.exe`
- unusual execution paths
- suspicious command-line behavior
- endpoint process relationships
- suspicious endpoint activity on `workstation-01`

The investigation focused heavily on determining whether observed behavior matched legitimate Windows activity or suspicious execution patterns.

### 4️⃣ Process Validation

Windows Task Manager was used to validate:

- Process IDs
- Active `svchost.exe` instances
- Administrator execution context
- Running process behavior

This helped validate evidence observed in SIEM and endpoint telemetry.

### 5️⃣ Memory Dump Review

A memory dump was reviewed using Visual Studio Code to search for:

- suspicious strings
- domain references
- supporting indicators

This helped support earlier findings from SIEM and endpoint telemetry.

---

## 🧠 Investigation Thinking & Decision-Making

One major lesson from this project was learning how to avoid jumping to conclusions too early.

For example:

- `svchost.exe` itself is not automatically malicious
- many Windows systems legitimately run multiple `svchost.exe` processes
- process names alone are weak evidence

The investigation focused more heavily on:

- execution path
- command-line behavior
- user association
- process IDs
- endpoint telemetry
- memory evidence
- supporting threat intelligence

This helped separate normal system behavior from suspicious activity.

---

## 📚 Important Lessons Learned

### Signal vs Noise Matters

Large amounts of SIEM and endpoint data can become distracting during investigations.

The important skill is identifying which evidence actually supports the concern.

### Validation Is Critical

Alerts should not automatically be trusted without validation.

Evidence was validated through:

- VirusTotal
- SIEM searches
- endpoint telemetry
- Task Manager validation
- memory dump review

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

The most valuable part of the project was learning how to think through uncertainty, identify meaningful signals, and make calm, evidence-based decisions.

---

# 🔎 Lab 03 — SMB & RDP Port Scanning / Lateral Movement Investigation

## 🛡️ Overview

This lab investigated a potential SMB and RDP port scanning alert and expanded it into a larger multi-stage investigation involving:

- SIEM detection validation
- Windows authentication review
- Explicit credential usage analysis
- Service creation investigation
- PsExec-style remote execution review
- Endpoint telemetry correlation
- Escalation decision-making

The investigation began from the following Splunk Mission Control finding:

```text
Potential SMB and RDP Port Scanning from Single Source
```

The source IP involved was:

```text
10.1.2.45
```

The primary goal was determining whether the activity represented simple scanning, administrative activity, or evidence of potential lateral movement.

---

## 🎯 Purpose

This project focused on investigating SMB and RDP-related activity by combining:

- SIEM alert validation
- Windows authentication analysis
- Service creation review
- Remote execution indicators
- PsExec-style behavior
- Endpoint telemetry analysis
- Case management workflows

This lab is valuable because SMB and RDP activity can be legitimate in enterprise environments, but they are also heavily associated with lateral movement and remote execution behavior.

---

## 🛠️ Tools & Technologies Used

| Tool / Data Source | Purpose |
|---|---|
| Splunk Enterprise Security | SIEM investigation and alert validation |
| Splunk Mission Control | Alert intake and workflow tracking |
| Hive / TheHive | Case creation and investigation management |
| Elastic Security | Endpoint telemetry analysis |
| Elastic Discover | Raw endpoint log review |
| Windows Security Logs | Authentication and service analysis |
| PsExec / PSEXESVC Analysis | Remote execution investigation |

---

## 🔍 Investigation Workflow

### 1️⃣ Mission Control Alert Review

The investigation began by reviewing the Splunk Mission Control finding involving source IP:

```text
10.1.2.45
```

The alert showed SMB/RDP-related connections to multiple internal systems.

The first objective was validating whether the detection logic matched actual behavior before escalating.

### 2️⃣ Case Creation & Investigation Management

Hive was used to create and manage the investigation case.

Case management included:

- Severity assignment
- Investigation status tracking
- Timeline documentation
- Evidence tracking
- Task management

This helped organize the investigation into a structured workflow rather than isolated searches.

### 3️⃣ SIEM Detection Validation

Splunk was used to confirm:

- multiple internal targets contacted
- SMB/RDP-related ports involved
- repeated connection behavior
- consistency with the original detection logic

Key finding:

- `10.1.2.45` connected to **six unique internal systems**

This confirmed the alert was not simple noise.

### 4️⃣ Windows Authentication Analysis

Authentication activity was reviewed using:

| Event ID | Meaning |
|---|---|
| 4624 | Successful logon |
| 4625 | Failed logon |
| 4648 | Explicit credential use |

Accounts reviewed included:

- `administrator`
- `yani.zubair`
- `michael.ascot`
- `roger.fedora`
- `svc_xhdi`

This helped determine whether the network activity connected to actual authentication attempts or credential use.

### 5️⃣ Explicit Credential Use Review

Event ID `4648` was particularly important because it may indicate credentials being used to access another system or service.

Explicit credential usage involving:

- `yani.zubair`
- `administrator`

increased investigative concern because it linked network activity with credential-based behavior.

### 6️⃣ Service Creation & PsExec-Style Behavior

Service creation events reviewed included:

| Event ID | Meaning |
|---|---|
| 4697 | Service installed |
| 7045 | New service created |

The investigation reviewed:

- `PSEXESVC.exe`
- executables launched from `C:\Windows\Temp`
- suspicious `svchost.exe` parent process behavior
- temporary executable paths
- remote execution indicators

This moved the investigation beyond simple scanning into possible remote execution activity.

### 7️⃣ Endpoint Correlation in Elastic Security

Elastic Security was used to review endpoint telemetry involving:

- `powershell.exe`
- `mstsc.exe`
- `PSEXESVC.exe`

Elastic Discover was used to correlate raw endpoint logs with the original Splunk findings.

This helped validate whether endpoint evidence supported the SIEM investigation.

---

## 📊 Key Findings

- Source IP `10.1.2.45` connected to six internal systems over SMB/RDP-related ports
- Authentication logs showed both successful and failed logons during the investigation window
- Explicit credential usage increased concern
- Service creation activity suggested possible remote execution
- PsExec-style behavior was identified through `PSEXESVC.exe`
- Suspicious executables launched from `C:\Windows\Temp`
- Affected systems included:
  - `db-01`
  - `fs-01`
- Elastic endpoint telemetry confirmed suspicious process relationships involving:
  - `mstsc.exe`
  - `powershell.exe`
  - `PSEXESVC.exe`

---

## 🧠 Investigation Thinking

A weaker investigation might stop at:

> “This is just port scanning.”

A stronger investigation asks:

- Were successful logons involved?
- Were credentials explicitly used?
- Were services created remotely?
- Did PsExec-style behavior appear?
- Were suspicious files executed from Temp?
- Does endpoint telemetry support the SIEM findings?
- Does this indicate potential lateral movement?

The key lesson was learning how separate evidence sources strengthen the overall investigation story.

---

## 🚨 Why This Was More Than Simple Port Scanning

This investigation became more serious because multiple evidence sources aligned:

- Network connections
- Authentication activity
- Explicit credential usage
- Service creation events
- PsExec indicators
- Temp directory execution
- Endpoint telemetry correlation

Together, these findings supported escalation and deeper investigation.

---

## 🧭 MITRE ATT&CK Concepts Observed

| Technique | Description |
|---|---|
| T1021.001 | Remote Services: Remote Desktop Protocol |
| T1021.002 | Remote Services: SMB/Windows Admin Shares |
| T1078 | Valid Accounts |
| T1569.002 | System Services: Service Execution |
| T1570 | Lateral Tool Transfer |
| T1059 | Command and Scripting Interpreter |

---

## 🛠️ Skills Demonstrated

- Splunk Mission Control investigation workflows
- SIEM detection validation
- Windows authentication analysis
- Explicit credential review
- Event ID interpretation
- PsExec-style remote execution investigation
- Service creation analysis
- Elastic endpoint telemetry correlation
- Parent-child process analysis
- Lateral movement investigation reasoning
- Evidence-based escalation decision-making

---

## 📸 Investigation Evidence

This lab includes 20 investigation screenshots covering:

- Mission Control alerts
- Splunk validation searches
- Authentication analysis
- Explicit credential use review
- Service creation events
- PsExec-style behavior
- Elastic process analyzer views
- Elastic Discover endpoint telemetry
- Hive case documentation

---

## 🧾 Professional Summary

This lab investigated potential SMB and RDP lateral movement activity using Splunk, Hive, Windows Security logs, and Elastic Security.

The investigation validated the original SIEM finding, reviewed authentication behavior, analyzed service creation events, identified PsExec-style indicators, and correlated endpoint telemetry to build a stronger investigation story.

The most important lesson from this project was learning that network alerts become significantly more serious when authentication logs, service creation events, and endpoint telemetry all support the same concern.

---

# 🧪 Lab 04 — Suspicious File Drop & Malware Execution Analysis

## 🛡️ Overview

This lab investigated a suspicious file drop and malware execution event using **Splunk Enterprise Security**, **VirusTotal**, **Excel-based hash deduplication**, and **Elastic EDR**.

The investigation began with a suspicious executable activity alert involving risky directories such as:

- `C:\Users\Public`
- `AppData\Roaming`
- `Temp`

The investigation then narrowed from broad suspicious executable activity into a confirmed malicious endpoint event supported by hash reputation, process lineage, outbound network behavior, Defender-related changes, DLL activity, and persistence-related execution.

---

## 🎯 Purpose

The purpose of this lab was to practice an end-to-end malware investigation workflow involving:

- SIEM alert triage
- Noise reduction and filtering
- Executable path analysis
- Parent process review
- SHA256 hash extraction
- Hash deduplication
- VirusTotal enrichment
- Elastic EDR pivoting
- Endpoint process tree analysis
- Outbound network review
- Persistence-related activity review
- Escalation and containment reasoning

---

## 🛠️ Tools & Technologies Used

| Tool / Data Source | Purpose |
|---|---|
| Splunk Enterprise Security | SIEM alert review and initial investigation |
| Splunk Mission Control | Alert intake and workflow tracking |
| Microsoft Excel | SHA256 hash deduplication and indicator organization |
| VirusTotal | Hash reputation validation |
| Elastic EDR | Endpoint process tree and telemetry analysis |
| Elastic Discover | Raw endpoint telemetry review |
| Elastic Timeline | Endpoint event timeline correlation |
| Windows Endpoint Telemetry | Process, network, Defender, DLL, and persistence review |
| SHA256 Hash Analysis | Indicator validation and enrichment |
| Windows Defender Events | Security configuration change review |
| DLL / Persistence Analysis | Post-execution behavior analysis |

---

## 🔍 Investigation Workflow

### 1️⃣ Mission Control Alert Review

The investigation began by reviewing a suspicious file execution alert in Splunk Mission Control.

The alert focused on executable activity in risky directories, including:

- `C:\Users\Public`
- `AppData\Roaming`
- `Temp`

These locations are important because malware and attacker tooling often use writable directories to stage payloads or evade casual review.

### 2️⃣ Initial Splunk Analysis

Splunk was used to review broad executable activity and identify useful fields, including:

- hostnames
- usernames
- executable paths
- parent processes
- command-line arguments
- SHA256 hashes

At this stage, the goal was not to escalate immediately. The goal was to understand the data and reduce noise.

### 3️⃣ Noise Reduction and Filtering

The investigation reduced noise by filtering out common or less useful patterns, including:

- common parent processes such as `explorer.exe`
- common service-related parent processes such as `services.exe`
- installer-style filenames
- binaries with less suspicious characteristics

The focus was narrowed toward:

- unsigned binaries
- missing-signature binaries
- suspicious executable paths
- suspicious hashes
- risky parent-child relationships

This step was important because broad suspicious file alerts can contain many results, and a strong analyst must reduce the dataset before making conclusions.

### 4️⃣ Hash Extraction and Deduplication

SHA256 hashes were exported into Excel and deduplicated to isolate unique file indicators.

This helped avoid repeatedly investigating the same hash and made the workflow more efficient.

The deduplicated hash list was then used for external reputation validation.

### 5️⃣ VirusTotal Hash Validation

VirusTotal was used to validate suspicious hashes.

One file hash was identified as malicious and was flagged by:

```text
63/72 security vendors
```

The malicious hash was correlated back to the file:

```text
C:\Users\Public\9m5sap.exe
```

on host:

```text
win-3470
```

This moved the investigation from broad suspicious executable activity to a confirmed malicious file requiring deeper endpoint investigation.

### 6️⃣ Elastic EDR Pivot

The investigation pivoted into Elastic EDR to analyze endpoint behavior connected to the malicious file.

Elastic EDR was used to review:

- process trees
- endpoint alerts
- outbound network connections
- process metadata
- timeline events
- persistence indicators
- post-execution behavior

### 7️⃣ Process Lineage Analysis

Elastic EDR showed process lineage involving:

```text
explorer.exe
↓
cmd.exe
↓
9m5sap.exe
```

This chain suggested that the malicious file was not simply a dormant artifact. It appeared to execute through an interactive or scripted process chain.

This increased the severity of the investigation because execution was confirmed.

### 8️⃣ Outbound Network Communication Review

Endpoint telemetry showed outbound communication to:

```text
103.86.47.221
```

This added another layer of concern because the malicious executable was associated with network activity after execution.

The network behavior supported the possibility of command-and-control, payload retrieval, or other malicious communication.

### 9️⃣ Defender Configuration and Post-Execution Activity

The investigation reviewed endpoint evidence involving:

- Defender configuration changes
- DLL load behavior
- `rundll32.exe`
- `regsvr32.exe`
- suspicious persistence-related execution

These behaviors were important because malware often attempts to weaken defenses, load additional components, or establish persistence after execution.

---

## 📊 Key Findings

- A malicious executable named `9m5sap.exe` was identified on host `win-3470`
- The file was located in:

```text
C:\Users\Public
```

- The SHA256 hash associated with the file was flagged by:

```text
63/72 VirusTotal vendors
```

- Elastic EDR telemetry showed:
  - malware detection activity
  - outbound communication to `103.86.47.221`
  - suspicious process execution
  - DLL load behavior
  - Defender configuration changes
  - possible persistence activity involving `regsvr32.exe`

- The execution chain showed:

```text
explorer.exe → cmd.exe → 9m5sap.exe
```

- Combined SIEM, OSINT, and EDR evidence supported treating the activity as a confirmed malicious endpoint event requiring escalation and containment.

---

## 🚨 Why This Was a Confirmed Malicious Endpoint Event

This investigation became more serious because multiple evidence sources aligned:

- Suspicious executable path
- Malicious hash reputation
- VirusTotal detection ratio of `63/72`
- Confirmed host and file path
- Process execution chain
- Outbound network communication
- Defender configuration changes
- DLL and persistence-related behavior
- Elastic EDR process and timeline evidence

This was stronger than a simple suspicious file alert because the investigation connected file reputation, execution behavior, endpoint telemetry, and post-execution activity.

---

## 🧠 Investigation Thinking

A weaker investigation might stop at:

> “VirusTotal says it is malicious.”

A stronger investigation asks:

- Where did the file execute from?
- Which host was affected?
- What parent process launched it?
- Was the file actually executed?
- Did it communicate externally?
- Did it modify Defender settings?
- Did it load DLLs or use Windows utilities?
- Is there evidence of persistence-related behavior?
- Does EDR telemetry support the SIEM finding?

This lab focused on building that stronger investigation story.

---

## 🧭 MITRE ATT&CK Concepts Observed

| Technique | Description |
|---|---|
| T1204 | User Execution |
| T1059 | Command and Scripting Interpreter |
| T1105 | Ingress Tool Transfer |
| T1071 | Application Layer Protocol |
| T1112 | Modify Registry |
| T1562.001 | Impair Defenses: Disable or Modify Tools |
| T1218.010 | Signed Binary Proxy Execution: Regsvr32 |
| T1218.011 | Signed Binary Proxy Execution: Rundll32 |
| T1547 | Boot or Logon Autostart Execution |

---

## 🛠️ Skills Demonstrated

- Splunk Mission Control alert triage
- SIEM noise reduction and filtering
- Suspicious executable path analysis
- Parent process review
- SHA256 hash extraction
- Excel-based hash deduplication
- VirusTotal OSINT enrichment
- Elastic EDR process tree analysis
- Endpoint timeline correlation
- Outbound network communication review
- Defender configuration change review
- DLL and persistence behavior analysis
- Malware escalation reasoning
- Evidence-based documentation

---

## 📸 Investigation Evidence

This lab includes structured screenshot evidence covering:

- Mission Control alert review
- Splunk filtering workflow
- Suspicious executable path analysis
- SHA256 hash review
- Excel hash deduplication
- VirusTotal enrichment
- Elastic EDR process tree analysis
- Endpoint network activity
- Timeline correlation
- Defender modification activity
- DLL activity
- Persistence-related evidence

---

## ✅ Outcome

This lab developed practical experience performing an end-to-end malware investigation using:

- Splunk SIEM
- Excel-based hash analysis
- VirusTotal enrichment
- Elastic EDR telemetry
- Endpoint timeline correlation

The investigation strengthened my ability to move from broad suspicious file alerts into focused malware investigations supported by behavioral evidence, endpoint telemetry, and structured escalation reasoning.

---

## 🧾 Professional Summary

This lab investigated a suspicious malware execution event using Splunk, VirusTotal, and Elastic EDR.

The investigation validated a malicious hash, correlated it to `C:\Users\Public\9m5sap.exe` on `win-3470`, reviewed endpoint process behavior, identified outbound communication to `103.86.47.221`, and analyzed post-execution activity including Defender configuration changes, DLL behavior, and possible persistence-related execution.

The most important lesson from this project was learning how to reduce SIEM noise, validate indicators, pivot into EDR telemetry, and build a complete malware investigation story from multiple evidence sources.

---

# 🧾 Week 15 Summary

Week 15 focused heavily on endpoint investigations and incident response reasoning.

Across these labs, I practiced:

- Turning alerts into structured investigations
- Validating detection logic before escalating
- Tracking observables and timelines
- Reviewing endpoint process behavior
- Correlating SIEM and EDR evidence
- Reviewing authentication activity
- Investigating service creation behavior
- Identifying remote execution indicators
- Investigating suspicious file execution
- Validating malicious file hashes
- Reviewing outbound network activity
- Analyzing post-execution behavior
- Thinking through blast radius and containment
- Building evidence-based escalation decisions
- Documenting investigations professionally

The biggest lesson from this week is:

> An alert is only the beginning.  
> A strong analyst builds the full investigation story before making a decision.
````

