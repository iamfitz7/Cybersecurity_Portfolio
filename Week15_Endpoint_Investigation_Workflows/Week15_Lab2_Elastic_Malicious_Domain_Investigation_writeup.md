Week 15 Lab#2 Write up: Cybersecurity Lab — Elastic + Splunk Malicious Domain Investigation

## 1. Context: What Area This Touches & Why It Matters

This project focuses on incident investigation, SIEM correlation, endpoint analysis, and case documentation. In real environments, security teams often receive alerts that look suspicious, but the alert alone does not prove what happened. The important part is knowing how to validate the activity, connect evidence from different tools, and decide what action makes sense.

This type of work matters because a single alert can be incomplete. A domain may look malicious, a process may look unusual, or a user may appear connected to suspicious activity, but the full picture only becomes clear when different pieces of evidence are reviewed together.

 2. Realistic Scenario: When This Knowledge Is Useful

A realistic situation for this kind of investigation would be an alert showing that a user accessed a malicious domain. At first, it may not be clear whether the activity was a real compromise, a false positive, or just blocked traffic with no impact.

The investigation would need to answer several questions:

- Which users accessed the domain?
- Was the domain confirmed as malicious by outside sources?
- Did the activity connect to suspicious endpoint behavior?
- Was there a suspicious process involved?
- Did the same behavior affect more than one user or machine?
- Should the case be escalated, contained, or monitored?

This matters because security work often starts with uncertainty. The goal is not to panic or assume the worst. The goal is to check the evidence carefully and make a reasonable decision.

 3. Thinking Process: How I Approached the Problem

My first thought was that the malicious domain alert needed to be validated before treating it as a confirmed incident. I did not want to assume that the alert was automatically correct just because it appeared in the security platform.

I started by looking at the case details and the observables. The alert showed malicious domain access, so I focused first on the domains and users involved. I expected to find either one clear malicious domain or a group of domains that needed to be checked one by one.

The next thing I focused on was reputation checking. I used VirusTotal results to see whether the suspicious domains were being flagged by multiple vendors. This helped separate stronger indicators from weaker ones. If only one vendor flagged a domain, I would be more careful. But when multiple vendors flagged a domain as malicious or phishing, that made the domain more important.

After that, I moved from domain evidence into log correlation. I wanted to know whether the suspicious domain activity connected to any user or endpoint activity. In Splunk, I looked at DNS and CrowdStrike-style logs to connect domain activity with users, processes, command lines, and hashes.

One thing that stood out was suspicious `svchost.exe` activity. At first, `svchost.exe` by itself is not automatically suspicious because it is a normal Windows process. But the path and behavior mattered. Seeing `svchost.exe` connected to unusual command-line activity, suspicious URLs, and hash evidence made it much more important.

I also had to be careful not to overstate what I found. The evidence suggested suspicious behavior, but I still needed to connect it to endpoint activity. That is why I reviewed Elastic Security process information and later checked Task Manager to verify process IDs. This helped move the investigation from “suspicious domain alert” to “endpoint behavior that supports escalation.”

 4. What Actually Mattered: Signal vs Noise

The first major signal was the domain reputation evidence. Domains such as `br-icloud.com.br` and the suspicious `signin` domain were flagged by multiple VirusTotal vendors. This mattered because it gave outside support that the domains were not just random traffic.

The second major signal was the process behavior. The suspicious activity was tied to `svchost.exe`, but not in a normal way. The process appeared connected to suspicious command-line behavior, hash evidence, and execution from an unusual location such as a user temp path. That mattered more than the process name alone.

A lot of noise existed in the investigation. For example, normal Windows systems have many `svchost.exe` processes running. Task Manager showed many entries, and not all of them were suspicious. The important part was not simply seeing `svchost.exe`; it was matching the specific PID, execution path, user context, and supporting evidence from SIEM and EDR logs.

 5. Decision: What Made Sense Based on the Information

Based on the evidence, the reasonable decision was to escalate the case for further response. The case had enough supporting evidence to justify escalation:

- malicious domain reputation from VirusTotal
- multiple users tied to suspicious domain activity
- suspicious `svchost.exe` behavior
- hash evidence connected to the process
- endpoint evidence from Elastic
- process ID verification through Task Manager
- memory dump strings showing suspicious domain artifacts

This decision made sense because the evidence was not based on one log source only. It came from case management notes, domain reputation, Splunk searches, endpoint telemetry, Task Manager validation, and memory dump review.

In a real environment, the next steps would be to contain the affected endpoint, block the malicious domains, preserve evidence, rotate credentials for affected users, and send the case to a deeper response team for full analysis.

 6. Risks, Trade-Offs, and Limitations

One risk in this type of investigation is overreacting too early. If an analyst isolates a machine before confirming the evidence, it could interrupt business operations without enough reason. At the same time, waiting too long can allow malicious activity to continue.

Another trade-off is visibility. Splunk and Elastic gave useful information, but each tool showed only part of the picture. Splunk helped with correlation, while Elastic helped with endpoint process behavior. Neither one should be treated as the full truth by itself.

This project also does not fully simulate a complete production environment. In a real company, there would likely be more logs, more users, more systems, legal considerations, and formal approval steps before containment. The memory dump review was also basic and focused on finding readable strings, not full malware reverse engineering.

7. Common Beginner Mistake

A common beginner mistake is assuming that a process name alone proves something is malicious. For example, seeing `svchost.exe` does not automatically mean compromise, because Windows uses many normal `svchost.exe` processes.

The better approach is to ask:

- Where did it run from?
- Who ran it?
- What was the parent process?
- What command line was used?
- What network activity happened around it?
- Does the hash or domain reputation support the concern?

This avoids jumping to conclusions. It also helps separate normal Windows behavior from suspicious behavior that only looks obvious after context is added.

8. One Practical Improvement

One practical improvement would be to create a clear checklist for domain-based investigations. The checklist should include:

- validate domain reputation
- identify affected users
- identify affected hosts
- check command-line activity
- review process path
- review hash evidence
- pivot into endpoint telemetry
- document the decision clearly

This would make the investigation more repeatable and help reduce missed steps.

9. Summary

This project helped me practice how to move from an alert to a more complete investigation story. I learned that the strongest evidence came from connecting domain reputation, user activity, process behavior, endpoint data, and memory dump findings. The biggest lesson was that one alert does not tell the whole story, and a careful investigation depends on knowing which signals matter. This strengthened my understanding of incident response, SIEM correlation, endpoint investigation, and calm decision-making.