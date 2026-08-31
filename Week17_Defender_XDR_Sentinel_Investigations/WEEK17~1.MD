# Week 17 — Lab 5: Suspicious PowerShell Endpoint Investigation & Response

## Incident Case Study

---

# Case Summary

This case involved suspicious-looking PowerShell activity on a Windows 11 Enterprise endpoint monitored by Microsoft Defender for Endpoint.

The endpoint was:

`desktop-3sepq1q`

The activity included:

- Encoded PowerShell execution
- File creation
- External HTTPS communication
- CMD execution
- WHOAMI execution

These behaviors can appear in real attacks, so they were treated as activity that required investigation.

The purpose of the case was to determine whether the behavior represented an actual security compromise or legitimate activity.

---

# Case Classification

Case type:

Suspicious PowerShell Endpoint Activity

Initial severity:

Medium

Final severity:

Low

Final classification:

Benign / Authorized Security Testing

Containment:

Not required

---

# Affected Device

Device:

`desktop-3sepq1q`

Operating system:

Windows 11 Enterprise

Local IP:

`10.0.2.15`

Endpoint security:

Microsoft Defender for Endpoint

Device status:

Active

---

# Initial Suspicion

The activity was considered suspicious enough to investigate because several behaviors appeared together.

The main indicators included:

- `powershell.exe`
- `-EncodedCommand`
- PowerShell launching another process
- `cmd.exe`
- `whoami.exe`
- File creation
- Outbound HTTPS traffic

None of these items alone prove malicious activity.

However, when several of them appear together, they can resemble attacker behavior.

---

# Investigation Question

The main question for the case was:

**What did PowerShell actually do?**

The investigation did not depend only on whether Defender called PowerShell suspicious.

I reviewed the activity across multiple telemetry sources.

---

# Evidence Source 1 — Process Telemetry

Using Microsoft Defender Advanced Hunting and `DeviceProcessEvents`, I identified the PowerShell activity.

The investigation showed an encoded PowerShell process containing:

`-EncodedCommand`

I reviewed:

- Timestamp
- Device name
- User account
- Initiating process
- Command line
- Process ID
- Initiating process ID

The encoded command was part of the controlled lab activity.

It created a harmless text file.

---

# Evidence Source 2 — Process Chain

The process telemetry also showed:

`powershell.exe → cmd.exe → whoami.exe`

PowerShell launched CMD.

CMD then launched WHOAMI.

This helped establish process ancestry.

WHOAMI is a normal Windows tool, but attackers sometimes use it for system and account discovery.

In this case, it was intentionally executed as part of the lab.

---

# Evidence Source 3 — File Activity

Using `DeviceFileEvents`, I identified a matching file event.

Defender recorded:

`week17-example.html`

with:

`ActionType = FileCreated`

The initiating process was:

`powershell.exe`

This supported the earlier PowerShell web request.

Other controlled files generated during the lab included:

- `week17-lab5.txt`
- `week17-user.txt`

These files were harmless and existed only to create known evidence for the investigation.

---

# Evidence Source 4 — Network Activity

Using `DeviceNetworkEvents`, I confirmed that PowerShell made an outbound network connection.

The Defender telemetry showed:

Initiating process:

`powershell.exe`

Remote URL:

`example.com`

Remote IP:

`104.20.23.154`

Remote port:

`443`

Protocol:

`Tcp`

Action:

`ConnectionSuccess`

This was important because it proved that PowerShell actually established the network connection.

The command line alone only showed what PowerShell was told to do.

The network telemetry showed the connection actually occurred.

---

# Evidence Source 5 — Device Timeline

I reviewed the Microsoft Defender device Timeline.

The Timeline contained PowerShell and other endpoint activity.

I inspected process details such as:

- Command line
- Execution time
- Process ID
- Image path
- User information
- Parent and initiating process information

The Timeline was useful for additional context.

However, not every PowerShell event shown on the device was related to my controlled activity.

I only included evidence that could be supported by timestamps and other matching details.

---

# Existing Incident Review

The device contained two existing incidents:

### Suspicious PowerShell command line

and:

### Execution incident on one endpoint

At first, these incident titles looked like they could be related to my lab.

I checked the timestamps before reaching a conclusion.

Both incidents were from:

August 6, 2026

My Lab 5 activity occurred on:

August 31, 2026

Because the timestamps were different, both incidents were excluded from the current case.

The older "Execution incident on one endpoint" was already marked:

- Resolved
- Benign Positive

This prevented me from incorrectly claiming that my controlled activity generated an alert.

---

# Timeline Reconstruction

The controlled activity can be reconstructed as follows.

## Encoded PowerShell

Lab user
↓
PowerShell
↓
`-EncodedCommand`
↓
Harmless encoded command
↓
Text file created

## Network Activity

PowerShell
↓
HTTPS request
↓
`example.com`
↓
`104.20.23.154`
↓
TCP port 443
↓
`ConnectionSuccess`
↓
`week17-example.html`

## Child Process Activity

PowerShell
↓
CMD
↓
WHOAMI

---

# Was the Activity Malicious?

No.

The activity contained suspicious-looking behaviors, but the investigation showed that it was authorized lab testing.

There was no evidence of:

