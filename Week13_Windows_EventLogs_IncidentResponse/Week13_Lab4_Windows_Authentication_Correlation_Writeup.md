Week 13 Lab #4 Write up: Windows Authentication Activity Correlation


1. Context: What Area This Touches & Why It Matters

Authentication logs show who is accessing a system and how often attempts are made. When combined with system logs, they give a clearer picture of what is happening on a machine at a specific time. This matters because access behavior is one of the first places where unusual activity shows up.

---

2. Realistic Scenario: When This Knowledge Is Actually Used

A system might show multiple failed login attempts followed by a successful login. At first, this could look suspicious, but it could also be normal behavior. The goal is to understand what actually happened instead of assuming.

---

3. Thinking Process: How You Approached the Problem

At first, I expected everything to work smoothly, including firewall logs. When that did not work, it forced me to adjust and focus on the logs that were available.

I started with Security logs because they directly show login attempts. Then I checked System logs to see if anything unusual was happening at the same time.

One thing that stood out was how easy it is to misread logs if you only look at one source. Combining logs gave me more confidence in what I was seeing.

---

4. Signals That Actually Mattered (Evidence Over Noise)

The main signal was the pattern of multiple failed login attempts followed by a successful login.

This mattered because it showed a sequence instead of isolated events. When I checked System logs during the same time, there was no unusual behavior, which supported the idea that this was normal activity.

---

5. Decision: What Action Made Sense Based on the Evidence

The right decision was to document the behavior and not escalate it.

The activity matched expected behavior, and there were no signs of anything beyond normal login attempts. If this were a real system, I would continue monitoring for repeated patterns over time.

---

6. Risks, Trade-Offs, and Limitations

If login behavior is misunderstood, normal activity can be treated as a threat, or real threats can be ignored. There is also limited visibility since this setup only includes host-level logs and not full network data.

---

7. Common Beginner Mistake

A common mistake is reacting too quickly to failed logins without checking the full context.

This can lead to false alarms. Looking at multiple logs helps avoid that.

---

8. One Practical Improvement

A useful improvement would be setting an alert for multiple failed logins within a short time window.

---

9. Summary

This work focused on analyzing authentication activity using multiple Windows log sources in Splunk. By combining Security and System logs, it was possible to better understand login behavior. The investigation showed how to evaluate patterns without overreacting. This approach reflects how basic log correlation supports decision-making in real environments.

