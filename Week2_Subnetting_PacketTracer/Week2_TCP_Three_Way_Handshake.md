Week 2 — TCP Three-Way Handshake Analysis (Wireshark)

Overview

In this lab, I analyzed a TCP three-way handshake using Wireshark to understand how reliable connections are established between systems at the network level. By capturing live traffic and isolating a single TCP stream, I observed the SYN, SYN-ACK, and ACK packets that form the foundation of TCP communication.

This activity addresses a common operational need: validating whether a system is successfully establishing TCP connections. In real environments, many application, network, and security issues trace back to failed or abnormal handshakes, making this analysis essential for troubleshooting and investigation.

Analysis & Reasoning

To generate a predictable TCP connection, an HTTP request was initiated from the system. The initial assumption was that a normal connection would produce a clean three-packet handshake before any application data was exchanged.

The first step in analysis was isolating a single TCP conversation using a stream filter. This removed unrelated traffic such as DNS and background network noise, allowing focus on one client-server interaction. From there, packet flags and sequence behavior were examined to confirm the handshake flow.

Early in the analysis, issues such as packet loss or retransmissions were ruled out because the SYN, SYN-ACK, and ACK packets appeared in the correct order with no resets or delays. The presence of valid sequence numbers and acknowledgments confirmed that both endpoints successfully synchronized state and agreed to communicate.

The most useful signals during this process were TCP flags, sequence numbers, and acknowledgment values. Other traffic visible in the capture was treated as noise and intentionally ignored to maintain clarity.

Outcome & Decision

The observed handshake completed successfully, indicating that the network path, endpoint configuration, and TCP behavior were functioning as expected. In a real operational setting, this result would support closing a connectivity issue or shifting investigation to higher layers, such as application logic or server-side processing.

For an early-career practitioner, the appropriate action here would be to document the findings, preserve the capture for reference, and escalate only if handshake anomalies were detected.

Risks & Lessons Learned

Misunderstanding the TCP handshake can lead to incorrect conclusions about outages or security events. A common beginner mistake is assuming that any failed connection is an application issue without first verifying whether the TCP session was ever established.

Another risk is analyzing too much data at once. Without isolating a single TCP stream, important indicators can be buried in unrelated traffic, leading to missed signals or wasted time.

Improvement Opportunity

One improvement to this setup would be enabling more structured logging or alerting around failed handshakes, such as tracking repeated SYN attempts or incomplete sessions. This would improve visibility and allow faster detection of connectivity problems or potential scanning behavior.

Professional Summary

This lab reinforced how TCP establishes reliable communication and how packet-level analysis can be used to validate or troubleshoot connectivity issues. By isolating a single TCP stream and examining handshake behavior, I practiced identifying meaningful signals while filtering out noise. This approach supports clear decision-making, proper documentation, and efficient escalation when issues fall outside basic network behavior. The skills practiced here are directly applicable to real-world troubleshooting and security investigations.