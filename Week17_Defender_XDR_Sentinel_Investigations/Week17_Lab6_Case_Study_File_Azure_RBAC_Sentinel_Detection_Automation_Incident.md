WEEK 17 LAB 6 – INCIDENT CASE STUDY

Azure RBAC Privilege Change Detection and Investigation

CASE TITLE

Investigation of Azure RBAC Contributor Role Assignment

CASE TYPE

Cloud Security / Identity and Access Management / Privilege Escalation

ENVIRONMENT

Microsoft Azure

Microsoft Sentinel

Microsoft Defender

Azure Activity Logs

Azure RBAC

Microsoft Entra identity logs

KQL

Azure Network Security Groups

SEVERITY

Medium

CASE SUMMARY

During a controlled cloud security exercise, Azure monitoring identified activity involving changes to Role-Based Access Control permissions.

The activity involved a test identity named:

Lab Cloud User

The account received Contributor access during the lab.

Because Contributor allows a user to manage Azure resources, this type of permission change should be monitored carefully.

The investigation focused on determining who performed the activity, what permissions were changed, what Azure resources were involved, whether the activity was successful, and whether the elevated permissions were still required.

The activity was investigated using Azure Activity logs, Microsoft Sentinel, Microsoft Defender, KQL, sign-in logs, and Azure RBAC information.

After the controlled test was completed, Contributor access was removed from Lab Cloud User and the account was returned to Reader access.

INITIAL SECURITY CONCERN

The main concern was a successful Azure RBAC role assignment.

Changes to cloud permissions can be normal administrative activity.

However, they can also be signs of:

Privilege escalation

Account compromise

Unauthorized administrative activity

Excessive permissions

Persistence

Misconfigured access controls

Because of the possible security impact, the role assignment required investigation.

AFFECTED IDENTITY

Account:

Lab Cloud User

Temporary Role:

Contributor

Final Role:

Reader

AFFECTED ENVIRONMENT

Resource Group:

rg-week17-lab6-security

Network Security Group:

nsg-week17-lab6

DETECTION SOURCE

The primary detection source was:

AzureActivity

Additional identity investigation was performed using:

SigninLogs

DETECTION RULE

The Microsoft Sentinel analytics rule used for this activity was:

Lab - Azure RBAC Role Assignment Activity

Severity:

Medium

MITRE ATT&CK Tactic:

Privilege Escalation

DETECTION LOGIC

The detection focused on Azure Activity operations involving RBAC role assignments.

The query looked for successful role assignment write activity.

This type of detection is useful because an attacker who gains access to an Azure identity may attempt to increase that identity's permissions.

Successful permission changes should therefore receive security attention, especially when the change involves powerful roles.

INVESTIGATION PROCESS

The investigation began by reviewing AzureActivity data.

I searched recent Azure administrative activity and examined important fields including:

TimeGenerated

Caller

CallerIpAddress

OperationNameValue

ActivityStatusValue

ResourceGroup

These fields helped answer several important questions.

When did the activity happen?

Who performed it?

Where did the request come from?

What Azure operation occurred?

Did the operation succeed?

Which resource group was involved?

IDENTITY INVESTIGATION

I then investigated sign-in activity associated with Lab Cloud User.

SigninLogs provided information about authentication activity involving the account.

Important fields included:

TimeGenerated

UserPrincipalName

AppDisplayName

ResultType

ResultDescription

AuthenticationRequirement

ConditionalAccessStatus

The results included activity involving the Azure Portal.

This allowed authentication information to be compared with Azure administrative activity.

RESOURCE ACTIVITY INVESTIGATION

I filtered AzureActivity for:

rg-week17-lab6-security

This helped separate the lab activity from unrelated Azure events.

I also filtered Azure activity for the Lab Cloud User.

This made it possible to connect the identity with activity occurring against Azure resources.

RBAC INVESTIGATION

The Azure Role Assignments page was reviewed to confirm the permissions assigned to Lab Cloud User.

During the controlled test, the account had:

Contributor

Contributor provides the ability to manage Azure resources.

Because this level of access was only required temporarily for testing, keeping the role after the test would create unnecessary security risk.

ANALYSIS

The RBAC change was confirmed as expected activity generated during an authorized security lab.

There was no evidence from the lab indicating that the role assignment was performed by an outside attacker.

However, the behavior itself represents an important security event.

In a production environment, the same type of activity could require immediate investigation.

An analyst would need to determine:

Who requested the role assignment?

Was the account authorized to make the change?

What role was assigned?

What scope received the role?

Was the target user expected to receive that level of access?

What IP address generated the activity?

Was the authentication normal?

Did the user perform suspicious actions after receiving the new permissions?

Were additional permissions changed?

CLASSIFICATION

Final Classification:

Authorized Administrative Activity / Controlled Security Test

Security Relevance:

Privilege Escalation Behavior Simulation

Severity:

Medium

WHY THE ACTIVITY STILL MATTERS

Even though this activity was authorized, the behavior is still valuable for detection engineering.

A real attacker could perform a similar role assignment.

For example, after compromising an Azure account, an attacker might attempt to assign Contributor, Owner, or another powerful role to an account they control.

The activity might look similar to legitimate administrative activity.

This is why the security team must combine detection with investigation rather than automatically assuming every role assignment is malicious.

MITRE ATT&CK MAPPING

Tactic:

Privilege Escalation

Reason:

Changing role assignments can provide an identity with additional privileges.

An attacker may attempt to increase permissions after gaining initial access to an environment.

RESPONSE ACTION

After completing the investigation and controlled testing, I removed the Contributor role from Lab Cloud User.

