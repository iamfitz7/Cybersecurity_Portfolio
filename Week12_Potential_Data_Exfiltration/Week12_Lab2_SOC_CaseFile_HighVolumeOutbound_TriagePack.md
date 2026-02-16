Week 12 Lab #2 Case File - High Volume Outbound Transfer Triage Pack (Zscaler)

Alert Summary

Multiple high-volume outbound transfer events were observed in Zscaler firewall logs. The goal of this investigation was not to confirm data exfiltration, but to build a structured triage workflow that identifies:

Top users by outbound volume

Top hosts involved

Department-level spikes

Repeated destination IPs

Time windows with abnormal traffic

The purpose was to create a repeatable method for prioritizing outbound traffic investigations.

Data Sources

Splunk Index: zscaler-soc

Log Type: Zscaler ZIA firewall logs (CSV ingestion)

Fields Used:

event_outbytes

event_user

event_devicehostname

event_department

event_dstip

event_action

_time

Timeline (Workflow Progression)

Validated index and confirmed events returned

Confirmed required fields existed (outbytes, user, host, dept, dstip)

Converted raw bytes → MB for analyst readability

Ranked users by total outbound MB

Ranked hosts by total outbound MB

Ranked departments by outbound MB

Ranked destinations and measured blast radius

Identified hottest 15-minute spike window

Built ≥150MB threshold view for fast suspicious filtering

SPL Searches Used
1. Data Validation
index=zscaler-soc
| head 20

2. Field Validation
index=zscaler-soc
| stats count as events 
count(eval(isnotnull(event_outbytes))) as has_outbytes 
count(eval(isnotnull(event_user))) as has_user 
count(eval(isnotnull(event_devicehostname))) as has_host 
count(eval(isnotnull(event_department))) as has_dept 
count(eval(isnotnull(event_dstip))) as has_dstip

3. Bytes → MB Conversion
index=zscaler-soc
| eval out_bytes=tonumber(event_outbytes)
| eval out_mb=round(out_bytes/1024/1024,2)
| where isnotnull(out_mb)
| table _time event_user event_devicehostname event_department out_mb event_dstip event_action

4. Top Users
index=zscaler-soc
| eval out_bytes=tonumber(event_outbytes)
| eval out_mb=round(out_bytes/1024/1024,2)
| stats sum(out_mb) as total_out_mb count as events by event_user
| sort - total_out_mb
| head 10

5. Top Hosts
index=zscaler-soc
| eval out_bytes=tonumber(event_outbytes)
| eval out_mb=round(out_bytes/1024/1024,2)
| stats sum(out_mb) as total_out_mb count as events by event_devicehostname
| sort - total_out_mb
| head 10

6. Top Departments
index=zscaler-soc
| eval out_bytes=tonumber(event_outbytes)
| eval out_mb=round(out_bytes/1024/1024,2)
| stats sum(out_mb) as total_out_mb count as events by event_department
| sort - total_out_mb
| head 10

7. Top Destinations + Blast Radius
index=zscaler-soc
| eval out_bytes=tonumber(event_outbytes)
| eval out_mb=round(out_bytes/1024/1024,2)
| stats sum(out_mb) as total_out_mb 
count as events 
dc(event_user) as unique_users 
dc(event_devicehostname) as unique_hosts 
by event_dstip
| sort - total_out_mb
| head 10

8. Hot Time Windows
index=zscaler-soc
| eval out_bytes=tonumber(event_outbytes)
| timechart span=15m sum(out_bytes) as total_out_bytes
| eval total_out_mb=round(total_out_bytes/1024/1024,2)
| sort - total_out_mb

9. Threshold View (≥150MB)
index=zscaler-soc
| eval out_bytes=tonumber(event_outbytes)
| eval out_mb=round(out_bytes/1024/1024,2)
| where out_mb >= 150
| sort - out_mb
| table _time event_user event_devicehostname event_department out_mb event_dstip event_action
| head 20


Evidence Observed

One user (alice@corp.local
) generated significantly higher outbound volume than peers

One host (alice-workstation) accounted for the majority of outbound MB

IT and HR showed higher department-level volume

A clear 15-minute spike window was visible

Threshold view isolated 5 high-volume events immediately

⚖ Decision + Justification

No confirmed exfiltration was declared.

The outcome of this lab was to:

Document prioritized entities

Identify spike windows

Flag high-volume events for deeper investigation

This workflow allows faster escalation decisions in real environments.


Recommended Next Steps for IR

Review user activity during spike window

Correlate with proxy URL + user agent logs

Validate business justification with department owner

Check endpoint telemetry (if available)

Monitor for recurrence