Week 5 Lab — Firewall and IDS Visibility with pfSense & Suricata

1. Context

This lab focused on how firewall rules and intrusion detection systems work together to control and observe network traffic. The core area involved firewall behavior, live network states, and IDS alerting. The main concern was understanding whether traffic that is allowed by firewall rules is still visible to monitoring tools, and how alerts relate to real traffic flows. This matters outside of class because in real environments, teams rely on both firewalls and IDS tools to understand what is happening on the network, not just what is being blocked.

2. Realistic Scenario

A realistic situation for this lab would be a network where outbound traffic appears normal, but security alerts are firing unexpectedly. An administrator might need to confirm whether the firewall is allowing traffic correctly while still giving the IDS enough visibility to detect suspicious behavior. This is not always an active incident as often it starts with a simple question like whether alerts match real traffic activity.

3. Thinking Through the Situation

At the start, I assumed that if a firewall rule allowed traffic, the IDS would automatically detect anything suspicious within it. My first step was to check the LAN firewall rules to confirm what traffic was being allowed and in what order. I then checked Suricata alerts to see whether detection was actually happening. I also looked at the firewall states to confirm that real connections were active. Early on, I was able to rule out routing and connectivity issues because traffic was clearly flowing. As I moved through the lab, I realized the importance of understanding how firewall policy, state tracking, and IDS inspection each play a different role.

4. Signals That Mattered

The most important signal was the Suricata alert showing a potential outbound SSH scan. This stood out because it directly correlated with traffic that had already been allowed by the firewall. The firewall states page also mattered because it confirmed active connections rather than blocked attempts. Other background traffic was less useful and mostly noise.

5. Decision and Outcome

Based on the evidence, the reasonable action would be to document the alert and continue monitoring rather than immediately blocking traffic. The firewall was doing its job, and the IDS was correctly detecting suspicious patterns. For an early-career level decision, validating visibility and documenting findings is the right step.

6. Risks, Limitations, and Trade-Offs

A key risk is assuming that firewall rules alone provide full security. Firewalls control access, but they do not explain intent. IDS tools add visibility but can also generate noise. There is always a trade-off between strict rules and usability.

7. Common Beginner Mistake

A common beginner mistake is thinking that IDS alerts only happen when traffic is blocked. This lab showed that alerts often occur on allowed traffic, which is exactly where monitoring matters most.

8. One Practical Improvement

A practical improvement would be clearer rule descriptions and documentation so alerts can be quickly tied back to specific firewall behavior.

9. Summary

This lab helped me understand how firewall rules, live traffic states, and IDS alerts connect in a real network. I learned how to validate that traffic is allowed while still being monitored. More importantly, it reinforced how to separate signal from noise and make reasonable decisions based on evidence. This type of visibility is critical for maintaining secure and understandable network environments.