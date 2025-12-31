Week 6 — VPN Traffic Verification & Encryption Validation


1. Context: What Area This Touches & Why It Matters

This work focuses on network traffic visibility and encryption validation.
In real environments, VPNs are often assumed to be secure simply because they are “on,” but assumptions are risky. Verifying that traffic is actually encrypted matters because sensitive data may still be exposed if tunnels are misconfigured, monitored incorrectly, or misunderstood.

This area relates to confirming security controls, not just deploying them.

2. Realistic Scenario: When This Knowledge Is Actually Used

A realistic situation is when remote access is enabled and someone asks, “Are we sure traffic is protected once users connect?” There may be no alert or outage — just uncertainty. Teams still need to confirm whether traffic is encrypted as expected, especially before trusting a VPN for internal access or sensitive activity.

Most real checks start with validating behavior, not reacting to incidents.

3. Thinking Process: How I Approached the Problem

My expectation was that once the VPN was connected, traffic should no longer appear as readable application data. Instead of assuming this, I focused on observing traffic directly. I reopened packet capture and applied filters related to VPN and TLS traffic to reduce noise.

At first, it was tempting to look for specific applications, but I realized that what mattered more was what I should not see — readable HTTP, DNS, or plaintext data. This shifted my focus from “finding traffic” to confirming that encryption was actually working.

4. Signals That Actually Mattered (Evidence Over Noise)

The key signal was seeing OpenVPN/TLS packets with encrypted payloads and no readable application data. The absence of plaintext traffic mattered more than the volume of packets. This confirmed that data was encapsulated inside the tunnel rather than exposed on the network.

5. Decision: What Action Made Sense Based on the Evidence

Based on the capture, the appropriate action was to document the verification and close the check. Encryption was behaving as expected. In a production setting, this confirmation would support allowing continued VPN use while maintaining routine monitoring.

6. Risks, Trade-Offs, and Limitations

If VPN traffic is not verified, teams may assume protection that isn’t actually present. However, deep inspection of encrypted traffic also reduces visibility, which is a trade-off between security and monitoring detail. This lab confirms encryption but does not evaluate endpoint security or authentication strength.

7. Common Beginner Mistake

A common mistake is assuming that a “connected” VPN automatically means traffic is secure. That assumption can hide misconfigurations. Verifying traffic behavior avoids relying on trust alone.

8. One Practical Improvement

A practical improvement would be to document standard verification steps so encryption checks are repeatable and consistent across environments.

9. Summary

This lab reinforced the importance of validating security controls instead of assuming they work. By focusing on traffic behavior and filtering out noise, I confirmed that VPN traffic was properly encrypted. The experience strengthened my understanding of how to verify security claims using evidence. This type of verification builds confidence in real systems and supports responsible decision-making.