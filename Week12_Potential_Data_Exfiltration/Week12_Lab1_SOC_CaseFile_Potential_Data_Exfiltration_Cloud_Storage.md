Case File — Potential Data Exfiltration to Cloud Storage


Alert Summary

High-volume outbound data transfers were observed from a single user and workstation to multiple cloud storage services. The activity was allowed by policy and involved large file uploads, requiring validation and escalation with documented limitations.

Data Sources

Proxy / Web Logs: Splunk index zscaler-soc

Key fields: event_user, event_devicehostname, event_url, event_outbytes, event_dstip, event_action, event_category, user_agent

Timeline

Large outbound transfers observed during a short time window

Scope narrowed to a single user and workstation

Cloud storage destinations identified

File-based URLs confirmed

User agent field reviewed and found not populated

Escalation decision made with visibility limitations documented

Splunk Searches Used

High-volume outbound identification

index=zscaler-soc
| eval out_bytes=tonumber(event_outbytes)
| eval out_mb=round(out_bytes/1024/1024,2)
| sort - out_mb
| table _time event_user event_devicehostname event_department out_mb event_dstip event_action


Web log pivot for scoped user/host

index=zscaler-soc
event_user="alice@corp.local"
event_devicehostname="alice-workstation"
| table _time event_url event_category user_agent event_dstip event_outbytes event_action

Enrichment

Cloud storage domains observed:

storage.google.com

storage.microsoft.com

storage.adobe.com

storage.blackperl.com

storage.openai.com

File types observed:

.png, .pdf, .xlsx, .docx, .mp4

MITRE ATT&CK Mapping

T1567 – Exfiltration Over Web Services

Decision

Escalate with documented limitations

Justification:
Outbound data volume was unusually high and involved multiple cloud storage providers. While the activity could be business-related, the absence of user agent visibility prevents confirmation of whether scripted tools were used. Risk could not be fully ruled out.

Recommended Next Actions

Pull endpoint telemetry from EDR for process execution review

Validate business justification for large cloud uploads

Review cloud storage access policies and thresholds

Monitor for repeat activity from the same user or host