DNS Query and Response Analysis (Wireshark)

Overview
In this lab, I captured and analyzed DNS query and response traffic using Wireshark in a Kali Linux environment. The goal was to understand how domain names are resolved into IP addresses and how this process appears at the packet level. DNS is a foundational network service, and visibility into DNS traffic is critical for both troubleshooting connectivity issues and identifying suspicious behavior.

Scenario
A common situation where this knowledge is applied is when a user reports that a website is not loading or is redirecting unexpectedly. Before assuming an application issue, it is important to confirm whether DNS resolution is functioning correctly. Abnormal DNS responses, unexpected IP addresses, or excessive queries can indicate misconfigurations or potential security concerns.

Analysis and Reasoning
After starting a packet capture, I generated DNS traffic by performing a domain lookup using the dig command. I then filtered traffic in Wireshark to isolate DNS packets. The initial focus was identifying the DNS query sent from the client and the corresponding response from the DNS server. By reviewing packet details, I confirmed that the query was successful and that valid IP addresses were returned. Non-DNS traffic was filtered out to reduce noise and focus only on relevant signals.

Evidence Observed
The DNS query packet showed the requested domain name, while the response packet contained multiple IP addresses associated with the domain. The response code indicated a successful lookup, confirming normal DNS behavior. These packet-level details provide clear evidence of how name resolution occurs and how responses are delivered to the client.

Outcome and Decision
Because the DNS query and response completed successfully, the appropriate action in a real environment would be to document normal behavior and continue investigating other layers if connectivity issues persist. This confirms that DNS is not the source of the problem and helps narrow the scope of troubleshooting.

Risks and Lessons Learned
If DNS behavior is misunderstood or ignored, legitimate issues may be misdiagnosed, or malicious activity such as domain spoofing or command-and-control traffic could go unnoticed. A common beginner mistake is assuming DNS is always functioning correctly without verifying packet-level evidence.

Improvement Opportunity
An improvement to this setup would be enabling DNS logging or monitoring tools to track query patterns over time. This would improve visibility and help identify anomalies more quickly.

Summary
This lab reinforced my understanding of the DNS query and response process and how to validate name resolution using packet captures. It demonstrated how DNS traffic supports both operational troubleshooting and security analysis. Documenting and analyzing DNS behavior at the packet level builds a strong foundation for more advanced network and security work.
