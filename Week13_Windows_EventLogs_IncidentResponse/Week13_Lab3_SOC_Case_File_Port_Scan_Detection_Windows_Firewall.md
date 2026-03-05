Week 13 Lab #3: SOC Case Study
Detecting a Port Scan Using Windows Firewall Logs

Overview

In this case study, I simulated a basic network reconnaissance scenario and investigated the resulting firewall logs to understand how a port scan appears in system logging. The goal was to practice detecting suspicious network behavior using built-in Windows logging rather than relying only on security tools.

The activity involved generating network traffic from a Kali Linux virtual machine and observing how Windows Firewall recorded the connection attempts. By reviewing the firewall log file, I was able to identify patterns that matched automated port scanning behavior.

Environment

The lab environment consisted of two virtual machines:

Attacker System

Kali Linux

IP address: 192.168.56.101

Target System

Windows 11

IP address: 192.168.56.102

Both systems were connected on the same virtual network.

Investigation Framework

Throughout this lab I followed a simple investigation approach:

Observe → Validate → Investigate → Interpret → Document

This structure helped guide the process instead of randomly checking logs or commands.

Step 1 — Environment Validation

Before generating any traffic, I first verified that the two systems could communicate.

From Kali Linux I ran a ping test to confirm the Windows system was reachable.

Evidence showed:

Successful ICMP responses

0% packet loss

consistent latency

This confirmed the network connection was functioning and that the test traffic would reach the target machine.

Step 2 — Generating Reconnaissance Traffic

Once connectivity was confirmed, I ran an Nmap scan from the Kali machine.

The command used was:

nmap -sS -Pn -T3 -p 1-1024 192.168.56.102

This type of scan attempts to identify open services by sending connection attempts to multiple ports.

Even though Nmap reported that the ports were closed or ignored, the activity still generated network traffic that the firewall could log.

Step 3 — Enabling Firewall Logging

To observe the behavior properly, I enabled logging inside Windows Defender Firewall with Advanced Security.

Logging settings were configured to record:

dropped packets

successful connections

The firewall log file was stored at:

%system32%\LogFiles\Firewall\pfirewall.log

Once logging was enabled, I opened the log file in Notepad to monitor network activity.

Step 4 — Investigating the Firewall Log

After running the scan, I reviewed the log file for patterns.

Several entries appeared showing repeated dropped connection attempts.

Example entries included:

DROP TCP 192.168.56.102 192.168.56.101 34856 445
DROP TCP 192.168.56.102 192.168.56.101 34858 445
DROP TCP 192.168.56.102 192.168.56.101 34856 135
DROP TCP 192.168.56.102 192.168.56.101 34856 139

Key indicators observed:

repeated connection attempts

multiple destination ports

same source host

timestamps within the same second

The targeted ports were:

445 (SMB)

135 (RPC)

139 (NetBIOS)

These ports are commonly checked during Windows network reconnaissance.

Signal vs Noise

One important part of the investigation was separating useful information from normal system traffic.

The log file also contained entries related to:

loopback traffic (127.0.0.1)

DNS requests

outbound HTTPS connections

These were expected system activities and not related to the scan.

The meaningful signals were the repeated dropped TCP attempts involving the Kali IP address and multiple Windows service ports.

Conclusion

Based on the firewall log evidence, the activity matched the pattern of an automated port scan.

The scan attempted connections to several Windows service ports in rapid succession. The Windows firewall blocked these attempts, and the dropped packets were recorded in the firewall log.

This lab demonstrated how reconnaissance activity can still be detected through system logs even when a firewall blocks the traffic.

Key Takeaways

This exercise helped reinforce several important concepts:

network scans create identifiable patterns in logs

firewall logs can provide useful detection signals

validating connectivity is important before troubleshooting

separating normal traffic from suspicious activity is a key investigation skill

Even simple logging sources like Windows Firewall can provide valuable insight when investigating network behavior.