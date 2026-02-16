Week 12 Lab #1 - Potential Data Exfiltration Cloud Storage 

1. Context: What Area This Touches & Why It Matters

Organizations rely heavily on proxy and web gateway logs to understand how data moves outside the network. Large outbound transfers to cloud storage can be normal, but they can also hide risky behavior if not reviewed carefully. This matters because data loss often happens quietly, not through obvious alerts.

2. Realistic Scenario: When This Knowledge Is Actually Used

A detection flags unusually high outbound traffic to cloud storage. Nothing is blocked, and the user is not known to be malicious. The question is not “is this an incident,” but “do we understand what is happening well enough to say it’s safe?”

3. Thinking Process: How You Approached the Problem

My first expectation was to see many users with moderate activity. Instead, one user and workstation stood out immediately due to the size of the transfers. I focused on narrowing scope instead of jumping to conclusions. I ruled out random noise early by converting bytes into MB so the activity was easier to understand.

As I pivoted into web logs, I expected user agent data to help confirm whether this was browser activity or scripted uploads. That assumption turned out to be wrong. The user agent field existed, but it was not populated. This forced me to slow down and adjust my conclusion rather than forcing an answer the data could not support.

4. Signals That Actually Mattered (Evidence Over Noise)

Two signals mattered most:

Outbound volume size: multiple transfers in the hundreds to thousands of MB

Destination context: known cloud storage providers with file-based URLs

These signals confirmed that large uploads were happening, even though the method could not be identified.

5. Decision: What Action Made Sense Based on the Evidence

The correct action was to escalate with context. The activity was not clearly malicious, but it also could not be cleared confidently. Escalation allows deeper endpoint review without overreacting.

6. Risks, Trade-Offs, and Limitations

Proxy logs alone do not always show how a transfer was performed. Without endpoint telemetry, it is possible to miss scripted activity. At the same time, escalating every cloud upload would create unnecessary noise.

7. Common Beginner Mistake (Subtle but Powerful)

A common mistake is assuming missing fields mean nothing happened. In reality, missing data often means missing visibility, not safety.

8. One Practical Improvement

Ensure proxy logging includes consistent user agent visibility or is clearly documented when it does not.

9. Summary

This investigation reinforced the importance of scope control, evidence prioritization, and honest documentation of gaps. The outcome was not about proving malicious intent, but about making a responsible decision with incomplete data. This is how real investigations often look in production environments.