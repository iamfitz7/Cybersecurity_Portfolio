Week 4 HomeLab: DNS Lookup Analysis (Wireshark)


Overview

In this lab, I analyzed DNS lookup traffic using Wireshark to understand how domain name resolution occurs at the packet level. The focus was on capturing and interpreting DNS query and response packets to see how a client requests domain information and how a DNS server replies with IP address records. This lab addressed the foundational problem of visibility: understanding what happens on the network before any application-level communication begins. In real production environments, DNS behavior directly impacts connectivity, performance, and security monitoring.

Analysis & Reasoning

The analysis started with the assumption that DNS issues could explain slow connections or failed access to services. I generated a controlled DNS request and filtered traffic to isolate only DNS packets. The first step was examining the DNS query to confirm the requested domain name, query type (A record), and transaction ID. I then matched this request to the corresponding DNS response to verify that the server returned valid IP addresses and appropriate response flags.

Early on, I ruled out application-layer problems since DNS resolution completed successfully. The response timing, answer count, and TTL values all indicated normal behavior. By comparing query and response fields side by side, I confirmed that name resolution was functioning correctly and that traffic could proceed to the next stage of communication.

Outcome & Decision

Based on the evidence, the appropriate action would be to document the DNS resolution as healthy and continue investigating higher-layer traffic if users still experienced issues. Since no anomalies were detected, escalation would not be necessary at this stage.

Risks & Lessons Learned

A misunderstanding of DNS behavior can lead to incorrect troubleshooting, such as focusing on firewalls or servers when the issue is actually name resolution. While DNS is lightweight and fast, it can still introduce delays or failures if misconfigured. The trade-off is that DNS provides useful visibility but also exposes metadata such as domain names, even when application traffic is encrypted.

A common beginner mistake is assuming HTTPS hides all activity. This lab reinforced that DNS requests remain visible unless additional protections are in place.

Improvement Opportunity

An improvement would be enabling DNS logging and alerting for abnormal query patterns, such as repeated failed lookups or requests to suspicious domains. This would improve early detection of misconfigurations or potential security issues.

Summary

This lab strengthened my understanding of DNS as a critical dependency for all network communication. By analyzing DNS lookups at the packet level, I gained practical insight into how resolution works, what normal traffic looks like, and how DNS data supports troubleshooting and security visibility. These skills translate directly to real-world monitoring and investigation scenarios.