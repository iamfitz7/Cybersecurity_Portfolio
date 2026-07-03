INCIDENT CASE STUDY

RDP SUCCESSFUL REMOTE LOGON INVESTIGATION

CASE OVERVIEW

This case involved the investigation of a successful Remote Desktop Protocol logon detected through Elastic Security.

The alert identified a successful RemoteInteractive authentication event involving the Administrator account on workstation-01. The connection was associated with the source IP address 142.114.44.227.

The alert required investigation because successful RDP access can have several possible explanations. It may represent normal remote administration, technical support activity, approved remote work, or other authorized access. However, successful RDP authentication can also represent unauthorized access if credentials have been stolen, guessed, reused, or otherwise compromised.

The purpose of the investigation was therefore not simply to confirm that an RDP login occurred. The main investigative question was whether the available evidence supported legitimate remote access, suspicious activity requiring additional investigation, or confirmed unauthorized access.

The investigation focused on detection validation, authentication analysis, source IP context, user and host attribution, timeline review, process activity, network activity, and the search for supporting or contradictory evidence.

The final assessment was that the alert represented a valid detection of successful RDP activity, but the reviewed evidence did not establish malicious post-authentication behavior or confirmed unauthorized access.

Final Classification: Benign Positive

Confidence Level: Moderate

Primary Security Platform: Elastic Security

Affected Host: workstation-01

Account Involved: Administrator

Source IP: 142.114.44.227

Primary Event: Successful RemoteInteractive Logon

Windows Event ID: 4624

Logon Type: 10

Investigation Area: Authentication and Remote Access

Week Assignment: Week 19 – Triage and Investigations

Lab Assignment: Lab 2

EXECUTIVE SUMMARY

Elastic Security generated an alert for a successful Remote Desktop Protocol authentication event involving the Administrator account on workstation-01.

Because the activity involved a privileged account and remote access from an external source IP address, the alert required additional investigation. However, the existence of a successful RDP authentication event alone was not treated as evidence of compromise.

The investigation began by reviewing the detection rule to understand the exact behavior that caused the alert. The rule logic showed that the detection was based on Windows Security Event ID 4624 combined with Logon Type 10. This confirmed that the alert was designed to identify successful RemoteInteractive logons commonly associated with RDP sessions.

The investigation then examined the affected user, destination host, source IP address, authentication details, event timeline, and available process and network activity.

The reviewed evidence confirmed that the RDP authentication event occurred. However, the investigation did not identify sufficient supporting evidence of brute-force authentication, password spraying, suspicious credential usage, malicious process execution, persistence, credential dumping, or other clear post-authentication activity that would justify classifying the event as a confirmed security incident.

The final assessment was Benign Positive with Moderate confidence. The detection accurately identified the activity it was designed to detect, but the available evidence did not support a conclusion of malicious remote access.

Additional organizational context would be required for a higher-confidence conclusion. In a production environment, this would include confirming whether the source IP was associated with approved remote access infrastructure, reviewing historical account behavior, checking VPN and firewall telemetry, validating authorized administrative activity, and confirming the login with the system or account owner when necessary.

1. INVESTIGATION BACKGROUND

Remote Desktop Protocol provides interactive remote access to Windows systems.

In legitimate environments, RDP may be used by system administrators, IT support teams, engineers, vendors, and approved remote users. Because the protocol provides interactive access to a system, successful authentication can give the remote user significant control over the endpoint depending on the permissions associated with the authenticated account.

This makes successful RDP authentication an important security event.

At the same time, successful RDP authentication should not automatically be classified as malicious. A security alert can correctly identify remote authentication activity without proving that the activity was unauthorized.

This distinction became the central issue in the investigation.

The alert established that a successful RemoteInteractive logon occurred. The investigation needed to determine what the surrounding evidence could prove about the meaning of that authentication event.

The investigation therefore followed a structured approach:

Understand the detection.

Validate the underlying event.

Identify the user.

Identify the affected host.

Review the source of the connection.

Examine authentication context.

Review the timeline.

Search for related activity.

