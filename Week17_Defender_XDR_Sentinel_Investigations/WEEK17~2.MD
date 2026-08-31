# Week 17 — Lab 5: Suspicious PowerShell Endpoint Investigation & Response

## Professional Technical Analysis Lab Write-Up

---

## Lab Overview

In this lab, I performed a suspicious PowerShell investigation using Microsoft Defender XDR, Microsoft Defender for Endpoint, Advanced Hunting, and KQL.

The purpose of the lab was not simply to see whether Microsoft Defender created an alert. The main goal was to determine what PowerShell actually did on the endpoint.

I generated controlled and harmless PowerShell activity inside my Windows 11 Enterprise Microsoft Defender for Endpoint virtual machine. I then investigated that activity from the Defender portal and correlated process, file, network, user, and timeline telemetry.

The investigation focused on answering questions such as:

- Who executed PowerShell?
- What command was executed?
- Was PowerShell using encoded commands?
- What processes did PowerShell launch?
- What files were created?
- Did PowerShell communicate over the network?
- What destination did it connect to?
- Were there signs of malware, persistence, credential theft, lateral movement, command-and-control, or data exfiltration?
- Was endpoint isolation actually necessary?

---

# Lab Environment

## Main Endpoint

Windows 11 Enterprise MDE virtual machine

Device name:

`desktop-3sepq1q`

Local IP address:

`10.0.2.15`

The Windows 11 Enterprise VM was onboarded into Microsoft Defender for Endpoint and appeared under the Defender device inventory.

The endpoint was connected to the Internet using VirtualBox NAT networking.

Other lab virtual machines such as Kali Linux, Ubuntu, pfSense, DC01, and the regular Windows 11 VM were not needed for this investigation.

---

# Security Tools Used

- Microsoft Defender XDR
- Microsoft Defender for Endpoint
- Microsoft Defender Antivirus
- Advanced Hunting
- Kusto Query Language (KQL)
- Windows PowerShell
- Windows Security
- Defender Device Timeline
- Defender Device Response Actions

---

# Defender Protection Verification

Before generating the lab activity, I verified that Microsoft Defender protections were active.

The important protection values included:

- AntivirusEnabled: True
- RealTimeProtectionEnabled: True
- BehaviorMonitorEnabled: True
- IoavProtectionEnabled: True
- IsTamperProtected: True

I also updated Defender security intelligence before beginning the investigation.

This helped make sure that the endpoint was in a healthy state before generating the test activity.

### Screenshot

`week17_Lab5_Defender_Protection_Status.png`

---

# Network Connectivity Verification

I confirmed that the Windows 11 Enterprise MDE endpoint could reach the Internet over HTTPS.

The test showed that TCP port 443 communication was successful.

This was important because one part of the lab required PowerShell to make a harmless HTTPS connection.

---

# Defender for Endpoint Device Verification

I opened Microsoft Defender and verified that the endpoint appeared under the device inventory.

The Defender portal showed:

- Device name: `desktop-3sepq1q`
- Windows 11 operating system
- Device status: Active
- Defender for Endpoint onboarding
- Risk information
- Device telemetry

### Screenshot

`week17_Lab5_MDE_Device_Overview.png`

---

# Controlled PowerShell Activity

I intentionally generated harmless activity that looked similar to behavior a SOC analyst may investigate.

The activity included:

1. PowerShell encoded command execution
2. File creation
3. PowerShell network communication
4. PowerShell launching CMD
5. CMD launching WHOAMI

The commands were created only for the lab and did not contain malware.

---

# Encoded PowerShell Activity

I created a harmless PowerShell command that wrote the following text into a temporary file:

`Week17 Lab5 PowerShell Investigation`

I converted the command into Base64 using PowerShell-compatible Unicode encoding.

I then executed the encoded command using:

`powershell.exe -NoProfile -EncodedCommand`

The command created:

`week17-lab5.txt`

inside the user's temporary directory.

This was important because encoded PowerShell can appear in both legitimate administration and malicious activity.

The presence of `-EncodedCommand` by itself does not prove malware.

It is an investigation signal that requires more context.

---

# PowerShell Network Activity

I generated a harmless HTTPS web request to:

`https://example.com`

PowerShell downloaded the normal Example Domain HTML page and saved it as:

`week17-example.html`

The file was created inside the user's TEMP directory.

Later, Microsoft Defender network telemetry independently showed that PowerShell made a successful connection.

The Defender evidence showed:

- Initiating process: `powershell.exe`
- Remote URL: `example.com`
- Remote IP: `104.20.23.154`
- Remote port: `443`
- Protocol: `Tcp`
- Action type: `ConnectionSuccess`

