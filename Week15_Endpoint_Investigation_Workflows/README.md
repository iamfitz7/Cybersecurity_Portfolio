# 🖥️ Multi-Host PowerShell Investigation  
### Week 15 — Endpoint Investigation Workflows | Lab 01

---

## 🛡️ Overview

This lab simulates a **multi-host endpoint investigation** involving suspicious PowerShell execution across multiple Windows workstations. The focus of this project is **incident response thinking**, not tools.

Rather than simply reviewing an alert, this lab follows a **realistic incident response workflow**:

- Alert appears  
- Case is created  
- Observables are tracked  
- Scope expands  
- Host behavior is analyzed  
- Blast radius is assessed  
- Containment decisions are made  
- Findings are documented  

This lab represents a **shift from alert review to incident handling**, which is a core skill in SOC and Incident Response roles.

---

## 🎯 Objectives

This lab was designed to practice real-world incident response thinking:

- Transition from **alert review** to **incident handling**
- Build a structured **case investigation workflow**
- Track observables and affected systems
- Analyze **endpoint process behavior**
- Assess **blast radius and potential spread**
- Make **containment decisions**
- Document findings in a professional case format

---

## 🧠 Why This Lab Matters

PowerShell is widely used for legitimate administration.  
However, it is also **commonly abused in real-world attacks**.

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

This lab focuses on **thinking like an incident responder**, not just identifying suspicious activity.

---

## 🚨 Mock Incident Scenario

A SIEM alert reports **suspicious PowerShell activity across multiple Windows workstations**.

Initial findings:

- PowerShell launched from `cmd.exe`
- Activity observed across **multiple hosts**
- **Multiple users involved**
- Files written to **Temp directory**
- Suspicious URL observed
- Behavior repeated across systems
- Possible **lateral movement behavior**

This scenario simulates:

- Multi-host compromise investigation  
- PowerShell abuse detection  
- Observable tracking  
- Process tree analysis  
- Blast radius expansion  
- Containment decision making  

---

## 🔍 Investigation Workflow

This lab follows a documentation-first investigation workflow:

### 1. Alert Intake & Case Creation

- Alert reviewed
- Case created
- Severity assigned (High)
- TLP classification applied (Amber)
- Initial facts documented

---

### 2. Observable Tracking

Tracked observables included:

**URLs**
- hxxps://example-malicious-site[.]com/payload
- hxxps://example-clean-site[.]com/update

**Hostnames**
- workstation1
- workstation2

**Users**
- userA
- userB
- Administrator

**Files**
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


System → cmd.exe → powershell.exe → download → file creation

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

These factors increased risk from **single-host alert** to **potential multi-host incident**.

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

 PowerShell may be legitimate administrative activity
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

This is not a tool-focused lab —  
This is a **thinking and workflow lab**, which is often more valuable.

---

## 🧾 Professional Summary

This lab simulated a multi-host PowerShell investigation to practice incident response workflows. The project focused on transforming an alert into a structured case, tracking observables, analyzing endpoint behavior, expanding scope, and making containment decisions. The investigation emphasized structured thinking, documentation, and proportional response — core skills used in real-world SOC and incident response environments.
---
