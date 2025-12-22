Wireshark HomeLab: Combined TCP / UDP / DNS / HTTP Traffic Analysis


Overview

This lab focused on analyzing how multiple core network protocols work together during a single client request. Using Wireshark, I captured and reviewed DNS, TCP, UDP, and HTTP traffic in one continuous timeline to understand how name resolution, connection setup, and data transfer interact in real network activity. The goal was to move beyond isolated protocol analysis and instead observe how real traffic flows across layers.

In production environments, issues rarely involve only one protocol. Understanding how these protocols relate helps reduce guesswork during troubleshooting and security investigations.

Analysis & Reasoning

I began by examining DNS traffic to confirm how domain names were resolved before any connection attempts occurred. Once an IP address was returned, I observed the TCP three-way handshake that established a reliable connection to the destination host. After the session was established, HTTP request and response packets showed how application-layer data was transferred over the existing connection.

UDP traffic was also visible in the capture, reinforcing the contrast between connectionless and connection-oriented communication. By keeping the capture unfiltered initially, I was able to follow the exact order of events and confirm that each step logically depended on the previous one.

This approach helped rule out false assumptions, such as application issues caused by DNS failures or transport-layer problems masking as HTTP errors.

Outcome & Decision

The combined view made it clear that when DNS resolution and TCP handshakes complete successfully, HTTP issues are more likely tied to application logic or server behavior. In a real environment, this insight would support a decision to monitor the application layer rather than escalate a network connectivity issue prematurely.

Risks & Lessons Learned

Misinterpreting one protocol in isolation can lead to incorrect conclusions. For example, focusing only on HTTP errors without confirming DNS or TCP behavior could result in wasted effort and delayed resolution. There is also a trade-off between visibility and data volume, as full captures can become noisy without a structured analysis approach.

Improvement Opportunity

A realistic improvement would be implementing targeted capture filters or logging at key points (DNS resolution failures, incomplete TCP handshakes) to reduce noise while preserving useful signals during investigations.

Professional Summary

This lab strengthened my ability to analyze network traffic as a complete system rather than individual pieces. By following traffic from DNS resolution through transport setup and application data exchange, I gained a clearer understanding of how network dependencies affect performance and reliability. This type of end-to-end analysis supports more accurate troubleshooting and better operational decisions.