This was one of the strongest pieces of evidence in the lab.

A command line containing a URL only shows what a program was instructed to do.

DeviceNetworkEvents showed that the network connection actually occurred.

### Screenshot

`week17_Lab5_PowerShell_Network_Activity.png`

---

# Child Process Activity

I also generated a process chain using PowerShell.

PowerShell launched:

`cmd.exe`

CMD then executed:

`whoami.exe`

The process relationship was:

PowerShell
↓
CMD
↓
WHOAMI

Microsoft Defender telemetry confirmed this relationship.

The results showed:

- `powershell.exe` as the initiating process for `cmd.exe`
- `cmd.exe` as the initiating process for `whoami.exe`

This demonstrated how process ancestry can be reconstructed during an investigation.

### Screenshot

`week17_Lab5_PowerShell_Child_Process_Correlation.png`

---

# Controlled Test Artifacts

The lab created three harmless files:

- `week17-lab5.txt`
- `week17-example.html`
- `week17-user.txt`

These files acted as known ground truth.

I already knew what actions I had performed, and I used Microsoft Defender telemetry to see whether I could independently reconstruct those same actions.

### Screenshot

`week17_Lab5_Generated_Test_Artifacts.png`

---

# Advanced Hunting Investigation

I used Microsoft Defender Advanced Hunting to investigate the generated activity.

The main tables used during the investigation were:

- DeviceProcessEvents
- DeviceFileEvents
- DeviceNetworkEvents
- DeviceEvents
- DeviceRegistryEvents
- DeviceInfo

---

# DeviceProcessEvents

DeviceProcessEvents was used to investigate:

- PowerShell execution
- User context
- Process command lines
- Parent and initiating processes
- Process IDs
- Child processes
- Encoded PowerShell behavior

The investigation found the encoded PowerShell process.

The command line contained:

`-EncodedCommand`

The event was associated with the lab account and the expected Windows 11 Enterprise endpoint.

### Screenshot

`week17_Lab5_Encoded_PowerShell_Evidence.png`

---

# Process Chain Investigation

The most useful process chain found during Advanced Hunting was:

`powershell.exe → cmd.exe → whoami.exe`

The process data showed that PowerShell initiated CMD and CMD later initiated WHOAMI.

This was important because process names alone do not provide enough context.

For example, `whoami.exe` is a normal Windows command.

However, knowing what process caused it to execute provides much more useful investigative information.

### Screenshot

`week17_Lab5_PowerShell_Process_Timeline.png`

---

# DeviceFileEvents

I used DeviceFileEvents to identify files created during the PowerShell activity.

Microsoft Defender successfully recorded:

`week17-example.html`

The event showed:

- ActionType: `FileCreated`
- FileName: `week17-example.html`
- Initiating process: `powershell.exe`
- TEMP directory file path
- Hash information where available

This provided independent file-system evidence supporting the PowerShell web request.

### Screenshot

`week17_Lab5_PowerShell_File_Activity.png`

---

# DeviceNetworkEvents

DeviceNetworkEvents was used to determine whether PowerShell actually made an outbound network connection.

The results showed one matching connection.

The important fields included:

- InitiatingProcessFileName: `powershell.exe`
- RemoteUrl: `example.com`
- RemoteIP: `104.20.23.154`
- RemotePort: `443`
- Protocol: `Tcp`
- ActionType: `ConnectionSuccess`

The connection matched the harmless `Invoke-WebRequest` activity generated earlier.

This helped move the investigation from command-line intent to actual observed network behavior.

---

# DeviceRegistryEvents

I checked DeviceRegistryEvents for PowerShell-related Registry modifications.

The controlled activity was not designed to create persistence or change Registry settings.

No relevant Registry changes connected to the lab activity were identified.

This was useful negative evidence.

It showed that the investigation did not find a Registry-based persistence mechanism related to the test activity.

---

# DeviceEvents

I reviewed DeviceEvents for additional endpoint activity related to PowerShell.

Some Defender and Windows events were visible, but I did not force unrelated activity into the case.

Only evidence that could be connected to the controlled activity was used in the final conclusion.

---

# Defender Device Timeline

I reviewed the device Timeline for:

- PowerShell activity
- Process execution
- Network events
- Defender process information
- Event timestamps
- User context
- Parent and initiating process information

The Timeline provided another way to view activity in chronological order.

I also found PowerShell-related Timeline events that were not clearly connected to my controlled lab activity.

I did not automatically treat those events as part of the case.

