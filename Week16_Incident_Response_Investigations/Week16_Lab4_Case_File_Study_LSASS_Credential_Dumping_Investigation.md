Security Operations Case File: LSASS Credential Dumping Investigation

## Case Title

Possible Credential Dumping via LSASS and External Upload Activity

- Case Summary

This case focused on investigating suspicious activity related to LSASS memory access, credential dumping, and possible data exfiltration. The investigation began with an alert for possible credential dumping on SRV-DC01. The main goal was to determine whether the alert was a false positive, attempted credential dumping, or successful credential theft.

The investigation found several important indicators, including Procdump execution, LSASS dump file creation, Rundll32 activity, Curl upload behavior, and external storage communication. When these events were reviewed together, the activity appeared consistent with a credential dumping and exfiltration attempt.

- Initial Alert

The investigation began with an alert titled:

Possible Credential Dumping via LSASS detected on SRV-DC01

The alert stated that sensitive account credentials may have been extracted from LSASS process memory. Because LSASS contains sensitive authentication data, this alert required careful review.

- Systems and Accounts Involved

Affected host:

SRV-DC01

Observed account:

[svc_adm@corp.local](mailto:svc_adm@corp.local)

Additional account context:

corp\admin activity also appeared in related events.

- Key Evidence Reviewed

The investigation focused on the following evidence areas:

Initial LSASS credential dumping alert
Detection rule details
Splunk event correlation
Procdump execution
LSASS dump file creation
Rundll32 usage
Curl upload activity
External storage destination
Elastic alert validation
Process tree analysis
Service account activity

- Evidence Chain

The strongest evidence came from the sequence of activity.

First, the alert identified possible LSASS credential dumping. Then, Splunk showed events related to Procdump, LSASS, Curl, and dump file paths. Procdump was seen using command-line arguments that referenced LSASS and created a dump file under the Windows Temp directory.

Next, Curl activity appeared shortly after the dump activity. The command line showed a file upload to an external storage location. This was important because it connected the local credential dumping behavior to possible external data transfer.

Elastic also showed an LSASS Memory Dump Creation alert, which helped support the findings from Splunk. The process analyzer showed process relationships involving cmd.exe and procdump64.exe, which helped explain how the activity happened.

- Important Observations

The first important observation was that Procdump was not just present on the system. It was used in a way that referenced LSASS and created a dump file. This is different from normal troubleshooting because LSASS contains sensitive authentication information.

The second important observation was that Curl was used to upload the dump file externally. This changed the severity of the case because the concern was no longer only local credential dumping. The evidence suggested possible credential exfiltration.

The third important observation was that the activity involved a service account. Service accounts can be risky because they often have elevated or persistent access. If one is abused, the impact can spread quickly.

- Analyst Reasoning

I did not treat the alert as confirmed by itself. I looked for supporting evidence that could explain what actually happened. The main things I wanted to confirm were:

Was LSASS accessed?
Was a dump file created?
What tool created it?
Was the file moved or uploaded?
What account was involved?
What system was affected?

The evidence became stronger as each question was answered. The connection between Procdump, LSASS, the dump file, and Curl upload activity made the case much more serious.

- Case Determination

Case status:

True Positive

Reason:

The investigation showed evidence of LSASS dump creation and external upload activity. Multiple data sources supported the same behavior pattern. The activity was consistent with credential dumping and possible credential exfiltration.

- Recommended Response Actions

Recommended actions:

Isolate SRV-DC01 from the network
Preserve logs and endpoint evidence
Disable or reset the affected service account
Review all recent logons from the involved account
Rotate privileged credentials
Review Tier-0 accounts for possible exposure
Block the external upload destination
Hunt for the same Procdump, Rundll32, and Curl behavior across other systems
Review lateral movement indicators
Document the full timeline

- Risk Level

High

Reason:

Credential dumping can lead to lateral movement, privilege escalation, and domain compromise. If LSASS memory was dumped and uploaded externally, the attacker may have access to sensitive authentication material.

- Limitations

This case was based on available lab evidence. In a real environment, additional work would be needed, including memory analysis, full endpoint timeline review, identity logs, network traffic review, and coordination with system owners.

- Final Case Summary

This investigation showed how a credential dumping alert should be validated through multiple pieces of evidence. The most important finding was not just that Procdump or Curl appeared, but that they appeared together in a suspicious sequence involving LSASS and a dump file upload. The case reinforced the importance of building an evidence chain before making a final decision. Based on the available evidence, this activity should be treated as a confirmed credential dumping and exfiltration incident.