- Malicious executable payload
- Persistence
- Credential dumping
- Credential theft
- Lateral movement
- Command-and-control
- Ransomware
- Data exfiltration
- Continued attacker access

The external destination was the known harmless domain:

`example.com`

The files created during the test were also harmless.

---

# Incident Disposition

Final classification:

**Benign / Authorized Security Testing**

Initial suspicion:

**Medium**

Final risk:

**Low**

Containment:

**Not required**

---

# Response Decision

Microsoft Defender provided response actions such as:

- Collect Investigation Package
- Automated Investigation
- Live Response
- Isolate Device
- Go hunt

I reviewed the available options but did not isolate the endpoint.

Isolation would have been unnecessary because there was no evidence of an active compromise.

The investigation did not identify any continuing attacker activity or malicious payload.

---

# Why Isolation Was Not Used

Endpoint isolation is an important response action when a machine may still be compromised.

Examples that would have made isolation more reasonable include:

- Malware execution
- Active command-and-control traffic
- Credential theft
- Ransomware activity
- Lateral movement
- Data exfiltration
- Unknown executable payloads
- Persistent attacker access

None of these conditions were found during this case.

Because the activity was controlled and authorized, isolation was not justified.

---

# Analyst Decision

The investigation started with activity that looked suspicious.

Instead of making a decision based only on PowerShell or `-EncodedCommand`, I correlated several types of evidence.

I reviewed:

- Process telemetry
- Command-line telemetry
- Parent and child process relationships
- User context
- File activity
- Network activity
- Device timeline information
- Existing incidents
- Response options

The evidence showed that the activity matched the authorized lab test.

The final decision was:

**No security incident requiring containment.**

---

# Important Investigation Lesson

One of the strongest lessons from this case was that an alert name alone is not enough to connect an alert to an investigation.

The Defender portal already contained an incident called:

`Suspicious PowerShell command line`

It would have been easy to assume that my lab caused it.

Instead, I checked the timestamp.

The incident happened several weeks before the lab.

I excluded it.

This showed the importance of following evidence instead of trying to make the evidence match an expected answer.

---

# Telemetry vs Alert

Another important lesson was understanding the difference between endpoint telemetry and an alert.

Telemetry means Defender recorded an event.

An alert means Defender detection logic decided that the event or group of events met a security detection condition.

My controlled activity produced useful process, file, and network telemetry.

I was able to fully investigate the activity even without confirming a new alert connected to the lab.

This showed why Advanced Hunting is valuable.

---

# Key Evidence

## Process Evidence

`powershell.exe → cmd.exe → whoami.exe`

## Encoded Execution

`powershell.exe -NoProfile -EncodedCommand ...`

## File Evidence

`week17-example.html`

Action:

`FileCreated`

## Network Evidence

Remote URL:

`example.com`

Remote IP:

`104.20.23.154`

Port:

`443`

Protocol:

`Tcp`

Result:

`ConnectionSuccess`

---

# MITRE ATT&CK Context

Some behaviors observed during the investigation can resemble techniques used by attackers.

Examples include:

PowerShell execution

Command and scripting interpreter activity

System or account discovery using WHOAMI

Network communication

These behaviors were reviewed as possible security indicators.

However, MITRE ATT&CK techniques describe behavior.

They do not automatically mean that an attack occurred.

Context and supporting evidence still determine the final incident decision.

---

# Final Case Conclusion

The suspicious PowerShell activity was successfully reconstructed using Microsoft Defender XDR and Defender for Endpoint telemetry.

The investigation confirmed:

- Encoded PowerShell execution
- PowerShell process activity
- CMD child process execution
- WHOAMI execution
- File creation
- Successful HTTPS communication
- Known remote destination
- User and endpoint context

No evidence showed an actual compromise.

The activity was confirmed to be controlled and authorized security testing.

Final disposition:

**Benign Authorized Security Testing**

Final risk:

**Low**

Containment:

**Not Required**

---

# Interview Explanation

If I were asked to explain this case during an interview, I would say:

"I performed a suspicious PowerShell endpoint investigation using Microsoft Defender XDR and Advanced Hunting.

I started with PowerShell process telemetry and identified an encoded command. I reviewed the user, full command line, initiating process, process IDs, and child processes.

I reconstructed a process chain where PowerShell launched CMD and CMD launched WHOAMI.

I then used DeviceFileEvents and found file creation associated with PowerShell.

After that, I used DeviceNetworkEvents and confirmed a successful TCP connection from PowerShell to example.com over port 443.

I also reviewed the Defender device Timeline and checked existing incidents.

I found a suspicious PowerShell incident in Defender, but timestamp validation showed that it occurred weeks before my lab activity. I excluded it instead of incorrectly claiming that my lab generated the incident.

The activity was authorized lab testing, and I found no malware, persistence, credential theft, lateral movement, C2, or exfiltration.

Because there was no continuing compromise, I decided endpoint isolation was not required."

---

# Final Takeaway

The biggest lesson from this case was:

**Suspicious telemetry is the beginning of an investigation, not the final answer.**

PowerShell may be legitimate or malicious.

Encoded commands may be legitimate or malicious.

WHOAMI may be legitimate or malicious.

Network traffic may be legitimate or malicious.

The analyst has to correlate the evidence and determine what actually happened.

That is what I practiced in this case.