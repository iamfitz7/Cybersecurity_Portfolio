Week #17 Lab 3 - Microsoft Sentinel & KQL Fundamentals:

Professional Technical Analysis Write-Up

Overview

This lab focused on the basic workflow of Microsoft Sentinel and Kusto Query Language (KQL). The goal was to understand how Azure activity becomes security telemetry, how that data is stored in a Log Analytics workspace, how KQL can be used to search and organize the data, and how Microsoft Sentinel can use the same telemetry for automated detection and incident investigation.

The lab followed a full security monitoring path: Azure Activity generated telemetry, the Azure Activity connector sent that telemetry to the Log Analytics workspace, the AzureActivity table stored the records, KQL was used to investigate the records, and a Sentinel analytics rule used that data to create an alert and incident.

Lab Goals

Understand how Microsoft Sentinel works with a Log Analytics workspace.

Configure Azure Activity as a data source.

Generate safe Azure administrative activity for testing.

Verify that Azure activity is collected and searchable.

Learn basic KQL operators for filtering, sorting, selecting fields, and summarizing events.

Review Microsoft Sentinel analytics rule templates.

Understand how detections map to MITRE ATT&CK.

Create and enable one scheduled analytics rule.

Review the alert and incident created by Sentinel.

Practice evidence-based investigation instead of assuming that an alert automatically means compromise.

Environment and Main Components

Microsoft Azure

Microsoft Defender portal

Microsoft Sentinel

Log Analytics workspace: LAW-Microsoft-Security-Lab

Azure Activity solution and data connector

Azure Policy

AzureActivity table

Kusto Query Language (KQL)

Microsoft Sentinel Analytics

MITRE ATT&CK

How the Environment Worked

A major lesson from the lab was that Microsoft Sentinel is not the same thing as the Log Analytics workspace. The workspace is the data and analytics layer where logs are organized into tables. Sentinel adds security operations features such as hunting, analytics rules, alerts, incidents, workbooks, watchlists, and automation.

Azure Subscription
        |
        v
Azure Activity Log
        |
        v
Azure Activity Data Connector
        |
        v
Log Analytics Workspace
        |
        v
AzureActivity Table
        |
        v
KQL
        |
        +----------------------+
        |                      |
        v                      v
Manual Hunting          Analytics Rules
                               |
                               v
                             Alert
                               |
                               v
                            Incident
                               |
                               v
                         Investigation

Configuring Azure Activity Data Collection

I installed the Azure Activity solution in Microsoft Sentinel and configured the Azure Activity connector. Azure Policy was used to send subscription-level administrative activity to LAW-Microsoft-Security-Lab. The policy used a system-assigned managed identity and the required permissions included Monitoring Contributor and Log Analytics Contributor.

After the connector became active, Azure control-plane activity could be collected and queried through the AzureActivity table.

Generating Safe Test Telemetry

To create real activity without performing anything harmful, I used a test resource group named rg-sentinel-kql-lab3-test. I created and updated the resource group and changed tags. These normal administrative actions produced Azure Activity records that could be used for the rest of the lab.

Understanding the AzureActivity Table

The AzureActivity table contains records of Azure administrative operations. I focused on fields that help answer common investigation questions.

Field
	

What it helps answer

TimeGenerated
	

When did the activity happen?

OperationNameValue
	

What action was performed?

ActivityStatusValue
	

Did the action succeed or fail?

ResourceGroup
	

Which resource group was involved?

Caller
	

Who performed the action?

CallerIpAddress
	

Where did the request come from?

ResourceId
	

Which Azure resource was affected?

KQL Fundamentals Practiced

I used KQL as an investigation tool. The basic idea was to start with a table and then pass the results through different operators until the data answered a specific security question.

Basic Query

AzureActivity
| take 10

This returned ten records from the AzureActivity table and showed the basic KQL pipeline structure.

Filtering by Time

AzureActivity
| where TimeGenerated >= ago(24h)

This limited the investigation to activity from the previous 24 hours.

Selecting Useful Fields and Sorting

AzureActivity
| where TimeGenerated >= ago(24h)
| project TimeGenerated,
          OperationNameValue,
          ActivityStatusValue,
          ResourceGroup,
          Caller,
          CallerIpAddress
| order by TimeGenerated desc

The project operator reduced unnecessary columns, while order by TimeGenerated desc placed the newest activity first. This created a cleaner investigation timeline.

Filtering to the Lab Resource Group

AzureActivity
| where TimeGenerated >= ago(24h)
| where ResourceGroup =~ "rg-sentinel-kql-lab3-test"
| project TimeGenerated,
          OperationNameValue,
          ActivityStatusValue,
          ResourceGroup,
          Caller,
          CallerIpAddress
| order by TimeGenerated desc

