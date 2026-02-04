Case File — Suspicious PowerShell LOLBAS Activity

Alert Summary

An alert was generated for suspicious PowerShell execution using Invoke-WebRequest with hidden and encoded parameters. The activity involved downloading external files in a way that attempted to reduce visibility and obscure intent.

The goal of this case was to validate whether this behavior was normal administrative activity or something that required escalation.

Data Sources

SIEM: Splunk Enterprise Security

Index: cs_soc

Log Type: Endpoint process execution logs

Primary Fields Reviewed:

Hostname

UserName

ProcessCommandLine

MaliciousURL

_time

Timeline of Activity

PowerShell executed with -nop and -w hidden

Encoded command used during execution

Invoke-WebRequest used to download external files

Downloaded files saved with misleading names

Activity observed on multiple hosts and users

Alert surfaced in Mission Control for review

Investigation Notes

The initial URLs looked familiar, which made this alert easy to underestimate. Instead of focusing on reputation alone, the review focused on how PowerShell was used.

Key attention was given to:

Hidden execution

Encoded command usage

File naming that did not match expected behavior

Repeated patterns across systems

This helped separate normal-looking data from risky behavior.

Key Signals

LOLBAS usage (Invoke-WebRequest)

Hidden PowerShell execution

Encoded command usage

External downloads saved as misleading executables

Repeated behavior across endpoints

These signals mattered more than reputation results alone.

Enrichment

URLs checked using VirusTotal

Some domains showed low or no detections

OSINT used as supporting context, not the final decision factor

MITRE ATT&CK Mapping

T1059.001 — Command and Scripting Interpreter: PowerShell

T1105 — Ingress Tool Transfer

Decision

Escalate for further investigation

Justification:
The behavior showed clear intent to hide execution and download external payloads. While reputation results were mixed, the execution method justified escalation.

Recommended Next Actions

Review endpoint activity following file execution

Validate whether the behavior matches approved scripts

Consider restricting encoded PowerShell execution

Improve alert documentation for faster triage

Additional Note

This case reinforced the importance of behavioral analysis over reputation-based decisions and showed how small command-line details can change the outcome of an investigation.