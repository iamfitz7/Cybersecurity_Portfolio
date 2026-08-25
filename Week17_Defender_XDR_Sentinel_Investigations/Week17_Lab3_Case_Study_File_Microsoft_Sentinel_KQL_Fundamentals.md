Week #17 Lab #3- Microsoft Sentinel & KQL Fundamentals

Case Study: From Azure Activity to a Sentinel Incident

Case Study Summary

This case study documents how I used Microsoft Sentinel, Azure Activity, Log Analytics, and KQL to build and follow a basic cloud security monitoring workflow. The goal was to collect real Azure administrative activity, investigate it manually with KQL, and then see how Microsoft Sentinel could use the same type of telemetry for automated detection and incident creation.

The Security Problem

Azure environments generate many administrative events. Users create resources, update settings, change tags, and perform other management actions every day. Most of this activity is normal, but attackers with access to a cloud account can perform similar actions.

The security challenge is to collect this activity, search it, find unusual behavior, and decide whether an alert represents expected administration or a real security problem.

Main Question

How can Azure administrative activity be collected and analyzed so that a security analyst can investigate it manually and also use it for automated detection?

Phase 1 - Building the Telemetry Path

Microsoft Sentinel was enabled on the Log Analytics workspace LAW-Microsoft-Security-Lab. I installed the Azure Activity solution and configured the Azure Activity connector so subscription-level administrative activity could reach the workspace.

Azure Policy was used to support the connection. A system-assigned managed identity and the required monitoring and Log Analytics permissions were part of the setup.

Azure Activity
     ↓
Azure Activity Connector
     ↓
Log Analytics Workspace
     ↓
AzureActivity Table

Phase 2 - Generating Controlled Test Activity

I created and worked with a test resource group named rg-sentinel-kql-lab3-test. I used normal actions such as creating the resource group, updating it, and writing tags. These actions were safe, but they still generated real Azure Activity telemetry.

Phase 3 - Verifying the Data with KQL

Once the telemetry was available, I used KQL to verify and investigate the records.

AzureActivity
| take 10

This first query confirmed that the AzureActivity table contained data.

Phase 4 - Narrowing the Investigation

A SIEM can contain a large amount of data, so I practiced reducing that data to only the events that mattered.

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

This query limited the time range, focused on the Lab 3 resource group, selected useful fields, and sorted the events with the newest first.

This was an important investigation step because it showed how an analyst can start with a large dataset and narrow it to one target.

Phase 5 - Looking for Patterns

I then used KQL aggregation to move from individual events to patterns.

AzureActivity
| where TimeGenerated >= ago(24h)
| summarize EventCount = count() by ActivityStatusValue
| order by EventCount desc

This grouped events by status and counted them.

AzureActivity
| where TimeGenerated >= ago(24h)
| summarize EventCount = count() by OperationNameValue
| top 10 by EventCount desc

This showed the most common Azure operations in the selected time period. The main lesson was that individual events explain what happened once, while aggregation helps show what is happening repeatedly.

Phase 6 - Moving from Hunting to Automated Detection

Up to this point, I was doing manual hunting. I decided when to run the query and I reviewed the results myself.

Next, I reviewed the Microsoft Sentinel analytics rule named Suspicious Resource deployment. The rule was a scheduled detection using Azure Activity data.

It was mapped to the MITRE ATT&CK Impact tactic and T1496 - Resource Hijacking. The rule was designed to identify rare resource or resource group deployment behavior involving a previously unseen caller.

Phase 7 - Enabling the Analytics Rule

I worked through the analytics rule wizard and reviewed the rule name, severity, detection logic, schedule, incident settings, and automated response section. I kept the rule enabled and did not add an automation playbook because automated response was outside the scope of this fundamentals lab.

After validation passed, I saved the rule and confirmed that it appeared under Active rules.

Phase 8 - Sentinel Incident

The rule produced an incident titled Suspicious Resource deployment. The incident was Low severity, Active, and contained one alert.

The alert identified Microsoft Sentinel as the service source and Scheduled detection as the detection source. The incident also showed MITRE ATT&CK T1496 - Resource Hijacking.

Phase 9 - Reviewing the Evidence

The incident contained user and IP-related entities. One visible user was fitzgerald.afari7. Another identity appeared as a GUID-style value, and an IPv6-related evidence item was also associated with the alert.

I did not assume the alert title meant an attacker had definitely hijacked Azure resources. Instead, I reviewed the rule description, timestamps, users, IP evidence, workspace, service source, and MITRE mapping.

Why the Alert Needed Investigation

The rule identifies unusual behavior, not guaranteed malicious behavior. A previously unseen caller performing a rare resource deployment can be suspicious, but legitimate administration can also appear unusual.

Detection
    ↓
Collect Context
    ↓
Review Evidence
    ↓
Validate Activity
    ↓
Make a Decision

This helped reinforce one of the most important SOC lessons from the lab: an alert is the beginning of an investigation, not the final answer.

Main Findings

The Azure Activity telemetry pipeline was working.

The AzureActivity table could be queried with KQL.

Resource-specific filtering made it possible to isolate the Lab 3 activity from unrelated events.

KQL aggregation helped turn raw logs into useful summaries.

The same telemetry could support manual hunting and an automated analytics rule.

The Suspicious Resource deployment rule mapped the behavior to MITRE ATT&CK T1496 - Resource Hijacking.

The Sentinel incident collected alert, user, IP, timestamp, and other investigation context.

The evidence required investigation before any final security conclusion could be made.

What I Learned

The biggest lesson was that Microsoft Sentinel is not just an alert dashboard. It is part of a larger security workflow that begins with data collection and ends with an analyst decision.

Activity Happens
      ↓
Telemetry Is Generated
      ↓
Connector Collects It
      ↓
Log Analytics Stores It
      ↓
KQL Searches It
      ↓
Analytics Rule Detects It
      ↓
Alert Is Created
      ↓
Incident Is Investigated
      ↓
Analyst Makes a Decision

Final Outcome

This lab successfully connected cloud telemetry, KQL, automated detection, MITRE ATT&CK, and incident investigation into one complete workflow.

I came away with a stronger understanding of how a SOC analyst can move from raw Azure logs to focused evidence and then use that evidence to support a security decision. The lab also showed why understanding the underlying data and detection logic is more important than simply knowing where to click in the Sentinel interface.