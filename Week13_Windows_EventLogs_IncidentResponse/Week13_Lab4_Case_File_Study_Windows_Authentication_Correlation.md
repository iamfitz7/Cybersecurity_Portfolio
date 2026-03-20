Case File: Windows Authentication Activity Correlation (Week 13 Lab #4)

Alert Summary

Multiple failed login attempts followed by a successful login were observed on a Windows system. The goal was to determine whether this behavior was expected or required further attention.

---

Data Sources

* Splunk (Search & Reporting)
* Windows Security Logs (EventCode 4624, 4625)
* Windows System Logs

---

Timeline

* System activity generated on Windows host
* Failed login attempts recorded (EventCode 4625)
* Successful login recorded (EventCode 4624)
* System logs captured events during the same timeframe
* Logs ingested into Splunk
* Queries used to isolate authentication activity
* Logs combined for correlation analysis

---

 Splunk Searches

**Authentication Activity**

```
source="WinEventLog:Security" (EventCode=4624 OR EventCode=4625)
```

**System Logs**

```
source="WinEventLog:System"
```

**Combined Logs**

```
(source="WinEventLog:Security" (EventCode=4624 OR EventCode=4625)) OR source="WinEventLog:System"
```

**Correlation Table**

```
(source="WinEventLog:Security" (EventCode=4624 OR EventCode=4625)) OR source="WinEventLog:System"
| eval signal=case(EventCode=4625,"AUTH_FAIL", EventCode=4624,"AUTH_SUCCESS", source="WinEventLog:System","SYSTEM_EVENT")
| stats count as events values(signal) as signals by host
| sort - events
```

---

 Enrichment

* No external enrichment used
* Focus remained on internal log validation

---

 MITRE ATT&CK Mapping

* T1110 – Brute Force (failed logins)
* T1078 – Valid Accounts (successful login)

---

 Decision

No escalation required.

The activity matched expected behavior. Failed logins followed by a successful login aligned with normal user interaction during testing.

---

 Next Actions

* Monitor repeated failed login patterns
* Consider basic alerting for login thresholds