This prevented the account from keeping unnecessary write access to Azure resources.

REMEDIATION

The account was changed from:

Contributor

to:

Reader

The Reader role allowed the account to maintain limited visibility without keeping the ability to modify resources.

This followed the principle of least privilege.

LEAST PRIVILEGE DECISION

The Contributor role was necessary only for the controlled portion of the lab.

Once the test was completed, there was no valid reason for the account to keep those permissions.

Leaving Contributor enabled would increase the possible impact if the account were compromised.

Removing the role reduced that risk.

DETECTION IMPROVEMENT

To improve future monitoring, I created the Microsoft Sentinel analytics rule:

Lab - Azure RBAC Role Assignment Activity

The rule monitors successful RBAC role assignment activity.

Instead of requiring an analyst to manually search AzureActivity every time, the detection logic can automatically evaluate new activity.

AUTOMATION RESPONSE

I also created:

Lab - RBAC Incident Triage Automation

The automation rule was connected to alerts generated by:

Lab - Azure RBAC Role Assignment Activity

The condition used:

Analytics Rule Name

Equals

Lab - Azure RBAC Role Assignment Activity

The automation was configured to update matching alerts and help move them into the investigation process.

This reduced the amount of manual work required when the detection fires.

INCIDENT TIMELINE

Stage 1:

A controlled Azure environment was prepared.

Stage 2:

Lab Cloud User was used as the test identity.

Stage 3:

Contributor access was assigned temporarily.

Stage 4:

The RBAC activity generated Azure Activity logs.

Stage 5:

AzureActivity data was reviewed using KQL.

Stage 6:

SigninLogs were reviewed to investigate authentication activity associated with the test account.

Stage 7:

Activity involving the lab resource group and test user was investigated.

Stage 8:

A Sentinel analytics rule was created to detect successful RBAC role assignment activity.

Stage 9:

The detection rule was enabled.

Stage 10:

Contributor access was removed from Lab Cloud User.

Stage 11:

The account's final permission was verified as Reader.

Stage 12:

A Sentinel automation rule was created for alerts generated by the RBAC analytics rule.

Stage 13:

The automation rule was enabled.

Stage 14:

The environment was verified in its final least-privilege state.

FINAL FINDINGS

The investigated role assignment was part of an authorized security test.

The Azure logging environment successfully provided evidence of administrative activity.

KQL allowed the activity to be searched and filtered.

Identity logs provided additional information about the test account.

The Contributor role represented elevated access that was not required permanently.

Contributor was successfully removed.

Lab Cloud User was verified with Reader access.

The Microsoft Sentinel analytics rule was enabled to detect future RBAC role assignment activity.

The Microsoft Sentinel automation rule was enabled to support automated alert handling.

FINAL DISPOSITION

True Positive Activity:

Yes, the RBAC activity actually occurred.

Malicious:

No.

Authorized:

Yes.

Reason:

Controlled Week 17 Lab 6 security testing.

Remediation Required:

Yes.

Remediation Performed:

Contributor access removed.

Final Permission:

Reader.

Monitoring Improvement:

RBAC analytics rule created and enabled.

Automation Improvement:

RBAC alert automation rule created and enabled.

LESSONS LEARNED

This case showed me why analysts should not automatically treat every security alert as malicious.

The detected behavior actually happened, which means the activity itself was real.

However, investigation showed that it was authorized lab activity.

That distinction is important.

A detection tells the analyst that something worth reviewing happened.

The investigation determines what the activity actually means.

I also learned that remediation should still be considered even when activity is legitimate.

The Contributor permission was authorized for testing, but it was no longer necessary after the test.

Removing it reduced unnecessary risk.

This case also showed how identity, cloud activity, SIEM detection, automation, and access control can work together during an investigation.

SECURITY RECOMMENDATIONS

Continue monitoring Azure RBAC role assignments.

Review Contributor and Owner assignments regularly.

Remove permissions that users no longer need.

Use least privilege when assigning Azure roles.

Monitor successful and failed administrative operations.

Correlate Azure Activity with sign-in information.

Investigate unusual source IP addresses.

Pay special attention to privileged role changes.

Use Sentinel analytics rules instead of relying only on manual searches.

Use automation carefully to reduce repetitive investigation work.

Do not automatically close privilege-related alerts without enough evidence.

Document why privileged access was granted and when it should be removed.

CASE CONCLUSION

This investigation successfully demonstrated how a cloud security analyst can detect, investigate, and respond to an Azure RBAC permission change.

The activity was intentionally generated as part of a controlled lab, but it represented behavior that could be security-sensitive in a real organization.

Azure Activity logs provided evidence of the administrative operations. Sign-in logs provided identity context. KQL allowed the activity to be investigated. Microsoft Sentinel converted the detection logic into an active analytics rule. An automation rule was then used to support alert handling.

Finally, unnecessary Contributor access was removed and Lab Cloud User was returned to Reader access.

The case demonstrated a complete security process:

Activity

Logging

Detection

Investigation

Validation

Automation

Remediation

Least privilege

The incident was closed as authorized controlled activity, with the elevated permissions successfully removed and continued monitoring put in place.

CASE STATUS

CLOSED – AUTHORIZED LAB ACTIVITY

REMEDIATION STATUS

COMPLETED

FINAL ACCESS STATUS

Lab Cloud User: Reader

DETECTION STATUS

Lab - Azure RBAC Role Assignment Activity: Enabled

AUTOMATION STATUS

Lab - RBAC Incident Triage Automation: Enabled

WEEK 17 LAB 6 STATUS

COMPLETED SUCCESSFULLY