Look for follow-on process execution.

Look for relevant network activity.

Consider alternative explanations.

Separate confirmed facts from assumptions.

Determine whether the evidence justified escalation.

2. INITIAL ALERT INFORMATION

The investigation began with an Elastic Security alert identifying a successful RDP logon.

The primary information associated with the alert included:

Alert Type: RDP Successful Interactive Remote Logon Detected

Affected Host: workstation-01

User: Administrator

Source IP Address: 142.114.44.227

Windows Event ID: 4624

Logon Type: 10

Severity: Medium

Risk Score: 47

The Administrator account increased the importance of the alert because privileged accounts generally have greater access to systems and resources than standard user accounts.

However, the account name alone was not treated as proof of compromise.

The initial investigation hypothesis was:

A successful RemoteInteractive authentication occurred on workstation-01 using the Administrator account from source IP address 142.114.44.227. The event requires investigation to determine whether the remote access was expected or potentially unauthorized.

This hypothesis intentionally avoided assuming that the event was malicious.

3. PRIMARY INVESTIGATION QUESTIONS

The investigation was organized around specific questions rather than random searches.

The main questions were:

What exact behavior caused the detection rule to trigger?

Did the underlying telemetry confirm a successful authentication?

Was the event consistent with RDP or another RemoteInteractive logon?

Which user account authenticated?

Which system received the connection?

What source IP address was associated with the activity?

Was the source IP seen in other relevant events?

Were failed authentication attempts observed before the successful login?

Was there evidence of brute-force activity?

Was there evidence of password spraying?

Was there evidence of suspicious credential use?

Did the authenticated user perform suspicious actions after the login?

Were suspicious processes executed?

Was PowerShell or a command shell involved?

Were there suspicious network connections?

Was there evidence of discovery activity?

Was there evidence of persistence?

Was there evidence of credential access?

Did the available evidence justify escalation?

What remained unknown at the end of the investigation?

These questions helped keep the investigation focused on evidence rather than assumptions.

4. DETECTION RULE VALIDATION

The first major investigative task was understanding why the alert existed.

The rule logic was reviewed before making any judgment about the activity.

The detection was associated with Windows Security Event ID 4624 and Logon Type 10.

Event ID 4624 represents a successful account logon.

Logon Type 10 represents a RemoteInteractive logon. This type of authentication is commonly associated with Remote Desktop Protocol access.

This distinction was important.

The rule was not claiming that malware had executed.

It was not proving that credentials had been stolen.

It was not proving that an attacker had compromised the endpoint.

It was identifying successful remote authentication behavior.

The detection therefore appeared technically valid because the underlying authentication event matched the behavior the rule was designed to identify.

At this stage, two separate questions had to remain separate:

Did the rule detect the intended behavior?

Yes.

Was the detected behavior malicious?

Not yet established.

This separation prevented premature classification.

5. AUTHENTICATION EVENT ANALYSIS

The authentication event was reviewed to confirm the nature of the activity.

The important fields included:

Event ID: 4624

Logon Type: 10

User: Administrator

Host: workstation-01

Source IP: 142.114.44.227

Logon Process: User32

The combination of Event ID 4624 and Logon Type 10 supported the conclusion that the event represented a successful RemoteInteractive authentication.

This confirmed the central fact of the case:

A successful remote interactive login involving the Administrator account occurred on workstation-01.

This fact was treated as confirmed.

However, the authorization status of the activity was not established by the authentication event alone.

The event answered:

Did the login occur?

Yes.

It did not independently answer:

Was the login authorized?

That required additional context.

6. USER AND HOST ATTRIBUTION

The account involved in the event was Administrator.

The affected endpoint was workstation-01.

This provided a clear user-host relationship for the investigation.

The use of an Administrator account increased the potential impact of unauthorized access because privileged access may allow broader system changes, security control modification, software installation, credential access attempts, or other administrative actions.

However, privileged accounts are also used for legitimate administrative purposes.

Therefore, the investigation did not treat the Administrator account name as an indicator of compromise by itself.

The correct interpretation was:

