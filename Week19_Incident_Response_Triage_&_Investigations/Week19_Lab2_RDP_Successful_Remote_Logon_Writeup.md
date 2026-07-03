RDP Successful Remote Logon Professional Technical Analysis Writeup

This investigation focused on a successful Remote Desktop Protocol authentication alert in Elastic Security. The alert was titled “RDP - Successful Interactive Remote Logon Detected” and was associated with Windows Security Event ID 4624 and Logon Type 10. In Windows environments, Logon Type 10 usually indicates a RemoteInteractive logon, which is commonly tied to RDP access.

The main goal of this investigation was to determine whether the successful RDP login represented expected remote access or possible unauthorized use of the Administrator account.

The alert involved the host workstation-01, the user Administrator, and the source IP address 142.114.44.227. The alert severity was Medium, with a risk score of 47. Because the account involved was Administrator, the event needed careful review even though the severity was not Critical.

I first reviewed the detection rule to understand why the alert fired. The rule was based on Windows Security logs where event.code equals 4624 and winlog.event_data.LogonType equals 10. This confirmed that the alert was not detecting malware, command execution, or persistence. It was specifically detecting a successful RDP-style authentication event.

After confirming the rule logic, I reviewed the alert fields. The key fields showed host.name as workstation-01, user.name as Administrator, process.executable as C:\Windows\System32\svchost.exe, and source.ip as 142.114.44.227. These fields helped define the investigation scope.

The source IP was important because it showed where the remote logon came from. Elastic enrichment showed the source IP was associated with Canada, specifically Ontario, and the network organization was Bell Canada. This information was useful context, but it did not prove whether the login was safe or malicious. IP location and ISP information can help guide an investigation, but they should not be treated as final proof.

Next, I reviewed the event details to confirm the authentication behavior. The event showed Logon Type 10 and Logon Process Name User32. This supported the conclusion that the alert correctly identified a successful RemoteInteractive logon. At this point, the detection itself appeared technically valid.

The next question was whether the successful login was suspicious enough to escalate. To answer that, I pivoted around the host workstation-01, the Administrator account, and the source IP address. I looked for signs of additional suspicious activity around the same time, including related authentication events, process activity, and network activity.

The timeline view showed the authentication event at Jun 26, 2026 @ 13:59:04.746. The event involved svchost.exe, source IP 142.114.44.227, user Administrator, and host workstation-01. This confirmed the central event being investigated.

I also searched for related authentication events, including successful logons, failed logons, logoffs, special privilege assignments, and explicit credential usage. These searches were intended to identify possible brute-force activity, password spraying, or suspicious credential use. Within the available alert data, there was not enough evidence to show a failed-login pattern before the successful RDP logon.

I also reviewed available process and network activity for workstation-01. In a malicious RDP case, I would expect possible follow-on activity such as PowerShell execution, command shell use, discovery commands, file downloads, persistence creation, credential dumping, or suspicious outbound connections. The available evidence did not show enough supporting activity to confirm post-authentication compromise.

The most important finding was that the alert accurately detected a successful RDP logon, but the surrounding evidence did not strongly support malicious activity. The Administrator account and external source IP made the event worth investigating, but those facts alone were not enough to classify the event as a confirmed incident.

The decision I would make for this alert is Benign Positive with Moderate confidence. This means the detection was valid because the RDP login did occur, but the available evidence did not support escalation as confirmed unauthorized access.

I would not call this a false positive because the rule fired on the behavior it was designed to detect. The successful RemoteInteractive logon was real. However, I would also not call it a true positive security incident because there was no strong evidence of malicious follow-up activity in the reviewed telemetry.

In a real environment, I would perform a few additional checks before fully closing the alert. I would confirm whether the Administrator account was expected to use RDP, compare the source IP against known VPN or admin access ranges, check change tickets, review firewall or VPN logs, and confirm with the system owner if needed. I would also review endpoint telemetry for any activity after the login, such as command execution, file changes, new services, scheduled tasks, or unusual network connections.

A common beginner mistake with this kind of alert is assuming that any successful RDP login with an Administrator account is automatically malicious. That can lead to over-escalation and poor alert handling. Another mistake is doing the opposite and dismissing the alert just because RDP is a normal administrative tool. The better approach is to validate the login, review the user, source, host, timing, and post-login behavior, and then make a decision based on the full context.

This lab reinforced the importance of treating alerts as starting points instead of conclusions. The alert provided a useful lead, but the final decision depended on reviewing the rule logic, validating the Windows event fields, checking source and account context, and searching for supporting evidence. Based on the information available, the alert represented a valid detection of successful RDP activity, but there was not enough evidence to confirm malicious access.