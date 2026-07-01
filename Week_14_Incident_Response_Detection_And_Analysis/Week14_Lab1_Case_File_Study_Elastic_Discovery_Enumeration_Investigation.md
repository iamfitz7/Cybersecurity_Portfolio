Elastic Security Discovery & Enumeration Investigation Case Study (Week 14 Lab #1)


Case Study: Elastic Security Discovery & Enumeration Investigation

Investigation Overview

Platform: Elastic Security

Investigation Type: Discovery & Enumeration Alert

Operating System: Windows Server 2022 Standard

Severity: High

Risk Score: 73

Host: workstation-01

User Context: NT AUTHORITY\SYSTEM

Objective

Investigate a Discovery or Enumeration alert to determine whether the observed activity represented legitimate administrative behavior or potentially malicious activity.

Initial Observation

Elastic Security generated a high-severity alert after detecting the execution of net.exe, a Windows command commonly associated with discovery and local group management.

Because this command is frequently used by both administrators and attackers, additional investigation was required before determining whether the alert represented malicious activity.

Investigation Findings

During the investigation, I reviewed:

Alert metadata
Process details
Command line execution
User context
Host information
Process lineage using Elastic Analyzer

The executed command was:

net localgroup "Performance Monitor Users" "NT SERVICE\SplunkForwarder" /add

The complete process lineage was:

wininit.exe
└── services.exe
    └── msiexec.exe
        └── msiexec.exe
            └── cmd.exe
                └── net.exe
                    └── net1.exe

The parent-child relationship indicated that the command originated from Windows Installer during software installation.

Key Evidence
Host
workstation-01
User
NT AUTHORITY\SYSTEM
Process
net.exe
Parent Process
cmd.exe
Ancestor Processes
msiexec.exe
services.exe
wininit.exe
Command Executed
net localgroup "Performance Monitor Users" "NT SERVICE\SplunkForwarder" /add
Analysis

Although net.exe is commonly associated with Windows discovery activity, the surrounding evidence suggested legitimate software installation behavior.

The command specifically added the Splunk Forwarder service account to the Performance Monitor Users group, which aligns with expected software configuration requirements.

Additionally, the process lineage originated from Windows Installer rather than an unexpected application or user process.

No additional evidence suggested credential theft, persistence, privilege escalation, malware execution, or lateral movement during the investigation.

Assessment

Classification

Likely Benign Administrative Activity

Confidence

Moderate to High

The available evidence strongly supports software installation behavior, although confirmation through organizational change management records would provide additional assurance in a production environment.

Recommended Next Steps
Verify that Splunk Forwarder installation was authorized.
Confirm software deployment records.
Document investigation findings.
Close the alert if administrative activity is confirmed.
Continue monitoring for related suspicious activity if authorization cannot be verified.
Lessons Learned

This investigation demonstrated that alert titles describe detected behavior rather than confirmed incidents.

A structured investigation that considers process lineage, command execution, user context, and surrounding system activity provides a much stronger basis for decision making than relying on alert severity alone.