The account context increased the importance of the alert and justified additional investigation, but additional evidence was required before malicious intent could be established.

7. SOURCE IP INVESTIGATION

The source IP address associated with the successful authentication was:

142.114.44.227

Elastic enrichment associated the IP address with Canada, specifically Ontario, and a network associated with Bell Canada.

This information provided geographical and network context.

However, IP enrichment was treated as supporting context rather than final evidence.

An IP address associated with a residential or commercial internet provider can be used by a legitimate remote employee, administrator, contractor, compromised device, VPN exit point, or unauthorized individual.

Therefore, geographic information alone could not establish whether the login was legitimate.

The source IP investigation was useful for defining the origin of the connection and identifying an observable that could be used for additional pivots.

In a production investigation, additional checks would include:

Comparing the IP against approved VPN ranges.

Reviewing historical authentication activity from the IP.

Determining whether the account had previously authenticated from the same source.

Checking whether other accounts had authenticated from the source IP.

Reviewing firewall telemetry.

Reviewing VPN logs.

Reviewing identity provider logs.

Checking whether the IP was associated with approved administrative access.

Confirming whether the geographic location matched expected user activity.

These checks would provide stronger organizational context than public IP enrichment alone.

8. TIMELINE ANALYSIS

The investigation reviewed the authentication event within its surrounding time context.

The successful authentication event was observed at approximately:

Jun 26, 2026 @ 13:59:04.746

The timeline associated the event with:

Host: workstation-01

User: Administrator

Source IP: 142.114.44.227

Process Context: svchost.exe

The timeline helped confirm when the authentication occurred and provided a reference point for searching before and after the event.

The investigation considered three important periods:

Before the authentication event.

During the authentication event.

After the authentication event.

The period before the event was important for identifying possible failed login attempts, authentication testing, brute-force activity, password spraying, or suspicious credential use.

The event itself was important for validating the successful RemoteInteractive login.

The period after authentication was important for determining whether the remote session was followed by suspicious execution, discovery, persistence, credential access, file activity, or network communication.

The available evidence confirmed the authentication event but did not establish a broader malicious sequence.

9. AUTHENTICATION CORRELATION

The investigation searched for related authentication activity involving the host, account, and source IP.

The goal was to determine whether the successful login was preceded by suspicious authentication behavior.

Relevant activity considered included:

Successful authentication events.

Failed authentication events.

Logoff events.

Explicit credential usage.

Special privilege assignment.

Repeated authentication attempts.

Authentication activity involving the same source IP.

Authentication activity involving the same Administrator account.

A common attack pattern may involve repeated failed logins followed by a successful authentication. Such a sequence can support a brute-force or password-guessing hypothesis.

Another possible pattern is one source IP attempting authentication against multiple accounts or systems. Depending on the environment and timing, that may support a password-spraying or broader credential attack hypothesis.

The available alert data reviewed during this investigation did not establish a clear failed-login pattern immediately preceding the successful RDP authentication.

This was useful negative evidence, but it was interpreted carefully.

The investigation did not conclude:

No failed authentication ever occurred.

Instead, the defensible conclusion was:

The reviewed telemetry did not provide sufficient evidence of a failed-login pattern that established brute-force or password-spraying activity before the successful authentication.

This wording accurately reflected the limits of the reviewed evidence.

10. PROCESS ACTIVITY REVIEW

The investigation also considered whether suspicious process execution occurred on workstation-01 after authentication.

This was important because successful unauthorized access often becomes more visible through what happens after authentication.

Examples of potentially concerning follow-on activity could include:

PowerShell execution.

Command Prompt execution.

Encoded commands.

Account enumeration.

Network discovery.

System information collection.

Security control modification.

Credential dumping.

Remote tool installation.

Service creation.

Scheduled task creation.

Suspicious file downloads.

Execution from temporary directories.

Execution from user-writable directories.

Unexpected script interpreters.

The available process activity reviewed during the investigation did not provide sufficient evidence of a malicious post-authentication process chain.

