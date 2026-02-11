Alert Summary

High-volume outbound network transfers were observed in Zscaler firewall logs. The activity exceeded expected volume thresholds and required validation, prioritization, and contextual review to determine whether it represented benign usage or potential data exfiltration behavior.

Data Sources

Splunk Index: zscaler-soc

Log Type: Zscaler ZIA Firewall Logs (CSV ingestion)

Fields Used:

event_outbytes

event_user

event_devicehostname

event_department

event_dstip

event_dstcountry

event_action

Timeline (Analyst View)

Data availability and index scope validated

Time range confirmed across full dataset (All time)

Outbound byte field verified as numeric and usable

Byte values normalized to megabytes

Threshold applied to isolate high-volume transfers

Aggregation performed by user, host, department, and destination IP

Results reviewed for prioritization and next-step context

Splunk Searches (Exact)

Index & time validation

| tstats earliest(_time) as earliest latest(_time) as latest where index=zscaler-soc
| convert ctime(earliest) ctime(latest)


Outbound byte distribution

index=zscaler-soc
| stats count min(event_outbytes) as min_out max(event_outbytes) as max_out avg(event_outbytes) as avg_out


High-volume outbound detection

index=zscaler-soc
| eval out_bytes=tonumber(event_outbytes)
| where out_bytes >= 50*1024*1024
| eval out_mb=round(out_bytes/1024/1024, 2)
| sort - out_mb
| table _time event_user event_devicehostname event_department out_mb event_dstip event_dstcountry event_action


Top users

index=zscaler-soc
| eval out_bytes=tonumber(event_outbytes)
| where out_bytes >= 50*1024*1024
| stats sum(out_bytes) as total_out_bytes count as events by event_user
| eval total_out_mb=round(total_out_bytes/1024/1024,2)
| sort - total_out_mb


Top hosts

index=zscaler-soc
| eval out_bytes=tonumber(event_outbytes)
| where out_bytes >= 50*1024*1024
| stats sum(out_bytes) as total_out_bytes count as events by event_devicehostname
| eval total_out_mb=round(total_out_bytes/1024/1024,2)
| sort - total_out_mb


Top departments

index=zscaler-soc
| eval out_bytes=tonumber(event_outbytes)
| where out_bytes >= 50*1024*1024
| stats sum(out_bytes) as total_out_bytes count as events by event_department
| eval total_out_mb=round(total_out_bytes/1024/1024,2)
| sort - total_out_mb


Top destination IPs

index=zscaler-soc
| eval out_bytes=tonumber(event_outbytes)
| where out_bytes >= 50*1024*1024
| stats sum(out_bytes) as total_out_bytes count as events by event_dstip
| eval total_out_mb=round(total_out_bytes/1024/1024,2)
| sort - total_out_mb

Enrichment

No external enrichment was performed at this stage. This analysis focused on volume-based prioritization to identify candidates for deeper investigation (web logs, OSINT, cloud storage context).

MITRE ATT&CK Mapping (Contextual)

TA0010 – Exfiltration

T1041 – Exfiltration Over C2 Channel (contextual)

T1567 – Exfiltration Over Web Services (contextual)

Note: Activity was not confirmed as malicious.

Decision

Action: Document findings and continue monitoring.
Justification:
The transfers exceeded expected thresholds but were allowed and lacked additional indicators of compromise. The results warrant visibility and follow-up rather than escalation.

Next Actions

Pivot destination IPs into web proxy logs

Review user agents and cloud service usage

Establish baseline thresholds for future alerts