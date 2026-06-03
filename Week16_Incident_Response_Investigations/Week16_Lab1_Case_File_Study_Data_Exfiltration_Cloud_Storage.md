Case File Study: Data Exfiltration via Cloud Storage

## Case Title: Data Exfiltration via Cloud Storage Investigation

##Case Type: Security Operations / Incident Response / Data Exfiltration

## Severity: High

- Case Summary

A data exfiltration alert was reviewed involving the user [alice@corp.local](mailto:alice@corp.local) and the device alice-workstation. The investigation focused on determining whether data was being moved from the internal environment to external cloud storage locations.

The investigation connected several forms of evidence: Splunk detection results, ZIA web logs, VirusTotal threat intelligence, Elastic EDR activity, staged files, process arguments, and remote interactive logon evidence. The strongest evidence came from the combination of cloud upload activity, command-line user agents, curl execution, PowerShell activity, and file names that suggested sensitive data staging.

- Initial Alert

The initial alert was:

Data Exfiltration via Cloud Storage Detected

The alert indicated possible outbound data movement from an endpoint or server toward cloud storage. Since data movement can be normal in business environments, the alert needed validation before making a final decision.

- Key User and Host

User:

[alice@corp.local](mailto:alice@corp.local)

Host:

alice-workstation

- Main Evidence Collected

1. Splunk Detection Evidence

Splunk showed outbound traffic from [alice@corp.local](mailto:alice@corp.local) with transfers above the detection threshold. The query converted outbound bytes into megabytes, which made the traffic easier to review.

Important fields included:

* event_user
* event_department
* event_locationname
* devices
* destination IPs
* apps
* total bytes
* outbound MB

The user appeared across multiple departments and locations, including HR, IT, Sales, HQ, Remote, and Branch-2. This was not normal behavior for one user and suggested possible account misuse or lateral movement.

2. ZIA Web Log Evidence

ZIA web logs showed cloud storage activity involving [alice@corp.local](mailto:alice@corp.local). The results included URLs, client IPs, file types, user agents, and total transfer sizes.

The important observation was that curl appeared as a user agent. Normal cloud uploads are usually performed through browsers or approved sync clients. Curl suggests command-line or scripted transfer activity.

3. Threat Intelligence Evidence

VirusTotal was used to review destination IP reputation. Some results appeared clean, but the investigation did not stop there. The IP relations and communicating files sections provided more historical context.

This mattered because a clean current detection score does not always prove safety. Some infrastructure may be newly used, shared, rotated, or not widely reported yet.

4. Elastic EDR Evidence

Elastic EDR showed suspicious endpoint activity on alice-workstation. Evidence included:

* PowerShell activity
* whoami.exe discovery activity
* curl.exe execution
* file staging under Temp paths
* process arguments referencing data upload
* RemoteInteractive logon evidence

The process arguments were especially important because they showed intent. Curl was not just running by itself. It was being used with arguments connected to upload activity and file names such as Stolen_Finance_Data.

5. File Staging Evidence

Files were observed in staging paths such as:

* C:\Windows\Temp\Exfil_Staging\Finance_Data.txt
* C:\Windows\Temp\Exfil_Staging\HR_Data.txt
* C:\Windows\Temp\Legal_Discovery_Backup.zip

This supported the idea that data was prepared before transfer. That matches a common exfiltration pattern: stage the data first, then transfer it out.

6. Remote Interactive Logon Evidence

The investigation showed RemoteInteractive logon evidence. This may indicate remote access through RDP or a remote-control method. This is important because it could help explain how the attacker accessed the system or controlled the activity.

- Timeline Summary

1. Alert identified possible cloud storage exfiltration.
2. Splunk showed outbound transfer activity above the threshold.
3. User behavior showed department and location anomalies.
4. ZIA logs showed cloud storage URLs and suspicious user agents.
5. VirusTotal was used to enrich destination IPs.
6. Elastic EDR showed process execution and staged files.
7. Process arguments showed curl activity tied to sensitive file names.
8. RemoteInteractive logon evidence suggested possible remote access.
9. Case should be escalated for containment and deeper review.

- Analysis

The evidence supports a likely data exfiltration attempt. The strongest finding was the combination of staged files, curl execution, cloud upload behavior, and suspicious process arguments.

One important point is that event outcomes marked as unknown should not be treated as harmless. A transfer could be partial and still expose sensitive information. Because of this, the case should be treated seriously even if every transfer is not clearly marked as successful.

- Recommended Response Actions

1. Isolate alice-workstation from the network.
2. Disable or rotate credentials for [alice@corp.local](mailto:alice@corp.local) based on company policy.
3. Preserve logs and endpoint evidence before deleting anything.
4. Review cloud access and rotate cloud credentials if needed.
5. Identify whether the same user or destination IPs appear on other systems.
6. Involve DLP, forensics, system administrators, and management as needed.
7. Interview the user or confirm business justification through the proper process.
8. Document all observables and actions in the case record.

- Case Decision

This case should be escalated as suspicious and treated as a likely exfiltration attempt until proven otherwise.

The case should not be closed as benign because the evidence includes suspicious cloud upload behavior, command-line transfer tools, staged sensitive files, and possible remote access activity.

- Lessons Learned

This investigation showed that data exfiltration is not proven by one alert alone. A strong case requires multiple pieces of evidence that support each other. The most important evidence came from the process arguments, staged file paths, user behavior anomalies, and cloud upload logs.

The biggest takeaway is that real incident response requires careful correlation. Splunk helped identify the event, ZIA showed the web activity, VirusTotal added context, and Elastic EDR showed what happened on the endpoint.