This finding influenced the final assessment because the successful authentication event did not appear, within the available evidence, to be part of a clearly established malicious sequence.

11. NETWORK ACTIVITY REVIEW

The investigation also reviewed available network context associated with workstation-01.

The purpose of this review was to determine whether the authentication event was followed by suspicious communication that could support a compromise hypothesis.

Potentially important network indicators would include:

Connections to suspicious external infrastructure.

Unexpected outbound connections immediately after authentication.

Connections to newly observed domains.

Connections to unusual destination ports.

Connections associated with suspicious processes.

Possible command-and-control traffic.

Large outbound data transfers.

Unexpected internal scanning.

Connections to multiple internal hosts.

The available evidence did not provide sufficient supporting network activity to confirm malicious post-authentication behavior.

Again, the absence of identified suspicious network evidence was not treated as absolute proof that no network activity occurred.

The correct conclusion was limited to the telemetry reviewed during the investigation.

12. SIGNAL VERSUS NOISE ANALYSIS

The investigation contained several pieces of information that initially increased concern:

A successful RDP login occurred.

The Administrator account was involved.

The connection originated from an external source IP.

The activity involved interactive remote access.

These facts were important and justified investigation.

However, none independently proved compromise.

The most important analytical task was determining whether these initial indicators were supported by additional evidence.

The investigation did not identify a strong failed-login sequence.

It did not establish brute-force activity.

It did not establish password spraying.

It did not identify sufficient suspicious process execution after the login.

It did not identify sufficient network evidence supporting compromise.

It did not establish persistence.

It did not establish credential access.

The signal was therefore:

A successful RemoteInteractive authentication involving a privileged account occurred and deserved investigation.

The unsupported assumption would have been:

An attacker successfully compromised the Administrator account.

The investigation avoided crossing that line without evidence.

13. ALTERNATIVE HYPOTHESES CONSIDERED

A structured investigation should consider multiple possible explanations.

Hypothesis 1: Legitimate Remote Administration

The Administrator account may have been used for authorized remote system management.

Supporting considerations:

RDP is commonly used for legitimate administration.

No confirmed malicious follow-on sequence was identified.

No clear brute-force pattern was established in the reviewed evidence.

Limitation:

Organizational authorization data was not available for direct confirmation.

Hypothesis 2: Compromised Administrator Credentials

An unauthorized person may have obtained valid credentials and successfully authenticated.

Supporting considerations:

A privileged account was used.

The login originated from an external IP address.

Limitations:

No clear failed-login sequence was established.

No malicious post-authentication behavior was confirmed.

No additional evidence directly established credential compromise.

Hypothesis 3: Brute-Force Attack Followed by Successful Authentication

Repeated password attempts may have eventually resulted in successful access.

Limitation:

The reviewed evidence did not establish the required failed-login pattern before the successful event.

Hypothesis 4: Password Spraying

The source may have attempted limited password combinations across multiple accounts.

Limitation:

The reviewed evidence did not establish a multi-account authentication pattern supporting this explanation.

Hypothesis 5: Approved Remote Access Through External Infrastructure

The source IP may have been associated with an approved remote user, administrator, contractor, or other authorized access method.

Limitation:

The investigation did not have sufficient organizational data to directly validate authorization.

After comparing the available evidence, legitimate or otherwise authorized remote access was the explanation that best fit the reviewed telemetry. However, the absence of complete organizational context prevented a High-confidence classification.

14. FACTS, ASSESSMENTS, AND UNKNOWNS

Confirmed Facts

A successful authentication event occurred.

The event was Windows Event ID 4624.

The event used Logon Type 10.

The activity was consistent with a RemoteInteractive logon.

The account involved was Administrator.

The destination host was workstation-01.

The source IP was 142.114.44.227.

The detection rule correctly matched the observed authentication behavior.

The available investigation did not identify sufficient supporting evidence of malicious post-authentication behavior.

Analytical Assessment

The event most likely represented legitimate or otherwise authorized remote access based on the lack of corroborating malicious behavior in the reviewed evidence.

This assessment was made with Moderate confidence rather than High confidence because direct authorization confirmation was unavailable.

