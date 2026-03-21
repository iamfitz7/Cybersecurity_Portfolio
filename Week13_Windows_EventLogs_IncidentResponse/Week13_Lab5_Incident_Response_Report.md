Incident Response Report: Unusual Authentication Activity Review

Date: 3/21/2026

Analyst: Fitzgerald Afari-Minta

Summary:
During log review, multiple failed login attempts followed by a successful login were identified on a Windows host. The purpose of the review was to determine whether the activity reflected expected user behavior or if it required escalation. Based on the available evidence, the activity matched normal testing behavior and did not show signs of a larger issue.

Initial Trigger:
Repeated failed login events followed by a successful authentication event were observed in the log data. This raised the question of whether the activity was simply normal user behavior or something that needed a deeper response.

Systems Involved:
- Windows 11 VM
- Windows Security Logs
- Windows System Logs
- Splunk Search & Reporting

Observed Activity:
The main activity observed was a sequence of failed login attempts followed by a successful login event. Security logs showed authentication activity, while System logs provided supporting context during the same time window. No unusual host behavior, service disruption, or additional suspicious activity was observed alongside the login pattern.

Assessment:
Based on the evidence reviewed, the activity appeared to match expected behavior during testing. The failed login attempts were followed by a successful login, and there were no supporting signs of persistence, abnormal system activity, or broader compromise. At this stage, escalation was not justified.

Actions Taken:
- Reviewed authentication-related log activity
- Compared Security logs with System logs in the same time window
- Documented the sequence of events
- Assessed whether the observed behavior required escalation
- Recorded the outcome and recommended monitoring steps

Recommended Next Steps:
- Continue monitoring authentication activity for repeated patterns
- Consider setting a simple alert for multiple failed logins in a short period
- Keep documentation clear so future reviews can be handled faster
- Reassess if the same pattern appears more frequently or on additional systems

Final Status:
Closed with documentation. No escalation required.