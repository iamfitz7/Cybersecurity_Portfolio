Lab: Wireshark Filter Practice (Protocol, IP, Port)


Overview

In this lab, I analyzed captured network traffic using Wireshark display filters to isolate specific protocols, IP addresses, and service ports. The goal was to move beyond raw packet collection and practice narrowing large volumes of traffic down to only what is relevant for investigation or troubleshooting. This lab focused on understanding how filtering improves visibility and speeds up decision-making in real environments.

Analysis & Reasoning

I started with the assumption that most captured traffic is background noise and that useful signals are often buried within it. Rather than reviewing packets sequentially, I applied targeted display filters to progressively narrow the dataset.

First, I filtered by protocol to isolate DNS traffic, which immediately revealed name resolution activity tied to recent browsing and command-line actions. From there, I filtered by IP address to focus on traffic involving a known external host (8.8.8.8), allowing me to view all communication to and from that system regardless of protocol. Finally, I filtered by port number to isolate HTTP traffic on port 80, which exposed request and response behavior tied to web access.

Throughout this process, I evaluated which packets contributed meaningful context versus which could be safely ignored, reinforcing the importance of signal-to-noise awareness.

Outcome & Decision

By applying protocol, IP, and port-based filters, I was able to quickly isolate relevant traffic without modifying or discarding the original capture. In a real environment, this approach supports faster triage and reduces unnecessary escalation by confirming whether observed behavior is expected or abnormal.

Risks & Lessons Learned

A common risk is reviewing unfiltered traffic and missing key indicators due to volume overload. Another risk is assuming filters remove data rather than temporarily hiding it, which can lead to incorrect conclusions if misunderstood.

Improvement Opportunity

An improvement would be pairing display filters with consistent documentation or saved filter profiles to standardize analysis across investigations. This would reduce time spent recreating filters and improve consistency between analysts.

Professional Summary

This lab strengthened my ability to isolate relevant network activity using Wireshark display filters. By filtering traffic by protocol, IP address, and port, I improved efficiency and clarity during analysis. These skills directly support faster troubleshooting and more confident decision-making in operational network environments.