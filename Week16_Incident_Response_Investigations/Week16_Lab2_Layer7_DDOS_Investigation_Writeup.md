
Week 16 Lab #2 Write Up — Layer 7 DDoS Investigation

A large portion of modern web infrastructure depends on applications remaining available and responsive to users. When traffic suddenly increases, organizations must determine whether the activity represents legitimate demand or malicious behavior. Understanding how to validate abnormal traffic is important because availability issues can impact customers, business operations, and security teams.

A realistic situation might involve a web application that suddenly becomes slow or unavailable. An alert may indicate a possible denial-of-service attack, but an alert alone does not prove anything. The challenge is determining whether the increase in traffic is normal user behavior, a system issue, or a deliberate attack. Before making any conclusions, the activity must be investigated and validated using available evidence.

My initial focus was not proving a DDoS attack. Instead, I wanted to understand whether traffic was actually behaving abnormally. I started by looking at request volume over time and comparing activity against expected patterns. When I observed a significant increase in requests, I knew something unusual had occurred, but that alone was not enough to confirm malicious activity. The next question became whether the traffic was coming from one source or many sources.

As I continued investigating, the most important finding was that the traffic originated from multiple IP addresses rather than a single host. This immediately changed the direction of the investigation. I also examined which resources were being targeted and found repeated requests against the homepage, login page, and API endpoints. These locations require server-side processing and can consume significant resources when accessed excessively.

Another important observation involved HTTP error activity. Increased error rates suggested the application was struggling to process requests successfully. I also reviewed User-Agent data and found indicators associated with automated tools rather than typical browser activity. These findings helped separate meaningful evidence from normal web traffic.

Based on the information available, the most reasonable conclusion was that the environment was experiencing a Layer 7 HTTP flood attack. Multiple sources were generating requests against common application resources while error rates increased and automated traffic indicators appeared. In a production environment, this would justify escalation, documentation, and coordination with infrastructure teams for mitigation.

One limitation of this investigation is that logs only provide part of the overall picture. Additional evidence such as firewall logs, load balancer telemetry, network captures, and infrastructure monitoring would likely be reviewed during a real incident. Security teams must balance visibility, investigation speed, and operational impact when responding to these situations.

A common beginner mistake is assuming that a large spike in traffic automatically means a DDoS attack. High traffic can also be caused by legitimate events, marketing campaigns, automated updates, or business activity. Effective investigation requires validating the source, distribution, targets, and impact before reaching a conclusion.

One practical improvement would be implementing automated baseline monitoring and alerting for web traffic behavior. Earlier detection of significant deviations could reduce response time and improve visibility into developing attacks.

This project strengthened my understanding of how web-based denial-of-service investigations are performed. More importantly, it reinforced the importance of validating evidence rather than trusting alert names alone. By examining traffic volume, source distribution, targeted resources, application impact, and automation indicators, I was able to build a more complete picture of what was occurring and make a reasonable investigative decision based on the available data.