# Week 16 Lab 3 Case File

## Incident Title

Possible Insider Threat, Privileged Account Abuse, and Data Exfiltration Investigation

---

## Incident Summary

A security alert identified abnormal activity associated with the privileged account corp\admin. Initial review indicated the account was performing actions across multiple systems that differed from expected administrative behavior.

The investigation focused on determining whether the activity represented legitimate administration, account compromise, insider misuse, or data theft.

---

## Alert Information

Detection Name:

Possible Insider Threat – Misuse of Privileged Account

Severity:

High

Status:

Confirmed Security Incident

---

## Investigation Timeline

### Phase 1 – Alert Validation

Reviewed the original detection and confirmed that the alert was associated with privileged account activity involving corp\admin.

The account was identified as an administrative account with elevated permissions capable of accessing sensitive organizational resources.

---

### Phase 2 – Authentication Review

Investigated Event ID 4624 events to identify successful logons associated with the account.

Findings showed activity across multiple hosts and source IP addresses.

Observed systems included:

* ADMIN-JUMP01
* DC01
* FILE-NAS01
* WB-ALICE01
* WB-BOB02

---

### Phase 3 – Privilege Validation

Reviewed Event ID 4672 records.

Confirmed that elevated administrative privileges had been assigned to the account.

This validated that the account possessed sufficient permissions to access sensitive organizational data.

---

### Phase 4 – Process Execution Analysis

Reviewed Event ID 4688 process creation events.

Observed:

* powershell.exe
* cmd.exe
* rclone.exe

Multiple PowerShell executions contained encoded commands and hidden execution parameters.

These activities increased concern regarding defense evasion techniques.

---

### Phase 5 – Sensitive File Access Investigation

Reviewed Event ID 4663 object access events.

Identified access to:

Q4_Salary.xlsx

The file contained potentially sensitive organizational information.

---

### Phase 6 – Data Exfiltration Evidence

Investigation identified Rclone execution referencing:

Q4_Salary.xlsx

Command activity indicated the file was being transferred to a remote cloud storage destination.

This represented the strongest evidence supporting potential data exfiltration.

---

### Phase 7 – EDR Validation

Endpoint investigation confirmed:

* PowerShell execution
* Encoded commands
* Rclone execution
* File transfer activity
* Process relationships

Process chain evidence showed:

cmd.exe

↓

powershell.exe

↓

rclone.exe

This validated findings observed within SIEM data.

---

## Findings

### Confirmed

* Successful privileged logons
* Administrative privilege assignment
* Encoded PowerShell execution
* Sensitive file access
* Rclone execution
* Potential cloud-based file transfer

### Risk Level

High

### Impact

Potential exposure of sensitive salary information.

---

- Recommended Containment Actions

* Isolate affected workstation
* Reset privileged credentials
* Review additional file access activity
* Block unauthorized cloud synchronization tools
* Monitor privileged account usage
* Expand investigation to determine blast radius



- Final Assessment

Evidence gathered during the investigation supports a conclusion of privileged account misuse involving suspicious PowerShell activity, access to sensitive files, and potential data exfiltration through Rclone. Additional containment and remediation activities are recommended to prevent further compromise and determine the full scope of the incident.
