Fallback Pivot EDR Curl Python Lab Write Up:

1. Context: What Area This Touches & Why It Matters

This investigation focuses on outbound network traffic and how it is validated during security monitoring. Large data transfers matter because they can point to data loss, misuse of tools, or activity that does not match normal user behavior. In real environments, teams rely on multiple data sources to understand what actually happened.

2. Realistic Scenario: When This Knowledge Is Actually Used

A proxy alert shows unusually high outbound traffic from a single workstation. There is no clear explanation yet. The question becomes whether this traffic came from normal browser use or from a command-line tool like curl or python. The situation is unclear, and it needs validation before taking action.

3. Thinking Process: How You Approached the Problem

I expected to first confirm the size and timing of the outbound transfer using proxy logs. That gave me a clear starting point. Once I had the time window and affected user and host, my next expectation was to pivot to endpoint data to see what actually ran on the system.

I checked whether endpoint telemetry was even available before assuming anything. At first, I expected to find Sysmon or Defender logs, but after running index and keyword checks, it became clear those logs were not present. That changed my approach from confirming execution to documenting the limitation clearly.

4. Signals That Actually Mattered (Evidence Over Noise)

The most important signal was the proxy log showing a large outbound transfer tied to a specific user and host. This mattered because it was repeatable and measurable. Another key signal was the absence of endpoint telemetry, which confirmed that I could not validate process execution within Splunk.

5. Decision: What Action Made Sense Based on the Evidence

The correct decision was to escalate with context. The proxy evidence was strong enough to raise concern, but endpoint confirmation was not possible in this environment. Escalating while clearly stating this limitation is more responsible than making assumptions or closing the alert early.

6. Risks, Trade-Offs, and Limitations

Without endpoint telemetry, it is not possible to confirm whether a tool like curl or python was used. This creates a trade-off between acting quickly and acting with full visibility. This lab also does not cover what endpoint investigation would look like outside of Splunk.

7. Common Beginner Mistake (Subtle but Powerful)

A common mistake is assuming endpoint logs exist just because proxy data does. This can lead to false conclusions. Proper investigation starts by checking what data is actually available.

8. One Practical Improvement (Only One)

Ensure endpoint telemetry such as Sysmon or Defender logs are ingested into Splunk so proxy alerts can be validated more fully in the future.

9. Summary

This investigation showed how important it is to validate data sources before drawing conclusions. Strong proxy evidence was identified, but endpoint confirmation was not possible due to missing telemetry. The alert was escalated with clear documentation of this limitation. This reflects real-world monitoring, where not every question has a clean answer.