This was one of the strongest queries in the lab because it narrowed all Azure administrative activity down to one specific investigation target.

Summarizing Activity by Status

AzureActivity
| where TimeGenerated >= ago(24h)
| summarize EventCount = count() by ActivityStatusValue
| order by EventCount desc

This changed the view from individual log records into grouped information. It helped show how many events had each activity status.

Finding the Most Common Azure Operations

AzureActivity
| where TimeGenerated >= ago(24h)
| summarize EventCount = count() by OperationNameValue
| top 10 by EventCount desc

This introduced basic behavioral analysis by showing which Azure administrative operations appeared most often.

Grouping Events by Hour

AzureActivity
| where TimeGenerated >= ago(24h)
| summarize EventCount = count() by bin(TimeGenerated, 1h)
| order by TimeGenerated asc

The bin function grouped timestamps into one-hour periods. This same idea can later help identify spikes such as repeated logins, scanning, malware activity, or unusual data transfers.

Manual Hunting vs. Automated Detection

Manual hunting is analyst-driven. I decide what question I want to answer, run KQL, and review the results. An analytics rule takes detection logic and runs it automatically. If the rule conditions are met, Sentinel can create an alert and then an incident.

Manual Hunting:
Analyst -> KQL -> Results -> Investigation

Automated Detection:
Telemetry -> Analytics Rule -> Alert -> Incident -> Investigation

Suspicious Resource Deployment Analytics Rule

I reviewed and enabled the Microsoft Sentinel rule named Suspicious Resource deployment. The rule used Azure Activity as its data source, was configured as a scheduled rule, and had a Low severity.

The detection was mapped to the MITRE ATT&CK Impact tactic and technique T1496 - Resource Hijacking. The rule is designed to identify rare resource or resource group deployment behavior involving a previously unseen caller.

A key lesson was that this mapping does not prove that resource hijacking happened. It tells the analyst what type of attacker behavior the rule is designed to detect.

Analytics Rule Creation

I moved through the analytics rule wizard and reviewed the General, Set rule logic, Incident settings, Automated response, and Review + create sections. I left automated response unconfigured because playbooks and Logic Apps are better suited for a later SOAR-focused lab. After validation passed, I saved the rule and verified that it appeared as an active rule.

Alert and Incident Investigation

The active rule produced an incident titled Suspicious Resource deployment. The incident was Low severity, Active, and contained one alert. The alert showed Microsoft Sentinel as the service source and Scheduled detection as the detection source.

The incident also contained user and IP-related entities. One visible user was fitzgerald.afari7, another identity appeared as a GUID-style value, and an IPv6-related evidence item was associated with the alert.

Instead of treating the alert title as proof of an attack, I reviewed the detection details, timestamps, users, IP-related evidence, MITRE ATT&CK mapping, workspace, and related entities. This reinforced the idea that an alert starts an investigation; it does not finish one.

Important KQL Operators Learned

Operator / Function
	

Purpose

where
	

Filters rows

project
	

Selects useful columns

take
	

Limits the number of returned records

order by
	

Sorts results

summarize
	

Groups and aggregates records

count()
	

Counts records

top
	

Returns highest-ranked results

ago()
	

Creates a relative time range

bin()
	

Groups timestamps into larger time periods

contains
	

Searches text for a value

=~
	

Case-insensitive equality

Key Technical Findings

Azure administrative activity successfully reached Microsoft Sentinel through the Azure Activity data path.

The AzureActivity table provided structured and searchable telemetry.

KQL could narrow a large set of events into a specific resource group and useful evidence fields.

KQL aggregation could turn raw events into useful summaries and patterns.

The same Azure Activity telemetry could support both manual hunting and automated detection.

The Suspicious Resource deployment rule connected cloud telemetry to MITRE ATT&CK T1496 - Resource Hijacking.

A Sentinel alert could become an incident containing users, IP evidence, timestamps, and investigation context.

An alert name or MITRE mapping alone is not enough to prove malicious activity.

Skills Practiced

Microsoft Sentinel navigation and configuration

Azure Log Analytics

Azure Activity telemetry

KQL querying and filtering

KQL aggregation and sorting

Threat hunting

Analytics rule review and creation

MITRE ATT&CK mapping

Alert triage

Incident investigation

Cloud security monitoring

Evidence-based security analysis

Conclusion

This lab helped me understand the full path from cloud activity to a security investigation. I configured Azure Activity telemetry, worked with a Log Analytics workspace, queried the AzureActivity table with KQL, filtered and summarized real events, reviewed and enabled an analytics rule, and investigated the alert and incident created by Sentinel.

The biggest lesson was that the important skill is not memorizing where to click. The important skill is understanding how telemetry is collected, how it is searched, how detection logic uses it, and how an analyst validates the evidence before making a decision.