Malicious Domain Access Zscaler SOC Case File 

Alert Summary

Proxy logs identified multiple users accessing domains categorized as Malicious Access that were allowed through the proxy. Several domains showed repeat access patterns and were flagged by external threat intelligence sources.

Data Sources

Splunk Index: zscalar-soc

Source Type: Zscaler proxy logs (CSV)

Fields Used:
dns_req, user, category, reqaction, count

External Intelligence:
VirusTotal, urlscan.io

Timeline

Zscaler logs ingested into Splunk

Malicious Access events identified where traffic was allowed

Domains extracted and counted by user

Domain list exported to CSV

Domains deduplicated and manually classified

OSINT checks performed on suspicious domains

Verdict reached based on combined evidence

Splunk Searches Used
index="zscalar-soc"
| rex field=_raw "dns_req\": \"(?<dns_req>[^\"]+)\""
| rex field=_raw "user\": \"(?<user>[^\"]+)\""
| rex field=_raw "reqaction\": \"(?<reqaction>[^\"]+)\""
| rex field=_raw "category\": \"(?<category>[^\"]+)\""
| search category="Malicious Access" AND reqaction="Allowed"
| stats count by dns_req user
| sort - count

OSINT Enrichment
Confirmed Malicious

signin.eby.de.zukruygxctzmmqi.civpro.co.za

Flagged by multiple vendors on VirusTotal

Indicators consistent with phishing infrastructure

br-icloud.com.br

High detection count across VirusTotal vendors

Associated with phishing and fraud activity

Not Confirmed / Low Confidence

malicious-application-update.com

phishing-site.org

ransomware-site.io

No current vendor detections

Classified as not malicious at this time, but noted for monitoring

MITRE ATT&CK Mapping

T1071.001 – Application Layer Protocol: Web Traffic

T1566 – Phishing (infrastructure indicators)

Decision

Escalate + Block

The combination of:

Multiple affected users

Repeated access patterns

Confirmed malicious verdicts from OSINT

supports escalation to block domains and notify stakeholders.

Next Actions

Block confirmed malicious domains at the proxy

Notify impacted users

Add unresolved domains to monitoring or tuning list

Review why malicious traffic was allowed