This was important because the goal was to follow evidence instead of assuming every PowerShell event belonged to the lab.

### Screenshot

`week17_Lab5_Defender_Device_Timeline.png`

---

# Existing Defender Incidents

The device already contained two incidents:

1. Suspicious PowerShell command line
2. Execution incident on one endpoint

At first, the incident names appeared relevant to the lab.

However, I checked the timestamps before using them as evidence.

Both incidents were from August 6, 2026.

My Week 17 Lab 5 activity occurred on August 31, 2026.

Because the timestamps did not match, I excluded both incidents from the Lab 5 findings.

The "Execution incident on one endpoint" case was already marked:

- Resolved
- Benign Positive

This was an important investigation lesson.

A matching alert or incident name does not prove that it belongs to the activity currently being investigated.

---

# Telemetry vs Alerts

This lab showed an important difference between telemetry and alerts.

Telemetry means:

Security tooling recorded something that happened.

An alert means:

Detection logic decided that the recorded behavior met conditions that were suspicious enough to generate an alert.

My controlled PowerShell activity produced useful endpoint telemetry even though I did not confirm a new Defender incident tied to the test.

The investigation was still successful because Advanced Hunting allowed me to reconstruct the behavior.

---

# Response Options Reviewed

From the Defender device page, I reviewed the available response actions.

The device menu included options such as:

- Collect Investigation Package
- Initiate Automated Investigation
- Initiate Live Response Session
- Isolate Device
- Go hunt

I did not perform device isolation.

### Screenshot

`week17_Lab5_Endpoint_Response_Assessment.png`

---

# Containment Decision

## Decision

Device isolation was NOT required.

The activity initially deserved investigation because it included:

- PowerShell
- Encoded command execution
- Child processes
- WHOAMI system discovery
- File creation
- External HTTPS communication

However, the full investigation showed:

- Known lab endpoint
- Known lab user
- Authorized activity
- Known harmless domain
- No malicious payload
- No persistence
- No credential theft
- No lateral movement
- No command-and-control infrastructure
- No data exfiltration
- No continuing attacker access

Isolating the endpoint would therefore have been unnecessary.

---

# Final Classification

Classification:

Benign / Authorized Security Testing

Initial suspicion:

Medium

Final risk:

Low

Containment:

Not required

---

# What PowerShell Actually Did

The investigation showed that PowerShell performed several controlled actions.

PowerShell executed a harmless Base64-encoded command that created a text file.

PowerShell also made a successful HTTPS connection to `example.com`.

Microsoft Defender recorded the destination as:

`104.20.23.154:443`

The connection was recorded as:

`ConnectionSuccess`

PowerShell also created:

`week17-example.html`

Finally, PowerShell launched CMD, and CMD executed WHOAMI.

The reconstructed activity was:

PowerShell
↓
Encoded command
↓
Controlled file creation

PowerShell
↓
HTTPS connection
↓
example.com
↓
104.20.23.154
↓
TCP 443
↓
ConnectionSuccess
↓
week17-example.html

PowerShell
↓
cmd.exe
↓
whoami.exe

No malicious payload was executed.

---

# Key Lessons Learned

The biggest lesson from this lab was that suspicious activity must be investigated in context.

PowerShell itself is not malicious.

`-EncodedCommand` is not automatically malicious.

WHOAMI is not automatically malicious.

A connection to the Internet is not automatically malicious.

The analyst needs to correlate:

Process activity
+
Command lines
+
User activity
+
File activity
+
Network activity
+
Timeline evidence
+
Other security telemetry

This is how I determined what PowerShell actually did.

Another major lesson was that I should never force evidence to match my original idea.

I found existing suspicious PowerShell incidents, but their timestamps were from August 6 rather than my August 31 lab activity.

I excluded them instead of incorrectly claiming that my lab generated them.

That helped make the investigation more accurate.

---

# Skills Practiced

- Microsoft Defender XDR investigation
- Microsoft Defender for Endpoint
- Advanced Hunting
- KQL
- Process investigation
- Parent and child process analysis
- Encoded PowerShell investigation
- File telemetry correlation
- Network telemetry correlation
- Device timeline review
- Incident validation
- User context analysis
- Risk assessment
- Endpoint containment decision making
- Evidence-based incident response

---

# Final Result

The investigation successfully reconstructed suspicious-looking PowerShell activity across process, file, network, identity, and endpoint telemetry.

The activity was determined to be authorized lab testing.

No evidence showed an actual endpoint compromise.

Final disposition:

**Benign Authorized Security Testing — No Containment Required**