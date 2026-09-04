# Week 17 — Lab 5: Suspicious PowerShell Endpoint Investigation & Response

## Incident Case Study

---

# Case Information

**Case Type:** Suspicious PowerShell Endpoint Activity

**Environment:** Windows 11 Enterprise / Microsoft Defender for Endpoint

**Investigation Platform:** Microsoft Defender XDR

**Investigation Method:** Advanced Hunting and Endpoint Telemetry Correlation

**Initial Suspicion:** Medium

**Final Risk:** Low

**Final Classification:** Benign / Authorized Security Testing

**Containment Required:** No

**Case Status:** Closed

---

# 1. Executive Summary

Suspicious-looking PowerShell activity was investigated on a Windows 11 Enterprise endpoint monitored by Microsoft Defender for Endpoint.

The activity included several behaviors that could deserve investigation in a real security environment:

* PowerShell execution
* Base64-encoded command execution
* File creation
* Outbound HTTPS communication
* Web content retrieval
* Child-process execution
* User discovery

Because these behaviors can also appear during legitimate administration, automation, and testing, the activity was not immediately classified as malicious.

Microsoft Defender XDR Advanced Hunting was used to investigate process, command-line, identity, file, network, registry, and other endpoint telemetry.

The investigation reconstructed the activity and determined that PowerShell executed a harmless encoded command, created expected temporary files, communicated with `example.com`, retrieved harmless HTML content, launched `cmd.exe`, and executed `whoami`.

No evidence was identified showing malicious payload execution, persistence, credential theft, lateral movement, data exfiltration, command-and-control activity, ransomware, or continued attacker access.

The activity was determined to be authorized security testing.

**Final Classification: Benign / Authorized Security Testing**

**Final Risk: Low**

**Containment: Not Required**

---

# 2. Initial Investigation Question

The case was not approached with the assumption that:

> PowerShell = malicious.

Instead, the main question was:

> **What did PowerShell actually do?**

This required investigating the behavior before deciding whether an incident had occurred.

---

# 3. Initial Indicators

The activity contained multiple indicators that deserved investigation:

```text
PowerShell
+
Encoded Command
+
Web Request
+
File Creation
+
Child Process
+
User Discovery
```

Any one of these behaviors may be legitimate.

When several occur together, additional investigation is reasonable.

The activity was therefore treated as suspicious until enough evidence was collected to make a supported decision.

---

# 4. Affected Asset

The affected system was a controlled Windows 11 Enterprise virtual endpoint monitored by Microsoft Defender for Endpoint.

The endpoint was confirmed to be visible within the Microsoft Defender device inventory.

Microsoft Defender Antivirus and important endpoint protections were also confirmed to be active before the test activity was generated.

This established that the system was operating as a monitored endpoint during the investigation.

---

# 5. Investigation Scope

The investigation focused on determining:

1. Which endpoint generated the activity?
2. Which user executed PowerShell?
3. What process started PowerShell?
4. What PowerShell command was executed?
5. Was the command encoded?
6. What processes did PowerShell start?
7. What files were created?
8. Did PowerShell communicate over the network?
9. What remote destination was contacted?
10. Was anything downloaded?
11. Was a malicious payload executed?
12. Was persistence established?
13. Was the Windows Registry modified?
14. Was credential access observed?
15. Was lateral movement observed?
16. Was data exfiltration observed?
17. Was command-and-control behavior observed?
18. Did the endpoint require containment?

---

# 6. Evidence Sources

The following Microsoft Defender XDR telemetry sources were used during the investigation:

### DeviceProcessEvents

Used to investigate:

* PowerShell execution
* Command lines
* User context
* Initiating processes
* Child processes
* Process IDs
* File hashes when available

### DeviceFileEvents

Used to investigate:

* File creation
* File modification
* File paths
* Initiating processes
* File hashes when available

### DeviceNetworkEvents

Used to investigate:

* Outbound communication
* Remote URLs
* Remote IP addresses
* Remote ports
* Protocol information
* Initiating processes

### DeviceEvents

Used to review additional endpoint and security-control telemetry.

### DeviceRegistryEvents