Remaining Unknowns

Whether the Administrator account owner expected the session.

Whether the source IP belonged to an approved administrator.

Whether the source IP was part of approved VPN infrastructure.

Whether the account had historical authentication activity from the same source.

Whether additional firewall, VPN, identity, or endpoint telemetry existed outside the reviewed dataset.

Whether there was an approved maintenance or administrative activity window.

These unknowns were documented rather than ignored.

15. FINAL CLASSIFICATION DECISION

Final Classification: Benign Positive

Confidence: Moderate

The alert was classified as a Benign Positive because the detection correctly identified the behavior it was designed to detect.

A successful RemoteInteractive authentication event occurred.

Therefore, the alert itself was not considered a false positive.

However, the investigation did not identify sufficient evidence to classify the authentication event as a confirmed security incident.

The decision was based on the following reasoning:

The rule logic correctly identified Event ID 4624 with Logon Type 10.

The authentication event was successfully validated.

The affected user, host, and source IP were identified.

The investigation did not establish a clear brute-force sequence.

The investigation did not establish password spraying.

The reviewed process activity did not provide sufficient evidence of malicious post-authentication execution.

The reviewed network activity did not provide sufficient evidence of compromise.

No broader malicious sequence was established from the available telemetry.

Based on these findings, escalation as a confirmed security incident was not supported by the available evidence.

16. WHY THIS WAS NOT CLASSIFIED AS A FALSE POSITIVE

This distinction is important.

The alert was not classified as a false positive because the detection accurately identified the behavior it was designed to detect.

The successful RemoteInteractive logon occurred.

A false positive would suggest that the detection incorrectly identified the event or that the underlying behavior did not actually match the detection condition.

That was not the case here.

The better classification was Benign Positive because:

The detection was technically correct.

The observed activity was real.

The available evidence did not establish malicious intent or unauthorized access.

This distinction helps preserve the difference between detection accuracy and incident severity.

17. ESCALATION CONSIDERATIONS

Although the available evidence did not support immediate escalation as a confirmed compromise, several findings would have changed the decision.

The alert would require stronger escalation if additional evidence showed:

Multiple failed logins immediately before the successful authentication.

One source IP attempting authentication against multiple accounts.

Successful authentication across several internal systems.

PowerShell execution shortly after the login.

Command Prompt activity performing system discovery.

Credential dumping activity.

Security control modification.

New service creation.

Scheduled task persistence.

Suspicious file downloads.

Malware execution.

Connections to suspicious external infrastructure.

Internal reconnaissance.

Lateral movement.

Unusual file access.

Data staging.

Data exfiltration.

Any of these findings could change the assessment because they would provide supporting evidence that the successful login was part of a larger attack sequence.

18. RESPONSE ACTIONS IN A REAL ENVIRONMENT

In a production environment, I would recommend the following additional validation before final closure:

Confirm whether the Administrator account was authorized to use RDP.

Compare the source IP with approved VPN and remote access ranges.

Review VPN authentication records.

Review firewall connection logs.

Review historical authentication behavior for the Administrator account.

Search for other accounts authenticating from the same source IP.

Search for other systems receiving connections from the same source IP.

Review endpoint telemetry following the authentication.

Check for suspicious process execution.

Review file creation and modification events.

Review service creation activity.

Review scheduled task creation.

Review PowerShell logging.

Review network connections after authentication.

Confirm the activity with the account or system owner when appropriate.

If the user denied performing the activity, the case would require immediate escalation and additional containment consideration.

19. RISKS AND LIMITATIONS

The primary limitation of this investigation was the lack of complete business and identity context.

Security telemetry can show that an authentication event occurred, but authorization is often determined through additional sources such as:

VPN logs.

Identity provider logs.

Administrative access policies.

Change management records.

User confirmation.

Asset ownership records.

Historical baselines.

Without those sources, the investigation could determine what happened technically but could not independently prove authorization with complete certainty.

Another limitation was that the absence of identified malicious activity within the reviewed telemetry does not prove that no malicious activity occurred.

