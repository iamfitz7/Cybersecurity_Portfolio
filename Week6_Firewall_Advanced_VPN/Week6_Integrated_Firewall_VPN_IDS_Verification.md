Week 6 — Combined Firewall + VPN + IDS Lab



1. Context: What Area This Touches \& Why It Matters

This work touches secure remote connectivity and network traffic visibility. In real environments, VPNs are often treated as “set it and forget it,” but teams still need to verify that traffic is actually protected once it leaves an endpoint. If encryption is assumed but not validated, sensitive data can be exposed without anyone realizing it. Understanding how to confirm encrypted traffic matters for both security assurance and troubleshooting.



2. Realistic Scenario: When This Knowledge Is Actually Used

A realistic situation would be a report that remote users are connected through a VPN, but someone wants confirmation that their traffic is truly encrypted on the network. There may be no alerts or obvious failures, just uncertainty. In these cases, validating traffic behavior becomes important before trusting the connection, especially when sensitive systems are involved.



3. Thinking Process: How You Approached the Problem

I expected that once the VPN was active, traffic would no longer appear as readable protocols like HTTP or DNS. My first focus was confirming the VPN connection itself, then observing how traffic appeared once it was running. Initially, it was confusing because the expected tunnel interface did not appear in Wireshark. That forced me to rethink my assumption that encryption verification depends on interface names rather than packet content. I shifted focus from where traffic appeared to how it appeared.



4. Signals That Actually Mattered (Evidence Over Noise)

The most important signal was seeing UDP traffic on the VPN port with TLS/OpenVPN indicators and no readable payload. Even though traffic was visible, the contents were encrypted and unreadable. That confirmed encryption was working as intended. The absence of plaintext protocols mattered more than the volume of packets.



5. Decision: What Action Made Sense Based on the Evidence

Based on the encrypted traffic patterns, the appropriate action was to document the verification and close the check. In a real environment, this would provide confidence that VPN controls are functioning. Continued monitoring would only be needed if behavior changed.



6. Risks, Trade-Offs, and Limitations

If teams assume VPN encryption without verifying it, sensitive data could be exposed. The trade-off is that encrypted traffic reduces visibility into payloads, which can make troubleshooting harder. This lab also focuses only on traffic encryption, not authentication strength or endpoint security.



7. Common Beginner Mistake

A common beginner mistake is assuming that not seeing a tunnel interface means encryption failed. In reality, encryption can still be verified by packet behavior and protocol indicators. Understanding this avoids false conclusions.



8. One Practical Improvement

A practical improvement would be documenting a standard process for verifying VPN encryption using packet analysis, so checks can be repeated consistently without guesswork.



9. Summary

This work reinforced the importance of validating security controls instead of assuming they work. By focusing on traffic behavior rather than interface names, I was able to confirm encryption in a realistic way. It strengthened my ability to reason through uncertainty and focus on meaningful signals. This approach reflects how real systems are verified, not just configured.