Used to determine whether PowerShell-related registry activity occurred.

### Defender Device Timeline

Used to reconstruct activity in chronological order.

---

# 7. Process Investigation

Process telemetry showed PowerShell activity on the endpoint.

The investigation focused on the full command line rather than simply the process name.

This was important because:

```text
powershell.exe
```

alone does not explain what occurred.

The command-line evidence showed activity involving encoded PowerShell and web-related PowerShell functionality.

The PowerShell activity was therefore investigated further.

---

# 8. Encoded PowerShell Analysis

An additional PowerShell process executed using:

```text
-EncodedCommand
```

Encoded PowerShell can be important during an investigation because the original command may not be immediately readable from the command line.

Attackers may use encoding to make commands less obvious.

However:

```text
Encoded PowerShell ≠ Confirmed Malware
```

The behavior caused by the encoded command was investigated before reaching a verdict.

The encoded command in this case wrote a harmless lab message into:

```text
%TEMP%\week17-lab5.txt
```

The resulting text was:

```text
Week17 Lab5 PowerShell Investigation
```

No malicious payload was associated with the encoded command.

---

# 9. File-System Investigation

File telemetry and known test artifacts were reviewed.

Three important files were associated with the activity:

```text
week17-lab5.txt
week17-example.html
week17-user.txt
```

### week17-lab5.txt

Created as the result of the encoded PowerShell command.

### week17-example.html

Created after PowerShell performed an HTTPS request to `example.com`.

### week17-user.txt

Created as output from the `whoami` command.

The files were consistent with the expected controlled test activity.

No malicious executable payload was intentionally retrieved or executed.

---

# 10. Network Investigation

Network activity associated with PowerShell was investigated.

PowerShell performed an HTTPS request involving:

```text
example.com
```

The request was associated with PowerShell web functionality and resulted in HTML content being written to:

```text
week17-example.html
```

The network evidence was important because it helped confirm that network communication occurred rather than relying only on a URL found inside a command line.

The destination was consistent with the controlled lab activity.

No command-and-control infrastructure was identified.

---

# 11. Child-Process Investigation

The process investigation identified activity involving PowerShell launching another Windows process.

The expected process chain was:

```text
powershell.exe
      ↓
cmd.exe
      ↓
whoami.exe
```

`whoami` was used to identify the current Windows user and the output was written to:

```text
week17-user.txt
```

User discovery commands can appear during legitimate troubleshooting, administration, and security testing.

They can also appear during attacker reconnaissance.

Because of this, the process chain was investigated in context rather than automatically classified as malicious.

---

# 12. User Context

The PowerShell activity was associated with the known lab user.

Identity information was important because commands can have different levels of risk depending on the account executing them.

For example, activity performed as SYSTEM or by a highly privileged account may require additional attention compared with expected activity from a known user performing an authorized test.

In this case, the user context was consistent with the lab activity.

---

# 13. Registry Review

PowerShell-related registry telemetry was reviewed to determine whether the activity modified the Windows Registry.

Registry activity can be important during an incident because attackers may use registry modifications for:

* Persistence
* Configuration changes
* Defense evasion
* Execution

No relevant registry activity was required to explain the controlled behavior.

No persistence mechanism was identified from the investigated activity.

---

# 14. Timeline Reconstruction

After correlating the available telemetry, the activity was reconstructed approximately as:

```text
Known User Activity
        ↓
PowerShell Executed
        ↓
Encoded PowerShell Executed
        ↓
week17-lab5.txt Created
        ↓
PowerShell Performed HTTPS Request
        ↓
example.com Contacted
        ↓
week17-example.html Created
        ↓
PowerShell Launched cmd.exe
        ↓
whoami.exe Executed
        ↓
week17-user.txt Created
```

This timeline provided a much clearer understanding of the activity than any individual event could provide by itself.

---

# 15. Evidence Supporting Initial Suspicion

The following evidence increased the initial level of interest:

* PowerShell execution
* Encoded PowerShell command
* Network communication
* Web request
* File creation
* Child-process execution
* Command shell execution
* User discovery

