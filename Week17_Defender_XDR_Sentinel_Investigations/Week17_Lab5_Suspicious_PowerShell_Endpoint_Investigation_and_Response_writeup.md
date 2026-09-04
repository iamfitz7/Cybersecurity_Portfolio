# Week 17 — Lab 5: Suspicious PowerShell Endpoint Investigation & Response

## Technical Analysis Lab Write-Up

---

## 1. Lab Overview

In this lab, I performed an endpoint security investigation involving suspicious-looking PowerShell activity using Microsoft Defender for Endpoint and Microsoft Defender XDR Advanced Hunting.

The main goal of this lab was not simply to detect that `powershell.exe` was running. PowerShell is a legitimate Windows tool that is commonly used by administrators, developers, IT teams, and security professionals. However, attackers can also abuse PowerShell to execute commands, communicate over the network, create files, launch other processes, and perform other actions.

Because of this, seeing PowerShell on a system is not enough to determine that malicious activity occurred.

The main investigation question for this lab was:

> **What did PowerShell actually do?**

To answer this question, I investigated and correlated several types of endpoint telemetry, including:

* Process activity
* Command-line activity
* User context
* Parent and child process relationships
* File activity
* Network activity
* Registry activity
* Device timeline events
* Additional endpoint security events

I then used the collected evidence to determine the risk of the activity and decide whether the endpoint needed to be contained.

---

## 2. Lab Objective

The objectives of this lab were to:

* Generate controlled PowerShell activity on a monitored Windows endpoint.
* Investigate PowerShell execution using Microsoft Defender XDR.
* Identify encoded PowerShell command activity.
* Examine the full PowerShell command line.
* Determine which user executed PowerShell.
* Investigate parent and child process relationships.
* Identify files created during the activity.
* Investigate outbound network connections made by PowerShell.
* Review additional endpoint and registry telemetry.
* Reconstruct the activity using the Defender device timeline.
* Separate suspicious indicators from confirmed malicious behavior.
* Perform a risk assessment based on evidence.
* Determine whether endpoint isolation was justified.
* Document the final investigation findings.

---

## 3. Lab Environment

### Endpoint

* Windows 11 Enterprise virtual machine
* Microsoft Defender Antivirus
* Microsoft Defender for Endpoint
* Microsoft Defender XDR

### Security Platform

* Microsoft Defender portal
* Defender for Endpoint device inventory
* Defender XDR Advanced Hunting
* Defender device timeline

### Query Language

* Kusto Query Language (KQL)

### Main Advanced Hunting Tables

* `DeviceProcessEvents`
* `DeviceNetworkEvents`
* `DeviceFileEvents`
* `DeviceEvents`
* `DeviceRegistryEvents`
* `DeviceInfo`

---

## 4. Investigation Method

I followed an evidence-based investigation process.

The investigation path was:

```text
Suspicious PowerShell Activity
        ↓
Affected Device
        ↓
User Account
        ↓
Parent Process
        ↓
PowerShell Process
        ↓
Command Line
        ↓
Child Processes
        ↓
File Activity
        ↓
Network Activity
        ↓
Additional Endpoint Events
        ↓
Registry Activity
        ↓
Device Timeline
        ↓
Risk Assessment
        ↓
Containment Decision
```

This approach helped me avoid making a decision based only on the name of a process.

---

## 5. Endpoint Security Validation

Before generating the lab activity, I confirmed that Microsoft Defender protections were active on the Windows 11 Enterprise endpoint.

I reviewed important Defender protection settings such as:

* Antivirus protection
* Real-time protection
* Behavior monitoring
* IOAV protection
* Tamper protection
* Security intelligence status

This step was important because I wanted to confirm that the endpoint was in a monitored and protected state before beginning the investigation.

I also confirmed that the Windows endpoint appeared in the Microsoft Defender device inventory.

This verified that Microsoft Defender for Endpoint was receiving information from the system.

---

## 6. Controlled PowerShell Activity

I intentionally generated harmless PowerShell activity that contained several behaviors that could deserve investigation in a real environment.

The activity included:

* PowerShell execution
* Base64-encoded PowerShell
* File creation
* HTTPS communication
* Web content retrieval
* Child-process creation
* User discovery using `whoami`

