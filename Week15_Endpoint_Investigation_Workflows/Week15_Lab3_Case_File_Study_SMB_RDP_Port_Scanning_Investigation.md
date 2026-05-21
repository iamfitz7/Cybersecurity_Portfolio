Case File Study: Potential SMB and RDP Port Scanning from Single Source

- Case Overview

**Case Title:** Potential SMB and RDP Port Scanning from Single Source  
**Case Type:** Possible Reconnaissance / Possible Lateral Movement  
**Severity:** Medium  
**Main Source IP:** 10.1.2.45  
**Main Concern:** One source IP appeared to connect to multiple internal systems over SMB/RDP-related ports.  
**Tools Used:** Splunk Enterprise Security, Hive, Elastic Security, Elastic Discover, Windows Security Logs, Endpoint Process Telemetry  

- Initial Alert

The case started from a finding in Splunk Mission Control named:

**Potential SMB and RDP Port Scanning from Single Source**

The alert description stated that a single source IP address scanned or connected to more than five unique target IPs on critical remote access and file-sharing ports. These ports included SMB and RDP-related activity.

The alert was not treated as a final conclusion. It was treated as a starting point for validation.

Investigation Question

The main question was:

**Is this normal internal administration or possible lateral movement behavior?**

To answer that, I needed to review more than one type of evidence. Network activity alone was not enough. I needed to check authentication logs, service creation events, process execution evidence, and endpoint activity.

- Evidence Reviewed

1. Mission Control Finding

The alert showed a medium-urgency finding related to SMB and RDP port scanning. The finding included MITRE ATT&CK-related annotations and a detection rule reference.

This confirmed that the case had a clear detection reason, but it still needed validation.

2. Hive Case Record

The case was opened in Hive with the title:

**Potential SMB and RDP Port Scanning from Single Source**

Hive showed the case severity, status, time to detect, and description. This helped organize the investigation and showed how case management supports incident response work.

3. Splunk Detection Validation

The Splunk search showed source IP:

**10.1.2.45**

This source IP reached six unique target IP addresses:

- 192.168.100.10
- 192.168.100.11
- 192.168.100.12
- 192.168.100.13
- 192.168.100.14
- 192.168.100.15

This confirmed that the alert logic matched the observed activity.

4. Windows Authentication Events

Windows authentication logs were reviewed for the source IP. The investigation included Event IDs:

- 4624: Successful logon
- 4625: Failed logon

The results showed multiple account and host combinations. This mattered because it helped connect network activity to authentication behavior.

Accounts and systems observed included:

- administrator
- michael.ascot
- roger.fedora
- svc_xhdi
- yani.zubair
- db-01
- dc-01
- fs-01
- win-3449

This showed that the investigation involved more than one system and more than one account.

5. Explicit Credential Use

Event ID 4648 was reviewed to check for explicit credential use. The search showed activity involving:

- Account: yani.zubair
- Target account: administrator
- Host: win-3449

This was important because explicit credential use can be relevant when reviewing possible remote access or lateral movement.

6. Service Installation Events

Event IDs 4697 and 7045 were reviewed to identify service installation behavior. These events can matter because remote execution tools often create services on target systems.

The logs showed service-related events on systems such as:

- db-01
- dc-01

This added more concern because service creation can be connected to remote execution.

7. PsExec-Style Activity

The investigation found `psexesvc.exe` activity from:

```text
C:\Windows\Temp\tGZQiyrm.exe -service