In another environment, a similar combination could be associated with attacker activity.

The behavior therefore required investigation.

---

# 16. Evidence Lowering the Risk

The following context lowered the final risk:

* Known lab endpoint
* Known lab user
* Authorized activity
* Controlled execution
* Expected temporary files
* Expected network destination
* Harmless HTML content
* No malicious executable payload
* No persistence identified
* No credential theft identified
* No lateral movement identified
* No data exfiltration identified
* No destructive behavior identified
* No ransomware activity identified
* No command-and-control infrastructure identified
* No evidence of continued attacker control

This demonstrated why context is critical during security investigations.

---

# 17. Risk Assessment

## Initial Suspicion: Medium

The combination of encoded PowerShell, network activity, file creation, child processes, and discovery activity justified further investigation.

## Final Risk: Low

After the available evidence was correlated, the activity matched the expected authorized security test.

There was no evidence supporting an active endpoint compromise.

---

# 18. Containment Assessment

One possible response action was device isolation.

Before taking that action, the following question was considered:

> Is there evidence that leaving this endpoint connected creates an ongoing security risk?

The investigation did not identify evidence of:

```text
Malware Execution
Credential Theft
Active Command-and-Control
Persistence
Lateral Movement
Ransomware
Data Exfiltration
Unknown Payload Execution
Continued Attacker Control
```

Because these higher-risk behaviors were not identified and the activity was confirmed as authorized testing, endpoint isolation was not justified.

### Containment Decision

**Not Required**

---

# 19. Final Classification

### Classification

**Benign / Authorized Security Testing**

### Initial Suspicion

**Medium**

### Final Risk

**Low**

### Device Isolation

**Not Required**

### Case Status

**Closed**

---

# 20. Analyst Conclusion

The investigation began with PowerShell behavior that contained several indicators commonly worth investigating.

Encoded command execution, outbound network communication, file creation, child-process activity, and user discovery could potentially appear during malicious activity.

Instead of treating these indicators as automatic proof of compromise, I investigated the behavior across multiple Microsoft Defender XDR telemetry sources.

Process telemetry showed what executed.

Command-line telemetry showed what PowerShell was instructed to do.

Identity telemetry helped determine who performed the activity.

File telemetry showed the resulting filesystem activity.

Network telemetry helped confirm outbound communication.

Registry and additional endpoint telemetry were reviewed for supporting evidence.

The device timeline helped place the events into chronological order.

After correlating the evidence, the activity was determined to be part of an authorized security lab.

No malicious payload, persistence, credential theft, lateral movement, data exfiltration, command-and-control activity, or continued attacker access was identified.

For those reasons, the endpoint did not require isolation.

The case was classified as:

> **Benign / Authorized Security Testing**

and closed with a final risk level of:

> **Low**

---

# 21. Lessons From the Case

The most important lesson from this investigation was that suspicious indicators require context.

Seeing:

```text
powershell.exe
```

is not enough.

Seeing:

```text
powershell.exe -EncodedCommand ...
```

creates more reason to investigate, but it is still not enough to confirm malicious activity.

The analyst must continue asking questions:

```text
Who executed it?
        ↓
What launched it?
        ↓
What command was executed?
        ↓
What did the command actually do?
        ↓
What processes were launched afterward?
        ↓
What files were created?
        ↓
Where did the process communicate?
        ↓
Was persistence created?
        ↓
Was sensitive information accessed?
        ↓
Did the activity spread to another system?
        ↓
Does the endpoint still present a threat?
```

The final verdict should come from the evidence collected through that process.

---

# 22. Case Takeaway

This case demonstrated the difference between simply identifying suspicious activity and performing an actual investigation.

A basic review might stop at:

> PowerShell used an encoded command.

This investigation continued beyond that point.

I identified the user, examined process relationships, analyzed command lines, investigated file activity, reviewed network communication, checked additional endpoint telemetry, reviewed registry activity, reconstructed the timeline, assessed the risk, and made a containment decision.

The case reinforced one of the most important principles I have learned during endpoint investigations:

> **Start with the evidence, correlate the behavior, and make the final decision based on what the evidence supports.**
