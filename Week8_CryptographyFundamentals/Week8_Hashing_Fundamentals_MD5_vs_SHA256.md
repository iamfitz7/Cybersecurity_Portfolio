Week 8 — Hashing Fundamentals (MD5 vs SHA256)

1. Context: What Area This Touches & Why It Matters

Hashing is a core part of system integrity and security monitoring. It’s used to verify files, detect unauthorized changes, and confirm that data has not been altered in transit or at rest. In real environments, hashes are often used quietly in the background for file integrity monitoring, malware analysis, and alert validation. If hashes are misunderstood, teams can miss tampering or misunderstand what a mismatch actually means.

This area matters because many security decisions depend on knowing whether data is the same or has changed, even when the change is very small.

2. Realistic Scenario: When This Knowledge Is Actually Used

A realistic situation is when a system reports that a file hash has changed overnight. There’s no obvious outage, and the file still opens normally, but monitoring flags it as different. At that point, the question isn’t “is the system down,” but rather “did something modify this file, and should we trust it?”

This kind of situation starts with uncertainty. Something feels off, but there’s no clear failure yet. Hashing helps validate whether a change really occurred and whether it should be investigated further.

3. Thinking Process: How I Approached the Problem

Going into this lab, I expected that hashing would simply produce a fixed value for a file and that changing the file would update that value. What surprised me was how dramatic the change was when only one character was added.

At first, I struggled with basic setup issues. I ran into terminal input problems caused by quoting errors, which made it hard to even modify the file correctly. That forced me to slow down, reset the terminal, and be more careful about how I entered commands. Once that was fixed, I focused on keeping the environment clean so I could trust the results.

As I observed the outputs, my understanding shifted from “hashes identify files” to “hashes are extremely sensitive integrity checks.” Even a tiny change completely rewrote the hash, which reinforced why hashes are reliable for detecting tampering but useless for hiding content.

4. Signals That Actually Mattered 

The most important signal was not the hash value itself, but how much it changed.

After adding a single character (!) to the file:

The MD5 hash changed completely

The SHA256 hash also changed completely

There was no visible similarity between old and new hashes

This confirmed the avalanche effect. I didn’t need to compare every character of the hash — seeing that everything changed was enough to confirm file modification.

5. Decision: What Action Made Sense Based on the Evidence

Based on the evidence, the correct action would be to document the hash change and identify what modified the file. Since the network and system were functioning normally, this wasn’t an availability issue. It was an integrity issue.

In a real environment, the next step would be correlating this hash change with logs or scheduled tasks, not immediately assuming malicious activity. The evidence supports investigation, not panic.

6. Risks, Trade-Offs, and Limitations

A major risk is assuming hashes provide confidentiality. They don’t. Anyone can hash the same file and get the same result.

Another limitation is that this lab doesn’t cover where a change came from — only that a change occurred. Hashing detects modification, but additional tools are needed to explain the cause.

7. Common Beginner Mistake

A common beginner mistake is thinking that MD5 and SHA256 are interchangeable. MD5 is fast but weak and vulnerable to collisions, while SHA256 is much stronger and still trusted for integrity checks.

Understanding this prevents teams from relying on outdated or unsafe hashing methods in environments where integrity actually matters.

8. One Practical Improvement

A simple improvement would be to pair hashing with basic file change logging. Hashes show that a file changed, but logs help explain when and why it changed. Together, they provide better context without adding unnecessary complexity.

9. Professional Summary

This lab helped solidify how hashing is used to verify integrity rather than hide data. By comparing MD5 and SHA256 and observing the avalanche effect firsthand, I learned how even a small change can be reliably detected. The challenges I ran into reinforced the importance of clean inputs and controlled testing. Overall, this strengthened my understanding of how hashes support real security decisions without needing to overcomplicate the process.

MD5 vs SHA256


MD5 is fast but outdated and collision-prone. It’s useful for quick checks but not trusted for security decisions.

SHA256 is stronger, longer, and far more resistant to collisions, making it the preferred choice for integrity monitoring today.

Both serve the same purpose and it's detecting change. However, only SHA256 should be trusted in modern environments.