No malware was installed or intentionally executed.

The purpose was to create realistic endpoint telemetry without creating an actual compromise.

---

## 7. Encoded PowerShell Execution

I first created a harmless PowerShell command that wrote the following text into a temporary file:

```text
Week17 Lab5 PowerShell Investigation
```

The command was converted into Base64 using PowerShell-compatible Unicode encoding.

The encoded command was then executed using:

```powershell
powershell.exe -NoProfile -EncodedCommand $encoded
```

This created:

```text
%TEMP%\week17-lab5.txt
```

This activity demonstrated an important investigation lesson.

`-EncodedCommand` can be suspicious because encoding can make a command harder to immediately understand. However, encoded PowerShell is not automatically malicious.

The command must still be investigated.

---

## 8. Network Activity

I generated controlled network activity using PowerShell's `Invoke-WebRequest`.

PowerShell made an HTTPS request to:

```text
https://example.com
```

The returned HTML was written to:

```text
%TEMP%\week17-example.html
```

This produced multiple pieces of evidence:

```text
powershell.exe
      ↓
HTTPS communication
      ↓
example.com
      ↓
week17-example.html
```

This allowed me to investigate both process activity and actual network telemetry.

It also demonstrated why seeing a URL inside a command is different from confirming that a network connection actually occurred.

---

## 9. Child-Process Activity

I also used PowerShell to start `cmd.exe`.

The command caused CMD to execute `whoami` and write the result to:

```text
%TEMP%\week17-user.txt
```

The process relationship was:

```text
powershell.exe
      ↓
cmd.exe
      ↓
whoami.exe
```

This gave me a process chain to investigate.

Instead of asking only:

> Did PowerShell run?

I could investigate:

> What launched PowerShell, and what did PowerShell launch afterward?

This was an important part of understanding process relationships.

---

## 10. Generated File Artifacts

The controlled activity produced three main files:

```text
week17-lab5.txt
week17-example.html
week17-user.txt
```

These files served as known ground truth for the investigation.

I knew what activity I had generated on the endpoint, and I could then determine whether Microsoft Defender telemetry allowed me to reconstruct the same activity from the security analyst side.

---

# 11. Advanced Hunting Investigation

After allowing time for endpoint telemetry to reach Microsoft Defender, I used Defender XDR Advanced Hunting to investigate the activity.

---

## 12. PowerShell Process Discovery

I started with `DeviceProcessEvents`.

This table allowed me to investigate process execution and related command-line information.

I searched for PowerShell activity and reviewed information such as:

* Timestamp
* Device name
* Account name
* Process name
* Full command line
* Initiating process
* Process ID
* Initiating process ID
* File hash information when available

This established the process portion of the investigation.

---

## 13. Encoded Command Investigation

I narrowed the process search to PowerShell commands containing indicators such as:

```text
-EncodedCommand
-enc
```

This allowed me to locate the encoded PowerShell execution.

The important lesson was that the encoded command was an **investigation signal**, not proof that malware had executed.

I continued investigating the behavior caused by the command before reaching a conclusion.

---

## 14. User Context

I reviewed the `AccountName` and related identity fields associated with PowerShell.

User context matters because the same command can have very different risk depending on whether it was executed by:

* A normal user
* Administrator
* Service account
* Domain administrator
* SYSTEM

In this case, the activity was connected to the known lab user on the controlled test endpoint.

This information helped lower the final risk after the rest of the evidence was reviewed.

---

## 15. Parent and Child Process Analysis

I examined the initiating process information to understand process relationships.

A process name by itself provides limited context.

For example:

```text
powershell.exe
```

does not explain why PowerShell was running.

The initiating process can help show what caused PowerShell to execute.

I also investigated processes started by PowerShell.

The lab generated the following important relationship:

```text
powershell.exe
      ↓
cmd.exe
      ↓
whoami.exe
```

This helped reconstruct the sequence of activity.

---

## 16. File Activity Investigation

I used `DeviceFileEvents` to investigate file-system activity related to the lab.

I searched for:

```text
week17-lab5.txt
week17-example.html
week17-user.txt
```

I reviewed information such as:

