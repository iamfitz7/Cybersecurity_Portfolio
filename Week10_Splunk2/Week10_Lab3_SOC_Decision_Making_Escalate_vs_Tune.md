Week 10 Lab #3 - SOC Decision-Making: True Positive vs False Positive vs Tune

1. Context: What Area This Touches & Why It Matters

This work focuses on security monitoring and alert triage inside a SIEM environment.
The core concern is not whether alerts exist, but how to interpret them responsibly and decide what action makes sense based on limited and imperfect information.

In real environments, alerts fire constantly. Some indicate real risk. Others reflect normal behavior. The ability to tell the difference matters because incorrect decisions either waste time or allow real threats to go unnoticed.

2. Realistic Scenario: When This Knowledge Is Actually Used

Imagine coming on shift and seeing multiple alerts in the analyst queue.
None clearly state “this is an incident,” but each suggests something may be wrong.

One alert involves PowerShell activity on a workstation.
Another shows repeated outbound traffic that looks like beaconing.

Nothing is confirmed yet. The question is not “Is this bad?” — it’s “What should we do next, and who should handle it?”

That decision has to be made quickly and carefully.

3. Thinking Process: How I Approached the Problem

I expected both alerts to be suspicious, but not equally actionable.

The first thing I focused on was where the behavior occurred. Endpoint execution immediately stood out compared to network-only patterns. I also paid attention to how specific the behavior was, not just how often it occurred.

At first, Alert B confused me. The traffic looked repetitive, but repetition alone does not confirm compromise. I had to slow down and remind myself that patterns can be noisy, especially in proxy data.

As I reviewed both alerts, my thinking shifted from “Is this malicious?” to “What level of certainty do we actually have, and what should L1 responsibly do?”

4. Signals That Actually Mattered (Evidence Over Noise)

Alert A (Suspicious PowerShell Activity)
The key signal was PowerShell execution using Invoke-WebRequest on an endpoint. This matters because it is a known LOLBAS technique and happens at execution level, not just in traffic logs.

Alert B (Probable Malware Beaconing)
The repeated outbound connections to the same destination stood out. However, there was no direct execution context or payload visibility. The signal suggested possible risk, not confirmed compromise.

In both cases, the quality of the signal mattered more than the quantity.

5. Decision: What Action Made Sense Based on the Evidence

Alert A → Escalate
This alert showed high-risk behavior with clear execution context. Escalating to L2/IR made sense because deeper endpoint analysis was required and beyond L1 scope.

Alert B → Mixed Decision (Escalate + Tune)
The alert justified escalation due to beacon-like behavior, but also showed signs of potential detection noise. This required two paths:

Escalation for validation

Feedback to detection engineering for possible tuning

In production, I would continue monitoring while waiting for higher-tier confirmation.

6. Risks, Trade-Offs, and Limitations

Escalating too aggressively creates alert fatigue.
Not escalating enough creates blind spots.

This lab does not cover host containment or forensic validation, which are critical next steps handled by higher tiers. It also highlights how limited visibility at L1 requires restraint.

7. Common Beginner Mistake 

A common mistake is treating all alerts the same — either escalating everything or closing too quickly.

This is tempting because it feels efficient. In reality, it creates more work later and damages trust in the SOC process. Proper triage means matching response to confidence level.

8. One Practical Improvement

Adding clearer internal notes on what qualifies as “escalate with tuning feedback” would reduce confusion and improve consistency across analysts.

9. Summary

This lab reinforced that SOC work is less about tools and more about judgment.
I learned how to evaluate alerts based on signal quality, execution context, and risk.
Most importantly, I practiced making decisions that respect role boundaries and workflow maturity.
These are the kinds of decisions that prevent both missed incidents and wasted effort.