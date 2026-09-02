WEEK 17 LAB 6 – AZURE RBAC SECURITY MONITORING, DETECTION, AND AUTOMATION

Professional Technical Analysis Lab Write-Up

LAB OVERVIEW

In this lab, I built a cloud security monitoring and detection workflow using Microsoft Azure, Microsoft Sentinel, Microsoft Defender, Azure Activity logs, KQL, Azure Role-Based Access Control (RBAC), Network Security Groups (NSGs), analytics rules, and automation rules.

The main goal of this lab was to understand how security teams can monitor important administrative activity in Azure, especially changes involving user permissions and privileged access.

Instead of only creating Azure resources, I also generated controlled activity, collected the logs, searched the activity using KQL, created a detection rule, investigated user activity, removed unnecessary permissions, and created an automation rule for future alerts.

This allowed me to practice several parts of a real cloud security workflow:

Prevention

Logging

Monitoring

Threat detection

Investigation

Access control

Least privilege

Remediation

Security automation

LAB OBJECTIVES

The main objectives of this lab were to:

Create a controlled Azure environment for security testing.

Work with Azure Role-Based Access Control.

Understand the difference between Reader and Contributor permissions.

Generate real Azure administrative activity.

Collect Azure Activity logs for security monitoring.

Verify that Azure Activity data was being received.

Use KQL to search Azure administrative activity.

Investigate activity performed by a specific test user.

Detect Azure RBAC role assignment activity.

Create a Microsoft Sentinel analytics rule.

Remove unnecessary privileged access.

Apply the principle of least privilege.

Create an automation rule for detected RBAC activity.

Build an end-to-end cloud security detection and response workflow.

ENVIRONMENT AND TECHNOLOGIES USED

The lab environment included:

Microsoft Azure

Microsoft Defender

Microsoft Sentinel

Azure Activity Logs

Azure Role-Based Access Control (RBAC)

Azure Network Security Groups

Log Analytics Workspace

Kusto Query Language (KQL)

Microsoft Sentinel Analytics Rules

Microsoft Sentinel Automation Rules

Windows 11 Enterprise virtual machine

Oracle VirtualBox

Microsoft Edge

IMPORTANT LAB RESOURCES

Resource Group:

rg-week17-lab6-security

Network Security Group:

nsg-week17-lab6

Test User:

Lab Cloud User

Detection Rule:

Lab - Azure RBAC Role Assignment Activity

Automation Rule:

Lab - RBAC Incident Triage Automation

Final Test User Permission:

Reader

PART 1 – CREATING THE LAB ENVIRONMENT

I started by working inside a dedicated Azure resource group named:

rg-week17-lab6-security

Using a separate resource group helped keep the lab resources organized and made it easier to identify activity related specifically to this lab.

The environment also included the Network Security Group:

nsg-week17-lab6

This gave me a controlled Azure resource where I could make administrative security changes and then investigate how those changes appeared in Azure logging.

PART 2 – WORKING WITH THE NETWORK SECURITY GROUP

I worked with the Network Security Group and created a controlled security rule change.

One of the rules created during the lab was:

Deny-Lab-SSH

The rule used a deny action and was created as a controlled Week 17 Lab 6 NSG change.

This part of the lab helped demonstrate that cloud security monitoring is not limited to malware or endpoint activity. Changes to cloud infrastructure can also be important security events.

For example, an attacker who gains access to an Azure account could attempt to modify firewall or NSG rules to allow access that should normally be blocked.

Because of this, administrative configuration changes should be logged and monitored.

PART 3 – CONFIGURING AZURE ACTIVITY LOG MONITORING

Azure Activity logs were used as an important data source for this lab.

Azure Activity records management-level actions performed against Azure resources.

This can include activity involving:

Resource creation

Resource modification

Resource deletion

Role assignments

Network configuration

Security configuration

Administrative actions

After configuring the environment, I verified that Azure Activity information was being received by the security monitoring environment.

This was an important step because a detection rule is only useful if the required logs are actually being collected.

PART 4 – VERIFYING DATA IN MICROSOFT SENTINEL

I used Microsoft Defender and Microsoft Sentinel to verify that AzureActivity data was available.

I observed Azure Activity data being received in the environment and then used Advanced Hunting to investigate it.

This confirmed that the logging pipeline was working.

The basic data flow was:

Azure activity occurs

Azure records the activity

The activity is sent to the security monitoring environment

Microsoft Sentinel can search the activity

KQL can be used to investigate it

Analytics rules can detect suspicious activity

PART 5 – HUNTING AZURE ACTIVITY WITH KQL