* Timestamp
* Device
* Action type
* File name
* Folder path
* Initiating process
* Initiating process command line
* File hashes when available

This allowed me to connect PowerShell and its related processes with file activity.

Instead of only knowing that PowerShell ran, I could investigate what changes occurred on the filesystem afterward.

---

## 17. Network Activity Investigation

I used `DeviceNetworkEvents` to investigate network activity initiated by PowerShell.

Important fields included:

* Timestamp
* Device name
* Initiating process
* Initiating process command line
* Remote URL
* Remote IP
* Remote port
* Protocol
* Action type

The investigation confirmed that PowerShell performed network-related activity associated with the controlled web request.

This was important because network telemetry provides stronger evidence of communication than simply seeing a URL inside a command line.

---

## 18. PowerShell Download Behavior Hunting

I also searched PowerShell command lines for web-related behavior such as:

```text
WebClient
DownloadFile
DownloadData
DownloadString
WebRequest
Invoke-WebRequest
http
https
```

This allowed me to identify the `Invoke-WebRequest` activity.

This demonstrated a more behavior-focused hunting method.

Instead of detecting every instance of:

```text
powershell.exe
```

I searched for PowerShell combined with web-related behavior.

---

## 19. Additional Endpoint Events

I reviewed `DeviceEvents` for additional security-related endpoint telemetry during the investigation window.

Not every telemetry source must produce useful results during an investigation.

A table returning no relevant events does not mean the investigation failed.

It simply means that the telemetry source did not provide additional supporting evidence for that activity and time period.

This reinforced the importance of reporting what the evidence actually shows instead of forcing evidence to match an expected conclusion.

---

## 20. Registry Investigation

I also checked `DeviceRegistryEvents` for PowerShell-related registry activity.

Registry activity can be important because attackers may modify the Windows Registry for persistence or configuration changes.

The controlled PowerShell activity was not designed to establish persistence.

No relevant PowerShell-initiated registry modifications were required to explain the observed activity.

This negative evidence was still useful because it helped determine what PowerShell did **not** do.

---

## 21. Device Timeline Investigation

I reviewed the Microsoft Defender device timeline around the time of the lab activity.

The investigation sequence could be reconstructed approximately as:

```text
User Activity
      ↓
PowerShell Execution
      ↓
Encoded PowerShell Execution
      ↓
Text File Creation
      ↓
PowerShell Web Request
      ↓
HTML File Creation
      ↓
CMD Execution
      ↓
whoami Execution
      ↓
Text File Output
```

The timeline helped connect separate pieces of telemetry into one investigation story.

---

# 22. Evidence Correlation

One of the most important skills practiced during this lab was **endpoint telemetry correlation**.

Instead of relying on one event, I combined:

```text
Process Telemetry
        +
Command-Line Telemetry
        +
User/Identity Context
        +
File-System Telemetry
        +
Network Telemetry
        +
Additional Security Telemetry
        +
Timeline Information
        =
Reconstructed Endpoint Activity
```

This provided much stronger evidence than simply seeing `powershell.exe`.

---

# 23. Suspicious Indicators

Several behaviors initially deserved investigation:

* PowerShell execution
* `-EncodedCommand`
* Outbound web communication
* `Invoke-WebRequest`
* File creation
* Child-process execution
* `cmd.exe`
* `whoami.exe`
* System/user discovery behavior

These behaviors can appear during malicious activity.

However, suspicious behavior is not the same as confirmed malicious behavior.

---

# 24. Context That Changed the Assessment

After correlating the evidence, I identified the following important context:

* Known lab user
* Known Windows test endpoint
* Authorized controlled execution
* Harmless Base64-encoded command
* Expected temporary text files
* Connection to `example.com`
* Harmless HTML retrieval
* No malicious executable payload
* No persistence mechanism identified
* No credential theft identified
* No lateral movement identified
* No data exfiltration identified
* No command-and-control infrastructure identified
* No destructive behavior identified
* No continued attacker control identified

The full context changed the final assessment.

---

# 25. Risk Assessment

### Initial Suspicion

**Medium**

The combination of encoded PowerShell, network communication, file creation, child processes, and user discovery deserved investigation.

### Final Risk

**Low**

