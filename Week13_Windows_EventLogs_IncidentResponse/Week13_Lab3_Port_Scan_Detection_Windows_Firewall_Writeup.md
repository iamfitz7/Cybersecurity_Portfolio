Week 13 Lab #3: Detecting a Port Scan Using Windows Firewall Logs

Lab Objective

The goal of this lab was to understand how port scanning behavior appears in system logs and how firewall logging can be used to detect reconnaissance activity.

Instead of only running a scan, the focus was on observing how the operating system records the activity and how those logs can be interpreted during an investigation.

Lab Setup

Two virtual machines were used in this lab.

Kali Linux acted as the source of the scanning traffic, while Windows 11 acted as the target system.

Before starting the investigation, it was important to confirm that the systems were actually communicating on the same network.

Step 1 — Confirm Network Connectivity

The first step was verifying that the Kali system could reach the Windows system.

I ran the following command from Kali:

ping -c 4 192.168.56.102

The results showed that the Windows machine responded successfully with no packet loss.

This step was important because if the machines could not communicate, any later troubleshooting could become confusing.

Step 2 — Identify the Kali IP Address

Next, I verified the IP configuration on the Kali system using:

ip a

This showed that the Kali machine was assigned the address:

192.168.56.101

Knowing this address was important because I needed to identify it later in the firewall logs.

Step 3 — Generate the Port Scan

Once connectivity was confirmed, I ran an Nmap scan against the Windows system.

sudo nmap -sS -Pn -T3 -p 1-1024 192.168.56.102

The scan completed successfully and reported that all scanned ports were closed or filtered.

Even though the ports were closed, the scan still generated multiple connection attempts that the firewall could detect.

Step 4 — Enable Firewall Logging

Initially, the firewall logs did not show the traffic I expected. This created some confusion during the lab.

After reviewing the firewall configuration, I realized logging needed to be enabled.

Inside Windows Defender Firewall with Advanced Security, I configured the logging settings to record:

dropped packets

successful connections

Once logging was enabled, the firewall began writing entries to the log file.

Step 5 — Reviewing the Firewall Log

The log file was located at:

C:\Windows\System32\LogFiles\Firewall\pfirewall.log

Opening the log in Notepad allowed me to review the connection attempts.

At first, the log contained a lot of normal system activity, including DNS traffic and local connections. These entries were not related to the scan.

After scrolling through the log, I located entries showing dropped TCP traffic involving the Kali IP address.

Step 6 — Identifying the Scan Pattern

Several log entries showed repeated connection attempts targeting different ports.

Examples included:

DROP TCP 192.168.56.102 192.168.56.101 34856 445
DROP TCP 192.168.56.102 192.168.56.101 34856 135
DROP TCP 192.168.56.102 192.168.56.101 34856 139

These ports correspond to common Windows services that attackers frequently scan.

The timestamps showed the attempts occurring very close together, which is typical for automated scanning tools.

Challenges During the Lab

A few issues occurred while completing this exercise.

One challenge was that the firewall logs initially contained a large amount of normal traffic. This made it difficult to quickly identify the relevant entries.

Another challenge was ensuring that firewall logging was correctly enabled. Without enabling dropped packet logging, the scan attempts would not appear in the log file.

These issues required slowing down and reviewing the configuration step-by-step rather than assuming the problem was with the scan itself.

Final Outcome

After enabling logging and reviewing the log file carefully, the port scan activity became visible.

The repeated dropped TCP entries targeting Windows service ports confirmed that the scan traffic had reached the system and was being blocked by the firewall.

Key Lessons

This lab reinforced several important investigation habits.

First, verifying connectivity prevents unnecessary troubleshooting later.

Second, system logs often contain a large amount of normal activity, so identifying useful signals requires careful review.

Finally, even basic firewall logs can reveal important information about network reconnaissance attempts.