I used Kusto Query Language to search the AzureActivity table.

One of my searches focused on activity involving the resource group:

rg-week17-lab6-security

The query filtered recent Azure activity and projected useful fields such as:

TimeGenerated

Caller

CallerIpAddress

OperationNameValue

ActivityStatusValue

ResourceGroup

This allowed me to see who performed an action, when the action happened, the IP address associated with the activity, what operation occurred, whether it succeeded, and what resource group was involved.

This was important because security analysts usually need context instead of simply knowing that an event happened.

PART 6 – CREATING AND TESTING THE LAB CLOUD USER

I used a controlled test account named:

Lab Cloud User

The account was used to test Azure permissions and generate RBAC activity.

During the lab, the user temporarily received the Contributor role.

Contributor is a powerful Azure role because it allows a user to manage Azure resources.

Giving a user more permissions than they need creates unnecessary security risk.

However, temporarily assigning the role in this controlled lab allowed me to generate realistic Azure RBAC activity that could then be investigated.

PART 7 – INVESTIGATING SIGN-IN ACTIVITY

I used the SigninLogs table to investigate authentication activity associated with the Lab Cloud User.

The query filtered activity using the test user's UserPrincipalName.

I reviewed fields including:

TimeGenerated

UserPrincipalName

AppDisplayName

ResultType

ResultDescription

AuthenticationRequirement

ConditionalAccessStatus

The results showed sign-in activity involving services such as Azure Portal.

This demonstrated the importance of combining identity information with cloud resource activity.

If a suspicious administrator action occurs, an analyst should not only investigate the action itself.

The analyst should also ask:

Who signed in?

When did they sign in?

What application did they access?

Was authentication successful?

Was Conditional Access involved?

What happened after authentication?

PART 8 – INVESTIGATING THE TEST USER'S AZURE ACTIVITY

I also searched AzureActivity specifically for activity generated by the Lab Cloud User.

This allowed me to connect the identity to actions performed against Azure resources.

The investigation showed activity associated with the resource group and the test account.

This is an important investigation technique because it allows an analyst to move from:

"Something changed."

to:

"This user performed this action against this resource at this time from this source."

PART 9 – DETECTING RBAC ROLE ASSIGNMENT ACTIVITY

The next major part of the lab focused on Azure RBAC.

RBAC controls what users and identities are allowed to do in Azure.

A role assignment can increase someone's access to cloud resources.

Because of this, unexpected role assignments can be an important security signal.

I used AzureActivity data to search for operations related to role assignments.

The detection logic looked for role assignment operations and successful write activity.

This helped identify events where Azure permissions were successfully changed.

PART 10 – CREATING THE MICROSOFT SENTINEL ANALYTICS RULE

After confirming that the activity could be found with KQL, I turned the search into a Microsoft Sentinel analytics rule.

The rule was named:

Lab - Azure RBAC Role Assignment Activity

The rule was configured as an active detection rule.

Its purpose was to detect successful Azure RBAC role assignment activity that could represent an unauthorized privilege change.

The rule was configured with Medium severity.

I associated the detection with the MITRE ATT&CK tactic:

Privilege Escalation

This makes sense because attackers may attempt to increase permissions after gaining access to a cloud environment.

Instead of manually running the KQL query every time, the analytics rule allows Microsoft Sentinel to repeatedly evaluate the activity.

The rule was configured to run on a schedule and trigger when matching results were found.

PART 11 – WHY RBAC CHANGES MATTER

Role assignments are especially important from a security perspective.

If an attacker compromises a normal user account, that account may have limited permissions.

The attacker may then attempt to give the account additional privileges.

For example, an attacker could attempt to obtain:

Contributor

Owner

User Access Administrator

or another privileged role.

A security team should therefore monitor unexpected role assignment creation and modification.

The detection created during this lab provides visibility into that type of activity.

PART 12 – REMOVING CONTRIBUTOR ACCESS

After generating and investigating the controlled RBAC activity, I removed the Contributor role from Lab Cloud User.

This was an important remediation step.

The user did not need Contributor access after the test was completed.

Leaving the account with unnecessary write access would violate the principle of least privilege.

After removing Contributor, I verified that Lab Cloud User had:

Reader

Reader provides significantly less access than Contributor and was more appropriate for the test account after completing the privileged portion of the lab.

PART 13 – APPLYING LEAST PRIVILEGE

The principle of least privilege means that users should receive only the permissions they need to perform their required tasks.

They should not keep powerful permissions simply because those permissions might be useful later.

This lab demonstrated that idea directly.

The test user temporarily needed Contributor access to generate controlled activity.

After the test was finished, Contributor was removed.

