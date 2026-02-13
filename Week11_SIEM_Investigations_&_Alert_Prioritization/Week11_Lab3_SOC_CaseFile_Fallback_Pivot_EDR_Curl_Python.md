Case File - Fallback Pivot EDR Curl and Python Lab


Alert Summary

A high-volume outbound data transfer was observed through proxy logs for a single user and workstation. A fallback pivot to endpoint telemetry was attempted to confirm whether non-browser tools such as curl.exe or python.exe executed during the same time window.

Data Sources

Proxy / Web Logs

Splunk index: zscaler-soc

Fields: event_user, event_devicehostname, event_dstip, event_outbytes

Splunk Platform Metadata

Index inventory (tstats)

Sourcetype inventory

Endpoint Telemetry (Attempted)

Sysmon / Windows Event Logs

CrowdStrike / Microsoft Defender (not present)

Timeline

High outbound traffic identified from proxy logs

Investigation window narrowed around highest data transfer event

Index inventory run to identify endpoint telemetry

Keyword and sourcetype checks performed for EDR data

No endpoint telemetry found in Splunk environment

Escalation documented with visibility limitation noted

Splunk Searches Used

Outbound volume validation

index=zscaler-soc
| eval out_bytes=tonumber(event_outbytes)
| eval out_mb=round(out_bytes/1024/1024,2)
| sort - out_mb
| table _time event_user event_devicehostname event_department out_mb event_dstip event_action


Index inventory

| tstats count where index=* by index
| sort - count


EDR keyword check

index=* (crowdstrike OR falcon OR defender OR "Microsoft Defender" OR sysmon OR "WinEventLog")
| head 20

Enrichment

No external enrichment performed

Endpoint telemetry not available in environment

MITRE ATT&CK Mapping

T1041 – Exfiltration Over C2 Channel (proxy-level indicator only)

T1071 – Application Layer Protocol (web-based data transfer)

Decision

Escalate with limitation noted

There is strong proxy evidence of high outbound transfer activity. However, endpoint telemetry required to confirm tool execution (curl/python) is not available in this Splunk environment. The escalation includes this limitation clearly.

Recommended Next Actions

Validate endpoint activity using EDR outside of Splunk

Review business justification for destination IPs

Correlate with file upload logs if available

Improve endpoint log ingestion for future investigations