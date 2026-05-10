Week 15 Lab #2: Case File Study — Malicious Domain Access Allowed with Suspicious Endpoint Activity

## Case Title

Malicious Domain Access Allowed — Domain Reputation, SIEM Correlation, EDR Review, and Process Validation

## Case Summary

A malicious domain access alert was investigated to determine whether the activity represented real risk. The investigation started with domain reputation checking, then moved into Splunk correlation, endpoint process review in Elastic, Task Manager validation, and memory dump string analysis.

The evidence showed suspicious domain activity connected to users, suspicious `svchost.exe` execution, hash indicators, and endpoint process activity. Based on the collected evidence, escalation was justified.

## Severity

High

## TLP / PAP

TLP: Amber  
PAP: Amber

## Data Sources Reviewed

- Hive case management
- Splunk Search & Reporting
- Zscaler-style DNS/proxy logs
- CrowdStrike-style process telemetry
- Elastic Security endpoint telemetry
- VirusTotal
- Windows Task Manager
- Memory dump review in Visual Studio Code

## Key Observables

### Domains

- `br-icloud.com.br`
- `signin.eby.de.zukruygxctzmmqi.civpro.co.za`

### Users

- `john.doe`
- `alice.jones`
- other users visible in DNS correlation results

### Process

- `svchost.exe`

### Suspicious Paths / Command Context

- `C:\Windows\svchost.exe -arg1`
- `C:\Users\Administrator\AppData\Local\Temp\svchost.exe`
- `svchost.exe -url br-icloud.com`

### Process IDs

- PID 1148
- PID 6192

### Hash Evidence

Hashes were observed in Splunk/CrowdStrike-style telemetry and connected to suspicious `svchost.exe` activity.

## Timeline

1. Malicious domain access alert was reviewed in the case management platform.
2. Investigation task was created to validate domain reputation and reduce detection noise.
3. VirusTotal confirmed suspicious domains were flagged by multiple vendors.
4. Splunk DNS correlation showed malicious domain access tied to multiple users.
5. Splunk process telemetry showed suspicious `svchost.exe` activity for `john.doe`.
6. Hash evidence was reviewed and documented in the case.
7. Elastic Security was used to inspect endpoint process behavior on `workstation-01`.
8. Task Manager was used to verify suspicious PIDs on the endpoint.
9. A memory dump was reviewed in VS Code, and suspicious domain strings were found.
10. Case notes were updated to document findings and support escalation.

## Investigation Notes

### Domain Reputation

VirusTotal showed that the suspicious domains were flagged by multiple vendors. This helped validate that the domain alert was worth investigating further. The domain `br-icloud.com.br` was especially important because it appeared in both reputation checks and later process/memory evidence.

### Splunk Correlation

Splunk was used to connect domain activity to users and process activity. DNS correlation showed multiple users tied to suspicious domains. Process searches showed `svchost.exe` activity connected to command-line and hash evidence.

This mattered because it moved the investigation beyond domain reputation. The activity was not just “a bad domain appeared.” It connected to user and endpoint behavior.

### Endpoint Review

Elastic Security showed suspicious process behavior on `workstation-01`. The investigation focused on `svchost.exe`, process relationships, execution path, and user context.

The suspicious part was not only the process name. The suspicious part was the combination of:

- unusual execution path
- administrator context
- suspicious domain relation
- process ID match
- supporting telemetry from Splunk

### Task Manager Validation

Task Manager was used to verify that the suspicious PIDs were present on the system. This helped confirm that the endpoint evidence was not only historical log data, but tied to active process information during review.

### Memory Dump Review

A memory dump was opened in Visual Studio Code and searched for suspicious strings. The domain `br-icloud.com` appeared in the dump content. This supported the idea that the suspicious domain was connected to endpoint activity.

This was not full malware reverse engineering. It was basic string-based validation to support the investigation.

## MITRE ATT&CK Mapping

### T1059 — Command and Scripting Interpreter

Suspicious command-line activity was observed around process execution.

### T1105 — Ingress Tool Transfer

The domain and process behavior suggested possible remote content retrieval or transfer activity.

### T1071 — Application Layer Protocol

Domain-based communication may align with application-layer network activity.

### T1204 — User Execution

User-linked activity was observed, although full user intent was not confirmed.

## Decision

Escalate for further response.

## Justification

Escalation was reasonable because the evidence came from multiple sources:

- malicious domain reputation
- multiple users associated with suspicious domains
- suspicious process execution
- hash evidence
- EDR process information
- Task Manager process validation
- memory dump string evidence

This was stronger than a single alert. The evidence supported further containment and response review.

## Recommended Next Actions

- Isolate or contain the affected workstation if approved.
- Block confirmed malicious domains at the proxy/DNS layer.
- Review affected user accounts and consider credential rotation.
- Collect full endpoint evidence before wiping or rebuilding the machine.
- Review additional hosts for the same domain, hash, or process behavior.
- Preserve case notes, screenshots, and timelines for review.
- Escalate to deeper response team for full analysis.

## Limitations

This investigation did not prove the full root cause. It also did not include full malware reverse engineering. The memory dump review was limited to basic string searching. More evidence would be needed in a production environment before making final root cause claims.

## Final Case Conclusion

The investigation found enough evidence to support escalation. Malicious domain reputation, user correlation, suspicious `svchost.exe` behavior, hash evidence, EDR process details, and memory dump strings all supported the concern. The case should not be closed as noise. It should be treated as suspicious endpoint activity requiring deeper response and containment review.