The account was left with Reader access.

The process was:

Grant temporary access

Generate controlled activity

Monitor the activity

Investigate the activity

Remove unnecessary access

Verify the lower permission level

This is much safer than permanently leaving a user with unnecessary administrative permissions.

PART 14 – CREATING THE AUTOMATION RULE

The final major technical part of the lab involved Microsoft Sentinel automation.

I created an automation rule named:

Lab - RBAC Incident Triage Automation

The rule was connected specifically to:

Lab - Azure RBAC Role Assignment Activity

The condition was configured using:

Analytics Rule Name

Equals

Lab - Azure RBAC Role Assignment Activity

This prevented the automation from being applied to unrelated security alerts.

The automation was configured to update matching alerts so that RBAC-related detections could move into the investigation workflow automatically.

This introduced a basic SOAR concept into the lab.

SOAR stands for Security Orchestration, Automation, and Response.

Instead of requiring an analyst to manually perform every repetitive action, automation can perform certain steps automatically when specific security conditions are met.

PART 15 – END-TO-END SECURITY WORKFLOW

By the end of the lab, I had created the following security workflow:

Azure administrative activity occurs.

Azure Activity logs record the event.

The security monitoring environment receives the data.

KQL is used to search and investigate the activity.

The Sentinel analytics rule searches for successful RBAC role assignment activity.

Matching activity can generate a security alert.

The automation rule recognizes alerts created by the RBAC analytics rule.

The alert can automatically move into the investigation workflow.

An analyst can investigate the caller, IP address, target resource, operation, authentication activity, and other evidence.

Unnecessary permissions can then be removed.

The user's access can be returned to a least-privilege level.

SECURITY CONCEPTS PRACTICED

This lab gave me hands-on experience with:

Cloud security monitoring

Azure security

Identity security

Role-Based Access Control

Least privilege

Privilege escalation detection

Log analysis

KQL

SIEM

Microsoft Sentinel

Microsoft Defender

Detection engineering

Security analytics

Cloud activity monitoring

Sign-in investigation

Network security groups

Alert automation

SOAR concepts

Incident investigation

Remediation

Security validation

KEY SECURITY FINDINGS

The main security finding from the controlled investigation was that the Lab Cloud User had received Contributor access.

Contributor provides much greater control over Azure resources than Reader.

The role assignment created security-relevant Azure activity that could be identified through AzureActivity logs.

The activity was successfully visible in the monitoring environment.

The Lab Cloud User's authentication activity was also available for investigation through SigninLogs.

The Sentinel analytics rule provided a way to continuously detect future RBAC role assignment activity.

The automation rule added an automated response step for alerts generated by that detection.

Finally, Contributor access was removed and the test user was verified with Reader access.

REMEDIATION PERFORMED

The main remediation action was:

Removed Contributor access from Lab Cloud User.

The account was then verified with:

Reader

This reduced the account's ability to modify Azure resources and returned the environment to a safer permission state.

WHAT I LEARNED

One of the biggest things I learned from this lab is that cloud security requires more than preventing attacks.

Security teams also need visibility.

An organization needs to know when permissions change, who made the change, where the activity came from, what resource was affected, and whether the activity was authorized.

I also learned how different security controls can work together.

RBAC controls access.

Azure Activity provides evidence of administrative actions.

Sign-in logs provide identity and authentication evidence.

KQL helps analysts search that evidence.

Microsoft Sentinel analytics rules turn searches into repeatable detections.

Automation rules help reduce repetitive analyst work.

Least privilege reduces the damage that an account could cause if it becomes compromised.

Together, these controls create a stronger security process than relying on any single tool.

FINAL RESULT

The lab was completed successfully.

I built and tested an Azure cloud security workflow that covered prevention, logging, detection, investigation, automation, and remediation.

The final environment demonstrated how Azure RBAC activity can be monitored using Microsoft Sentinel and how unnecessary privileged access can be removed after investigation.

The Lab Cloud User was returned to Reader access, the RBAC analytics rule was enabled, and an automation rule was created to support future RBAC alert handling.

FINAL STATUS

Resource Group: rg-week17-lab6-security

Network Security Group: nsg-week17-lab6

Test Account: Lab Cloud User

Temporary Elevated Role: Contributor

Final Role: Reader

RBAC Detection Rule: Lab - Azure RBAC Role Assignment Activity

Detection Severity: Medium

MITRE ATT&CK Tactic: Privilege Escalation

Automation Rule: Lab - RBAC Incident Triage Automation

Analytics Rule Status: Enabled

Automation Rule Status: Enabled

Lab Status: COMPLETED