The evidence showed that the activity came from an authorized security lab and did not produce signs of an actual endpoint compromise.

### Final Classification

**Benign / Authorized Security Testing**

---

# 26. Containment Decision

### Response Considered

**Device isolation**

### Decision

**Containment not required**

### Reason

The investigated PowerShell behavior was generated as part of an authorized lab exercise.

The investigation did not identify evidence of:

* Malware execution
* Persistence
* Credential theft
* Command-and-control activity
* Lateral movement
* Data exfiltration
* Ransomware
* Unknown payload execution
* Continued attacker control

Isolating an endpoint can interrupt business activity and user access. Because of this, containment should be based on evidence of real or continuing risk rather than performed automatically because suspicious-looking activity was observed.

---

# 27. Final Investigation Conclusion

The investigation started with PowerShell behavior that contained several suspicious characteristics.

The activity included an encoded command, file creation, HTTPS communication, web content retrieval, child-process execution, and user discovery.

I used Microsoft Defender XDR Advanced Hunting to investigate the activity across multiple endpoint telemetry sources.

Process telemetry helped identify PowerShell and its related process activity. Command-line telemetry provided details about what PowerShell was instructed to do. File telemetry helped identify resulting files. Network telemetry helped identify outbound communication. Identity information showed which user executed the activity. Additional endpoint and registry searches helped determine whether other suspicious actions occurred. Finally, the device timeline helped reconstruct the sequence of events.

The investigation confirmed that the behavior was generated by an authorized security lab and did not represent an actual compromise.

**Final Classification:** Benign / Authorized Security Testing

**Initial Suspicion:** Medium

**Final Risk:** Low

**Containment:** Not Required

The final decision was based on the complete evidence rather than the presence of PowerShell alone.

---

# 28. Key Lessons Learned

This lab reinforced several important security investigation lessons.

### PowerShell is not automatically malicious

PowerShell is a legitimate Windows tool. Analysts must investigate what it was used to do.

### Encoded commands require investigation

`-EncodedCommand` can hide the readable command from quick inspection, but encoding alone does not prove malicious intent.

### Command lines provide important context

Seeing `powershell.exe` provides limited information. The full command line can show what PowerShell was actually instructed to perform.

### Process relationships matter

Understanding parent and child processes helps reconstruct how activity started and what happened afterward.

### Network telemetry strengthens an investigation

Seeing a URL in a command is useful, but confirming an actual network connection provides stronger evidence.

### File activity can show the result of execution

Process activity tells me what executed. File telemetry helps show what the process changed or created.

### Negative evidence matters

The absence of persistence, credential theft, lateral movement, exfiltration, and other malicious behavior helped support the final assessment.

### Telemetry is not the same as an alert

Security tools can record activity even when no alert is generated. Analysts can use hunting tools to investigate recorded behavior directly.

### Containment should be evidence-based

A device should not automatically be isolated because suspicious activity exists. The analyst must determine whether there is an active or continuing risk.

### Correlation creates the complete story

The strongest conclusion came from combining process, command-line, identity, file, network, registry, and timeline evidence.

---

# 29. Skills Practiced

This lab provided hands-on practice with:

* Microsoft Defender for Endpoint
* Microsoft Defender XDR
* Advanced Hunting
* KQL
* Endpoint investigation
* PowerShell investigation
* Encoded command analysis
* Process analysis
* Parent/child process correlation
* Command-line analysis
* File-system investigation
* Network investigation
* Identity context analysis
* Device timeline analysis
* Registry investigation
* Behavioral detection
* Endpoint telemetry correlation
* Risk assessment
* Incident classification
* Containment decision-making
* Evidence-based incident response
* Technical documentation

---

# 30. Final Takeaway

The biggest lesson from this lab was that a suspicious process should be treated as the beginning of an investigation, not the end of one.

Finding PowerShell was only the starting point.

The real investigation required me to answer:

```text
Who ran it?
What launched it?
What command did it execute?
What did it launch?
What files did it create?
Where did it communicate?
Did it modify the system?
What happened afterward?
Was the behavior expected?
Does the endpoint still present a risk?
```

By answering those questions with endpoint telemetry, I was able to move from a suspicious PowerShell event to a supported and defensible final conclusion.
