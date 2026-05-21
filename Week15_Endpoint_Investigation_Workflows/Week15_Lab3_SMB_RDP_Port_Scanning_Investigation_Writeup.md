Technical Analysis Write-Up: SMB and RDP Port Scanning Investigation

1. Context: What Area This Touches & Why It Matters

Remote access and file-sharing services are important parts of many business networks, but they can also become major risk areas when they are scanned or abused. SMB and RDP are common examples because they are tied to file access, remote login, server administration, and internal movement across systems.

In real environments, activity against ports like SMB and RDP needs to be reviewed carefully. Not every scan is automatically malicious, but repeated connection attempts from one source to several internal systems can suggest reconnaissance, misconfiguration, or possible lateral movement behavior.

This type of investigation matters because defenders need to know whether the activity is normal administration, routine scanning, or something that needs escalation.

2. Realistic Scenario: When This Knowledge Is Useful

A realistic situation would be a security alert showing that one internal IP address connected to several systems over SMB or RDP-related ports. At first, the activity may not clearly prove an attack. It could be a scanner, an admin tool, a system management process, or an attacker trying to find reachable machines.

The main question would be:

Is this expected internal behavior, or does this show possible lateral movement?

That question cannot be answered from one alert alone. It requires checking the source IP, target systems, authentication activity, service creation events, endpoint behavior, and raw logs. This is where SIEM, case management, and EDR evidence become important together.

3. Thinking Process: How I Approached the Problem

My first thought was to avoid assuming that the alert was automatically malicious. The alert was about potential SMB and RDP port scanning from a single source, so I expected to see one system contacting multiple target systems on ports related to file sharing or remote access.

I started by validating the alert logic in Splunk because I wanted to confirm that the detection was based on real events and not just a case title. The important thing was to check whether one source IP was actually reaching more than five unique targets. Once I saw source IP `10.1.2.45` connected to multiple destination IPs, the alert became more meaningful.

After that, I wanted to understand whether the network activity was connected to authentication behavior. Port scanning alone tells me that connection attempts happened, but authentication logs help explain whether accounts were also involved. I reviewed Windows Security events such as 4624 and 4625 to compare successful and failed logons. This helped show that the activity was not only network-level; there were also account and host signals to review.

One thing that made the investigation more challenging was separating normal remote administration activity from suspicious behavior. RDP and SMB can be used by legitimate admins, so I had to be careful not to overreact. I also had to keep reminding myself that a single event does not tell the full story. A failed login, a successful login, or a service creation event matters more when it connects to the same source IP, time period, or affected host.

As I continued, I checked for signs of remote execution or service creation. This is where events like 4697, 7045, and 4688 became important. Seeing PsExec-style activity and service executables running from `C:\Windows\Temp` changed my understanding of the case. The investigation was no longer only about port scanning. It started to show possible remote execution behavior across multiple systems.

I then moved into Elastic to review endpoint-level behavior. This helped me see process relationships, such as `mstsc.exe`, `powershell.exe`, and `PSEXESVC.exe`. Elastic gave more context than the raw Splunk tables alone because I could view process relationships and endpoint details more clearly.

4. What Actually Mattered: Signal vs Noise

The first major signal was the source IP `10.1.2.45` reaching multiple internal targets. This mattered because the alert was based on one system scanning or connecting to more than five unique target IPs on SMB/RDP-related ports. That helped confirm that the alert logic matched the activity.

The second major signal was the service/process evidence. The appearance of suspicious service activity, temporary executable paths, and PsExec-style behavior mattered more than normal background noise. Windows environments generate many normal events, but executables running from `C:\Windows\Temp` and service creation events connected to remote access behavior are stronger signals.

Normal logins, system events, and background Windows activity could have been distracting. The key was not to list everything, but to focus on the evidence that helped answer the main question: did this look like simple scanning, or was there more activity connected to remote movement?

5. Decision: What Made Sense Based on the Information

Based on the evidence, the reasonable decision would be to escalate the case for deeper review. The activity included more than just a port scan. There was network scanning behavior, authentication activity, service creation evidence, PsExec-style process activity, and endpoint evidence in Elastic.

A fair conclusion would be:

Potential SMB/RDP scanning from a single source showed enough supporting evidence to justify escalation for possible lateral movement investigation.

In a real environment, the next steps would include confirming whether the source system was authorized to perform scanning or administration, reviewing the affected accounts, checking whether PsExec usage was expected, and validating whether the services and temporary executables were approved. I would also recommend reviewing endpoint containment options carefully before isolating anything, especially if the affected hosts support important business functions.

6. Risks, Trade-Offs, and Limitations

One risk is assuming that all SMB or RDP activity is malicious. Many organizations use remote administration tools, file shares, and internal management services every day. Blocking or isolating systems too quickly could interrupt normal business work.

Another risk is going the other direction and treating the activity as harmless because SMB and RDP are common. Attackers also use common tools and normal-looking services to move through networks. That means the investigation needs balance.

This project also has limitations. It does not fully simulate the pressure of a real incident bridge, legal review, business approval, or live containment decision. It also depends on the available logs in the environment. If endpoint telemetry or authentication logs were missing, the conclusion would be weaker.

7. Common Beginner Mistake

A common beginner mistake is stopping after the first alert and saying, “This is lateral movement,” without validating the evidence. Beginners may see a scary alert name and immediately assume the worst.

That can cause problems because alerts are not final conclusions. They are starting points. A better approach is to validate the detection, identify the source and targets, review authentication behavior, check endpoint process activity, and then make a careful decision.

This project helped me understand that good investigation is not about reacting quickly to the alert name. It is about building a clear story from several pieces of evidence.

8. One Practical Improvement

One practical improvement would be to create a standard investigation checklist for SMB/RDP scanning alerts. The checklist could include:

- Validate source IP and target count
- Review destination ports
- Check authentication events
- Review service creation events
- Look for PsExec or remote execution indicators
- Pivot into EDR for process relationships
- Document known facts, unknowns, and recommended next steps

This would make the investigation more repeatable and reduce the chance of missing important evidence.

9.  Summary

This project strengthened my understanding of how incident investigations move from alert validation into deeper endpoint review. I practiced separating network scanning evidence from authentication and process execution evidence. The most important lesson was that one alert is not enough to make a strong conclusion. A careful investigation needs multiple data points, clear documentation, and a decision that matches the evidence.