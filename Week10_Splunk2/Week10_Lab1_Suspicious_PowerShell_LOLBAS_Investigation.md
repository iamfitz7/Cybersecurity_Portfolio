Week 10 Lab #1: Suspicious PowerShell LOLBAS Investigation

1. Context: What Area This Touches & Why It Matters

This work focuses on endpoint monitoring and command-line visibility. PowerShell is widely used in Windows environments, which makes it both powerful and risky when misused.

Understanding how legitimate tools can be abused matters because many real threats rely on built-in system utilities rather than obvious malware.

2. Realistic Scenario: When This Knowledge Is Actually Used

A security alert fires for PowerShell activity, but nothing immediately looks malicious. The domain names seem normal, and no malware is detected.

Still, something feels off. The question becomes whether this is normal automation or behavior that should be taken more seriously.

3. Thinking Process: How I Approached the Problem

At first, I expected to find either clearly malicious URLs or obvious false positives. Instead, the behavior sat in a gray area.

I focused on how the command was executed rather than what it downloaded. Hidden execution and encoded commands stood out. I also compared activity across hosts to see if this was a one-off or a pattern.

My understanding shifted from “this looks normal” to “this is intentionally hiding itself,” which changed how I evaluated the risk.

4. Signals That Actually Mattered

The most important signal was the PowerShell command line itself. Flags like -nop and -w hidden, combined with encoded commands and misleading output file names, mattered more than the domain reputation.

These details helped separate signal from noise.

5. Decision: What Action Made Sense

The correct action was to escalate with context. The behavior did not confirm a full incident, but it clearly justified deeper investigation.

In a real environment, the next step would involve endpoint review and script validation.

6. Risks, Trade-Offs, and Limitations

PowerShell is heavily used by administrators, so overreacting can cause noise. At the same time, ignoring hidden execution risks missing real threats.

This lab focused on investigation and decision-making, not response or containment.

7. Common Beginner Mistake

A common mistake is trusting reputation tools too much. Clean VirusTotal results can create false confidence when the execution behavior itself is suspicious.

8. One Improvement That Could Be Made

Restricting encoded PowerShell execution and improving alert documentation would reduce risk while keeping visibility intact.

9. Summary

This investigation improved my ability to read command-line behavior and make decisions when evidence is unclear. Instead of relying on tool output alone, I focused on intent, execution context, and consistency across systems. This approach reflects how real SOC work often starts—with uncertainty and careful judgment, not clear answers.