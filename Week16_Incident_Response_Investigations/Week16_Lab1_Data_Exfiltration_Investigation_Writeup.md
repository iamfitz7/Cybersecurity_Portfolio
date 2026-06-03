Week #16 Lab 1 Writeup: Data Exfiltration Investigation

1. Context: What Area This Touches & Why It Matters

Data exfiltration investigations focus on proving whether data is leaving an organization without permission. This matters because an alert by itself does not prove that data was stolen. A strong investigation has to connect network activity, user behavior, endpoint evidence, file evidence, and process behavior.

In real environments, this type of investigation is important because attackers may use normal tools like curl, PowerShell, Python, or cloud storage services to move data out. These tools can look normal by themselves, so the real value comes from understanding the full pattern.

2. Realistic Scenario: When This Knowledge Is Useful

A realistic situation would be a security team receiving an alert that a user uploaded a large amount of data to cloud storage. At first, this could be normal business activity. A user may upload work files to a cloud platform, or an application may sync data automatically.

The investigation becomes more serious when the same user appears across multiple departments and locations, uses command-line tools instead of a browser, and has endpoint activity showing file staging. At that point, the main question becomes: is this normal cloud usage, or is someone attempting to steal data?

3. Thinking Process: How I Approached the Problem

I started by treating the alert as a lead, not as the final answer. The alert showed possible data exfiltration through cloud storage, but I still needed to understand whether the activity was actually suspicious.

The first thing I focused on was the Splunk query. The query converted outbound bytes into megabytes, which made the transfer size easier to read. This helped me understand the size of the outbound traffic without getting distracted by large byte values. The query also filtered for transfers above 150 MB, which helped reduce normal web traffic noise.

After that, I looked at the user and device behavior. The user [alice@corp.local](mailto:alice@corp.local) appeared across different departments and locations, including HR, IT, Sales, HQ, Remote, and Branch-2. That stood out because a normal user usually belongs to one department and mostly works from expected locations. This changed my thinking from “possible large upload” to “possible compromised account or lateral movement.”

Then I moved into proxy evidence through ZIA logs. This gave more context about the URLs, file types, user agents, and cloud destinations. Seeing cloud storage URLs was important, but the user agent mattered even more. The presence of curl suggested this was not normal browser-based uploading.

From there, I moved into Elastic EDR to understand what happened on the endpoint. This was where the investigation became stronger. The endpoint showed discovery activity like whoami.exe, PowerShell activity, curl execution, staged files, and process arguments referencing stolen finance data. The process arguments helped answer the question of intent because they showed what the command was trying to do.

One challenge in this lab was not assuming too much too early. Some IP reputation results appeared clean, and some event outcomes were unknown. It would have been easy to stop there and say the evidence was weak. Instead, I had to keep connecting the pieces: Splunk showed the volume, ZIA showed the upload path, Elastic showed the process behavior, and VirusTotal gave threat intelligence context.

4. What Actually Mattered: Signal vs Noise

The biggest signal was not just that data moved out. The strongest signal was the combination of outbound transfer activity, cloud storage destinations, curl user agent behavior, and endpoint process arguments referencing sensitive file names.

The second major signal was the user behavior. Alice appearing across multiple departments and locations did not look normal. That helped support the possibility of account misuse or lateral movement.

The noise was the large amount of general endpoint activity and the clean VirusTotal result. A clean result did not automatically make the destination safe. It only meant that the IP was not currently flagged by vendors. That still required deeper review because attackers can use cloud infrastructure and IPs with little or no reputation history.

5. Decision: What Made Sense Based on the Information

Based on the evidence, the reasonable decision would be to escalate the case for incident response review, document the findings, isolate the host, and disable or rotate the user’s credentials according to company policy.

This decision makes sense because the evidence showed more than one suspicious point. The investigation had network evidence, proxy evidence, endpoint evidence, process evidence, and file staging evidence. In a real environment, I would not close this as benign without user verification, manager confirmation, and deeper review from the incident response or forensics team.

The next step would be to preserve the evidence, identify whether the transfer was successful or partial, confirm what files were involved, and determine whether other hosts or accounts were affected.

6. Risks, Trade-Offs, and Limitations

One risk is assuming that “unknown” or “failed” event outcomes mean no data was lost. In reality, a partial transfer could still expose sensitive information. That is why the outcome field should be reviewed carefully, but not trusted by itself.

Another risk is assuming that cloud storage traffic is normal just because the destination looks like a known provider. Attackers often use legitimate services because they blend into normal traffic.

A limitation of this project is that it uses a controlled environment. In a real company, there would be more logs, more users, more business context, and stricter evidence handling requirements. A real investigation would likely involve DLP, legal, HR, management, and forensics teams.

7. Common Beginner Mistake

A common beginner mistake is focusing only on the alert name. If the alert says “data exfiltration,” it is easy to assume the case is already proven. That is not enough.

A better approach is to prove the case with evidence. The analyst should ask: who did it, what data was involved, where did it go, what tool was used, was it successful, and what should happen next? This avoids weak conclusions and helps build a stronger investigation.

8. One Practical Improvement

One practical improvement would be to create a clear evidence checklist for data exfiltration investigations. The checklist should include destination IPs, URLs, user agent, total bytes, username, hostname, file names, file paths, process arguments, event outcomes, and threat intelligence results.

This would make the investigation easier to document and easier for another teammate to review.

9. Summary

This project helped me understand how data exfiltration investigations are built from multiple sources of evidence. I practiced connecting Splunk results, ZIA proxy logs, VirusTotal enrichment, Elastic EDR process activity, and staged file evidence. The main lesson was that no single alert proves the full story. A strong investigation comes from careful correlation, clear documentation, and reasonable decisions based on the evidence.