Telemetry coverage may be incomplete.

Logging may be limited.

Some attacker behavior may not generate alerts.

Some events may exist in data sources that were not available during the investigation.

These limitations are why the confidence level remained Moderate.

20. COMMON BEGINNER MISTAKE

A common mistake is seeing the words “successful RDP login” and “Administrator” and immediately concluding that the endpoint was compromised.

This approach confuses suspicious context with confirmed malicious activity.

The opposite mistake is also possible.

An analyst may assume that RDP is normal administrative activity and close the alert without investigating it.

Both approaches are weak.

The better approach is to validate:

What happened.

Who authenticated.

Where the connection came from.

What happened before the login.

What happened after the login.

Whether the source was expected.

Whether the user was authorized.

Whether other evidence supports compromise.

This approach allows the final decision to be based on evidence rather than fear or familiarity.

21. PRACTICAL DETECTION AND INVESTIGATION IMPROVEMENTS

One improvement would be to enrich successful RDP alerts with additional organizational context automatically.

Useful enrichment could include:

Whether the source IP belongs to an approved VPN range.

Whether the account is authorized for RDP.

Whether the account is privileged.

Whether the destination system normally accepts remote access.

Whether the source country is expected.

Whether the account has used the source IP before.

The number of failed logins before success.

The number of accounts targeted by the source IP.

Recent endpoint process activity after authentication.

Recent alerts involving the user or host.

This would help future analysts move more quickly from detection validation to risk assessment.

A useful triage view could immediately answer:

Was the login successful?

Was the source expected?

Is the account privileged?

Were there failures before success?

What happened immediately afterward?

That would reduce investigation time while improving consistency.

22. LESSONS LEARNED

The most important lesson from this investigation was that a valid detection and a confirmed security incident are not the same thing.

The detection rule correctly identified successful RDP activity.

That fact was important, but it was only the beginning of the investigation.

The meaning of the activity depended on context.

The investigation reinforced the importance of:

Understanding rule logic before classification.

Validating underlying telemetry.

Separating facts from assumptions.

Using timelines to understand sequence.

Correlating authentication, process, and network evidence.

Using negative evidence carefully.

Documenting unknowns.

Avoiding overconfidence.

Matching the classification to what the evidence can actually support.

The investigation also reinforced that stopping is part of good investigative discipline. Once the available evidence has answered the main questions and no specific unresolved lead remains, continued random searching can create investigation drift without improving the decision.

23. INVESTIGATION OUTCOME

The investigation confirmed that Elastic Security accurately detected a successful RemoteInteractive authentication involving the Administrator account on workstation-01 from source IP address 142.114.44.227.

The event matched Windows Security Event ID 4624 with Logon Type 10 and was therefore consistent with successful RDP access.

The investigation reviewed the rule logic, authentication context, user, host, source IP, timeline, and available process and network activity. The reviewed evidence did not establish a clear brute-force sequence, password-spraying pattern, malicious process chain, suspicious network behavior, persistence, credential access, or other post-authentication activity sufficient to confirm compromise.

The final classification was Benign Positive with Moderate confidence.

The alert was considered a valid detection of real remote authentication behavior, but the available evidence did not support escalation as confirmed unauthorized access. Additional organizational context, particularly VPN records, approved administrative access information, historical authentication baselines, and user validation, would be necessary to increase confidence further.

24. PROFESSIONAL CASE SUMMARY

This case involved the investigation of a successful RDP authentication event involving a privileged account and an external source IP address. The investigation validated the detection logic, confirmed Windows Event ID 4624 with Logon Type 10, attributed the activity to the Administrator account on workstation-01, and reviewed available authentication, process, network, and timeline evidence.

The investigation demonstrated the importance of separating a valid detection from a confirmed incident. Although the authentication event was real and required investigation, the reviewed evidence did not establish sufficient malicious follow-on activity to support escalation as unauthorized access.

The case was classified as a Benign Positive with Moderate confidence, while clearly documenting the additional organizational context that would be required for a higher-confidence decision.
