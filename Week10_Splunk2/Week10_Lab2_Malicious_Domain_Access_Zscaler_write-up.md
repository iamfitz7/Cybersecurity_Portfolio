Week 10 Lab #2: Malicious Domain Access Zscaler HomeLab (Proxy) Write-Up

1. Context: What Area This Touches & Why It Matters

This work focuses on proxy and DNS visibility, where outbound web traffic can reveal early signs of phishing or malware delivery. Even allowed traffic matters, because users often reach malicious infrastructure before an incident is fully understood.

2. Realistic Scenario: When This Knowledge Is Used

A proxy alert fires showing “Malicious Access,” but the traffic is marked as allowed. There is no immediate incident, but the pattern feels off. The question becomes whether this is noise, user error, or something that needs action.

3. Thinking Process: How I Approached the Problem

I expected to see a small number of random domains. Instead, I noticed repeated domains accessed by multiple users. I focused first on what was allowed, not blocked, because allowed traffic creates risk. At first, I assumed some domains might just be false positives. After reviewing counts and user overlap, I realized some domains were appearing too often to ignore.

4. Signals That Actually Mattered

The strongest signal was repeat access to the same domains by different users, combined with confirmed malicious verdicts from VirusTotal. This mattered more than single hits or domain names alone.

5. Decision: What Action Made Sense

Escalation made sense because:

The evidence was consistent across logs and OSINT

The decision was proportional to the risk

Blocking prevents further exposure while investigation continues

In production, I would escalate with context and recommend immediate blocking.

6. Risks, Trade-Offs, and Limitations

Blocking domains too quickly can interrupt business activity. Not blocking confirmed malicious domains increases risk. This lab focuses on detection and triage, not full incident response.

7. Common Beginner Mistake

A common mistake is trusting a single tool verdict. This can lead to either overreacting or ignoring real threats. Correlating logs with OSINT avoids that trap.

8. One Practical Improvement

Automatically enriching proxy alerts with OSINT results would reduce manual effort and speed up decisions.

9. Summary

This investigation focused on identifying and validating malicious domain access using proxy logs and external intelligence. The work emphasized judgment, correlation, and proportional response. Evidence supported escalation and blocking of confirmed threats. This reflects how early decisions are made in real SOC environments.