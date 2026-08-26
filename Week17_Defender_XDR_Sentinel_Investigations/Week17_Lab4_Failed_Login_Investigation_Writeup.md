# Week 17 — Lab 4: Failed Login Investigation

## Microsoft Sentinel Identity and Authentication Investigation

### Lab Overview

In this lab, I completed an end-to-end failed login investigation using Microsoft Sentinel, Microsoft Entra ID, Log Analytics, and Kusto Query Language (KQL).

The main goal of the lab was to investigate repeated failed authentication attempts and determine whether the activity represented normal user behavior, expected testing activity, or a possible security threat.

I started by making sure Microsoft Entra sign-in logs were being sent to my Log Analytics workspace. I then used KQL in Microsoft Sentinel to search the `SigninLogs` table and investigate failed authentication activity.

After reviewing the raw authentication events, I analyzed the affected user, source IP address, timestamps, failure reasons, and number of failed attempts.

I also created and tested a custom Microsoft Sentinel scheduled analytics rule designed to detect repeated failed Microsoft Entra sign-ins. During testing, I discovered an issue with my original time-based detection logic. I investigated the problem, corrected the KQL, tested the rule again, and successfully generated an alert and security incident.

Finally, I investigated the generated incident in Microsoft Defender, reviewed the evidence, and resolved the incident after confirming that the activity was expected because I intentionally generated the failed logins as part of this controlled lab.

---

# Lab Objectives

The main objectives of this lab were to:

* Understand how authentication activity appears in Microsoft Entra ID.
* Send Microsoft Entra sign-in logs to a Log Analytics workspace.
* Use Microsoft Sentinel to investigate authentication activity.
* Learn how to query the `SigninLogs` table using KQL.
* Identify failed authentication attempts.
* Identify the user involved in the activity.
* Identify the source IP address.
* Analyze authentication failure reasons.
* Review the timing and frequency of failed login attempts.
* Build an authentication timeline.
* Create a custom scheduled analytics rule.
* Detect repeated failed login attempts automatically.
* Map account and IP entities to a Sentinel detection.
* Generate a Microsoft Sentinel security alert.
* Generate and investigate a Microsoft Defender incident.
* Make an evidence-based final decision.
* Properly resolve and document the incident.

---

# Technologies and Security Concepts Used

## Platforms and Tools

* Microsoft Azure
* Microsoft Sentinel
* Microsoft Defender
* Microsoft Entra ID
* Log Analytics Workspace
* Kusto Query Language (KQL)
* Google Chrome
* Windows 11

## Security Concepts

* Identity security
* Authentication
* Failed authentication
* Sign-in telemetry
* SIEM
* Log analysis
* Detection engineering
* Alert triage
* Incident investigation
* Entity mapping
* Timeline analysis
* Evidence correlation
* MITRE ATT&CK
* Incident classification
* Incident resolution

---

# Lab Environment

The investigation was performed in my Microsoft Azure security lab environment.

My Microsoft Sentinel instance was connected to a Log Analytics workspace that I used to store and search security telemetry.

The important data source for this investigation was Microsoft Entra ID sign-in activity.

The main Log Analytics table used during the investigation was:

```kusto
SigninLogs
```

This table contains information about Microsoft Entra interactive sign-in attempts.

Some of the important fields I investigated included:

* `TimeGenerated`
* `UserPrincipalName`
* `IPAddress`
* `AppDisplayName`
* `ResultType`
* `ResultDescription`
* `Location`
* `ConditionalAccessStatus`
* `AuthenticationRequirement`

---

# Phase 1 — Verifying Available Security Data

Before beginning the failed login investigation, I needed to confirm what data was available inside my Log Analytics workspace.

At first, I attempted to query the `SigninLogs` table.

However, no results were available.

Instead of assuming that there were simply no failed login attempts, I checked the tables that actually contained data.

I used a query similar to:

```kusto
search *
| summarize Events=count() by $table
| sort by Events desc
```

This helped me identify which tables were currently receiving information.

This was an important troubleshooting step because a SIEM can only investigate information that is actually being collected.

---

# Phase 2 — Configuring Microsoft Entra Diagnostic Logs

I discovered that Microsoft Entra sign-in telemetry was not yet available in my Log Analytics workspace.

I went to the Microsoft Entra diagnostic settings and created a diagnostic setting for the lab.

The diagnostic setting was configured to send important identity logs to my Log Analytics workspace.

The important log categories included:

* `AuditLogs`
* `SignInLogs`

The destination was my Microsoft security Log Analytics workspace.

This created the data path:

```text
Microsoft Entra ID
        ↓
Authentication activity
        ↓
Diagnostic settings
        ↓
Log Analytics Workspace
        ↓
Microsoft Sentinel
```

This step helped me understand that Microsoft Sentinel does not automatically contain every possible security log.

The correct data source must first be connected or configured.

---

# Phase 3 — Investigating Failed Authentication Events

After the sign-in telemetry began reaching the workspace, I returned to Microsoft Sentinel Logs.

I queried the `SigninLogs` table and filtered for unsuccessful authentication attempts.

One of the main KQL filters I used was:

```kusto
SigninLogs
| where ResultType != 0
```

A successful Microsoft Entra authentication normally has a `ResultType` of `0`.

Filtering for:

```kusto
ResultType != 0
```

allowed me to focus on unsuccessful authentication attempts.

I then selected important investigation fields.

Example:

```kusto
SigninLogs
| where ResultType != 0
| project
    TimeGenerated,
    UserPrincipalName,
    IPAddress,
    Location,
    AppDisplayName,
    ResultType,
    ResultDescription
| sort by TimeGenerated desc
```

This gave me a much cleaner view of the authentication activity.

---

# Phase 4 — Understanding the Authentication Evidence

I did not want to classify the activity based only on the fact that failed logins existed.

Failed authentication can happen for many reasons.

For example:

* A user mistyped a password.
* A user forgot a password.
* An application stored an old password.
* A password was recently changed.
* An attacker was guessing passwords.
* An attacker was performing password spraying.
* An automated process was repeatedly trying invalid credentials.
* The activity was intentionally created during security testing.

Because of this, I reviewed several pieces of evidence together.

---

## User Identity

I identified the user associated with the failed authentication events.

This answered:

> Which account was receiving the failed authentication attempts?

Knowing the affected identity is important because the risk can change depending on the account.

For example, repeated authentication attempts against an administrator account may require more attention than attempts against a low-privilege test account.

---

## Source IP Address

I reviewed the `IPAddress` field.

This answered:

> Where were the authentication attempts coming from?

Multiple failed authentication attempts from the same IP address can indicate that one system or user is repeatedly attempting authentication.

However, the IP address alone is not enough to prove malicious activity.

It must be combined with other evidence.

---

## Application

I reviewed:

```text
AppDisplayName
```

This helped identify which Microsoft application or service the authentication attempt was trying to access.

Knowing the application adds important context to the investigation.

---

## Result Code

I reviewed:

```text
ResultType
```

The failed login activity included the Microsoft Entra result code associated with unsuccessful authentication.

The result information helped confirm that the events were real authentication failures rather than simply suspicious alerts without supporting telemetry.

---

## Failure Description

I also reviewed:

```text
ResultDescription
```

This provided more information about why authentication failed.

This is important because a SOC analyst should not stop at:

> "The login failed."

A better investigation asks:

> "Why did the login fail?"

---

# Phase 5 — Timeline Analysis

I sorted authentication events using their timestamps.

Example:

```kusto
| sort by TimeGenerated desc
```

This allowed me to determine the order in which authentication attempts occurred.

Timeline analysis helped answer questions such as:

* When did the activity begin?
* When did it stop?
* How quickly did the attempts occur?
* Were the attempts spread across hours?
* Did several attempts happen within only a few minutes?
* Did a successful authentication happen afterward?

Time is extremely important during an investigation.

Three failed attempts over several months mean something very different from three failed attempts within a few seconds or minutes.

---

# Phase 6 — Creating Controlled Failed Login Activity

To test the detection in a controlled environment, I intentionally generated failed authentication attempts using my dedicated lab account.

I entered incorrect credentials several times.

I then returned to Microsoft Sentinel and confirmed that the authentication failures appeared in `SigninLogs`.

This allowed me to test the detection using known activity.

The activity was authorized and intentionally created for this security lab.

---

# Phase 7 — Validating the Fresh Authentication Events

I used a query similar to:

```kusto
SigninLogs
| where TimeGenerated > ago(30m)
| where ResultType != 0
| project
    TimeGenerated,
    UserPrincipalName,
    IPAddress,
    AppDisplayName,
    ResultType,
    ResultDescription
| sort by TimeGenerated desc
```

The query showed multiple recent failed sign-in attempts.

The important evidence showed:

* The same lab account.
* The same source IP address.
* Multiple authentication failures.
* Closely related timestamps.
* Failed authentication results.

At this point, I had confirmed that the source telemetry existed.

---

# Phase 8 — Building the Microsoft Sentinel Analytics Rule

I created a custom scheduled analytics rule called:

**Repeated Failed Microsoft Entra Sign-ins**

The purpose of the rule was to automatically detect repeated failed authentication attempts involving the same user and source IP address.

Instead of manually searching the logs every time, Sentinel could run the KQL automatically.

The detection process was:

```text
SigninLogs
      ↓
Find authentication failures
      ↓
Group related events
      ↓
Count failed attempts
      ↓
Compare count to threshold
      ↓
Generate alert when threshold is reached
```

---

# Phase 9 — Entity Mapping

I configured entity mappings for the analytics rule.

The important entities were:

## Account

The account entity represented the user involved in the authentication activity.

Fields included:

* Account name
* UPN suffix

## IP

The IP entity represented the source IP address associated with the authentication attempts.

Entity mapping is useful because Sentinel can connect raw log information to actual security objects.

Instead of only seeing text inside a log, the analyst can understand:

> This is the affected account.

and:

> This is the source IP.

---

# Phase 10 — Custom Alert Details

I also included useful information in the detection results.

These details included:

* `FailedAttempts`
* `FirstAttempt`
* `LastAttempt`

This made the resulting alert easier to investigate.

Instead of forcing an analyst to immediately run another query, the alert could already explain how many failures occurred and when the activity started and ended.

---

# Phase 11 — Detection Logic Problem

During testing, I ran into an important problem.

My original query used:

```kusto
bin(TimeGenerated, 5m)
```

The purpose was to group authentication failures into five-minute periods.

At first, this seemed correct.

However, my three failed authentication attempts happened around a five-minute clock boundary.

For example, the events occurred approximately around:

```text
8:29
8:32
8:33
```

Even though these events happened only a few minutes apart, the fixed five-minute bins separated them.

Sentinel effectively saw:

```text
8:25–8:30
1 failure

8:30–8:35
2 failures
```

My rule required at least three failures.

Neither group contained three failures.

Because of this, the query returned zero detection results.

---

# Phase 12 — Troubleshooting the Detection

This was one of the most important lessons from the lab.

The raw logs proved that the authentication failures existed.

Therefore, the problem was not:

* Missing telemetry.
* A broken data connector.
* Missing authentication activity.

The problem was the detection logic.

I compared the raw `SigninLogs` events to the analytics query and realized that the fixed time buckets were splitting activity that I wanted to treat as one sequence.

This taught me an important detection engineering lesson:

> A KQL query can be technically valid but still fail to detect the behavior I actually intended to detect.

---

# Phase 13 — Correcting the Detection Logic

I changed the detection logic so the rule could evaluate the authentication failures across its configured lookback period.

The corrected logic included:

```kusto
SigninLogs
| where ResultType != 0
| extend
    AccountName = tostring(split(UserPrincipalName, "@")[0]),
    AccountUPNSuffix = tostring(split(UserPrincipalName, "@")[1])
| summarize
    FailedAttempts = count(),
    FirstAttempt = min(TimeGenerated),
    LastAttempt = max(TimeGenerated)
    by UserPrincipalName, AccountName, AccountUPNSuffix, IPAddress
| where FailedAttempts >= 3
| extend TimeGenerated = LastAttempt
| project
    TimeGenerated,
    UserPrincipalName,
    AccountName,
    AccountUPNSuffix,
    IPAddress,
    FailedAttempts,
    FirstAttempt,
    LastAttempt
```

This query performed several important actions.

### `where ResultType != 0`

This kept only failed authentication attempts.

### `extend`

This separated the user's UPN into values that could be used for account entity mapping.

### `summarize`

This grouped related authentication failures together.

### `count()`

This calculated how many failures occurred.

### `min(TimeGenerated)`

This identified the first failed attempt.

### `max(TimeGenerated)`

This identified the last failed attempt.

### `where FailedAttempts >= 3`

This created the actual detection threshold.

If fewer than three failures were present, the query did not return a detection.

If three or more were present, the activity matched the detection condition.

---

# Phase 14 — Validating the Corrected Rule

After correcting the KQL, I tested the detection again.

The query successfully returned a matching result.

This proved that:

```text
Authentication telemetry → Working

KQL filtering → Working

Event grouping → Working

Threshold logic → Working

Detection query → Working
```

This was an important validation step before relying on the automated alert.

---

# Phase 15 — Scheduled Detection

The analytics rule was configured as a scheduled detection.

The rule automatically searched recent authentication telemetry for the repeated failed login pattern.

The rule was enabled so Microsoft Sentinel could continuously apply the detection logic without requiring me to manually run the KQL every time.

This helped me understand the difference between threat hunting and automated detection.

With manual hunting:

```text
Analyst
   ↓
Writes KQL
   ↓
Runs query
   ↓
Reviews results
```

With an analytics rule:

```text
Sentinel
   ↓
Runs KQL automatically
   ↓
Detects matching activity
   ↓
Generates alert
   ↓
Creates incident
   ↓
Analyst investigates
```

---

# Phase 16 — Alert Generation

After generating fresh controlled authentication failures and allowing the scheduled analytics rule to run, the detection triggered successfully.

Microsoft Sentinel generated an alert for:

**Repeated Failed Microsoft Entra Sign-ins**

The alert contained useful information about the matching authentication activity.

This confirmed that the automated detection was functioning as expected.

---

# Phase 17 — Incident Generation

The alert resulted in a Microsoft Defender incident.

The incident was named:

**Repeated Failed Microsoft Entra Sign-ins**

The incident was assigned a **Medium** severity.

At this point, I moved from detection engineering into incident investigation.

The question was no longer only:

> Did the rule work?

The question became:

> What actually happened, and what should I do about it?

---

# Phase 18 — Incident Investigation

Inside the incident, I reviewed the available evidence.

This included:

* Alert information.
* Affected account.
* Authentication activity.
* Source IP information.
* Failed attempt count.
* First observed attempt.
* Last observed attempt.
* Related query results.
* Incident activity.

The evidence confirmed that the detection had correctly identified the repeated authentication failures.

---

# Phase 19 — Evidence-Based Classification

Even though the activity looked suspicious from the point of view of the detection rule, I knew that the failed authentication attempts were intentionally generated during the lab.

This created an important difference between:

**Detection**

and:

**Final analyst conclusion**

The detection was correct because repeated authentication failures really happened.

However, the final security context showed that the behavior was authorized.

My reasoning was:

```text
Did multiple authentication failures occur?
YES

Did the detection identify them correctly?
YES

Was the activity intentionally generated?
YES

Was this an authorized security lab?
YES

Was there evidence of an actual account compromise?
NO
```

Therefore, I classified the activity as expected confirmed activity rather than treating it as an actual attack.

---

# Phase 20 — Incident Resolution

After completing the investigation, I used the Manage Incident screen.

I changed the incident status to:

**Resolved**

I classified the incident as:

**Informational, expected activity — Confirmed activity**

I also documented the reason for my decision.

The investigation determined that the authentication failures were intentionally generated during an authorized lab exercise and that there was no evidence of unauthorized account compromise.

I then saved the incident.

This completed the incident lifecycle.

---

# Final Investigation Conclusion

The investigation confirmed that multiple failed Microsoft Entra authentication attempts occurred against the dedicated lab account from the same source IP address within a short period.

The authentication activity was successfully collected in Microsoft Entra sign-in telemetry and forwarded to the Log Analytics workspace.

KQL analysis confirmed the failed authentication events and allowed me to analyze the user, source IP, timestamps, application, failure information, and number of attempts.

A custom Microsoft Sentinel scheduled analytics rule was created to detect repeated failed sign-ins.

During testing, the original detection logic did not trigger because fixed five-minute time buckets separated closely timed authentication events. I identified the problem by comparing the raw logs with the detection results.

I corrected the KQL logic and successfully validated the detection.

The updated analytics rule generated the expected alert and Microsoft Defender incident.

After reviewing the incident evidence, I determined that the activity was expected because the failed logins were intentionally generated as part of the authorized lab.

The incident was resolved as expected confirmed activity.

No evidence of unauthorized account compromise was identified.

---

# Final Classification

**Incident:** Repeated Failed Microsoft Entra Sign-ins

**Severity:** Medium

**Final Status:** Resolved

**Classification:** Informational, expected activity — Confirmed activity

**Security Impact:** No confirmed security impact

**Account Compromise:** Not identified

**Detection Result:** Successful

---

# What I Learned

This lab taught me that a failed login by itself does not automatically mean an attack occurred.

Security investigations require context.

I learned to ask:

* Which user was involved?
* What IP address generated the activity?
* When did it happen?
* How many attempts occurred?
* Why did authentication fail?
* What application was involved?
* Were there successful logins nearby?
* Does the activity match normal behavior?
* Is there enough evidence to call the activity malicious?

I also learned that collecting the correct telemetry is extremely important.

If Microsoft Entra sign-in logs are not reaching the Log Analytics workspace, even a perfect KQL query cannot investigate them.

Another major lesson was that detection logic must be tested.

My first query looked correct, but the fixed five-minute time grouping caused the detection to miss the activity.

Instead of assuming Sentinel was broken, I went back to the raw evidence and worked through the detection step by step.

This taught me to separate three questions:

1. Does the data exist?
2. Does my query correctly find the data?
3. Does the automated rule correctly turn the data into a detection?

That troubleshooting process helped me better understand how SIEM detections actually work.

---

# Skills Demonstrated

This project helped me practice:

* Microsoft Sentinel administration
* Microsoft Entra ID log analysis
* Log Analytics
* KQL
* Authentication investigation
* Failed login analysis
* Identity security monitoring
* SIEM investigation
* Detection engineering
* Analytics rule creation
* Threshold-based detection
* Entity mapping
* Alert enrichment
* MITRE ATT&CK mapping
* Security alert investigation
* Incident triage
* Timeline analysis
* Evidence correlation
* Detection troubleshooting
* Incident classification
* Incident documentation
* Incident resolution

---

# Portfolio Screenshot Evidence

The following screenshots can be included with this project:

```text
week17_Lab4_initial_failed_signins.png
week17_Lab4_failed_signins_by_user.png
week17_Lab4_failed_signins_by_ip.png
week17_Lab4_user_authentication_timeline.png
week17_Lab4_failure_reason_analysis.png
week17_Lab4_recent_failed_signins.png
week17_Lab4_detection_rule_validation.png
week17_Lab4_detection_rule_match.png
week17_Lab4_failed_login_analytics_rule.png
week17_Lab4_failed_login_incident_queue.png
week17_Lab4_failed_login_alert_evidence.png
week17_Lab4_incident_overview.png
week17_Lab4_affected_entities.png
week17_Lab4_final_incident_disposition.png
```

Only screenshots that clearly show useful evidence should be included in the final GitHub repository.

---

# Interview-Ready Explanation

If I were asked about this project during an interview, I would explain it like this:

> I built a failed login investigation in Microsoft Sentinel using Microsoft Entra sign-in logs and KQL. I configured Entra diagnostic settings to send sign-in telemetry into my Log Analytics workspace and then used the `SigninLogs` table to investigate failed authentication attempts.
>
> I analyzed the affected account, source IP address, timestamps, failure information, application, and number of attempts. I then created a scheduled Sentinel analytics rule to automatically detect repeated failed sign-ins from the same user and source IP.
>
> During testing, I found that my original query used fixed five-minute time buckets. My test attempts happened across one of those bucket boundaries, so the rule did not trigger even though the failures happened only a few minutes apart. I went back to the raw logs, confirmed the events were there, identified the time-grouping problem, corrected the KQL, and tested it again.
>
> After the correction, the detection successfully matched the activity and generated a Microsoft Defender incident. I investigated the alert and related entities and confirmed that the failed logins were activity I had intentionally generated for the lab. I resolved the incident as expected confirmed activity because the detection was correct, but the behavior was authorized.
>
> The biggest thing I learned was that a working SIEM query is not just about correct syntax. The logic has to correctly represent the behavior I am trying to detect, and the final incident decision has to be based on evidence and context.

---

# Overall Result

**Lab Status: Successfully Completed**

This lab demonstrated a complete security monitoring workflow:

```text
Authentication Activity
        ↓
Microsoft Entra ID
        ↓
Diagnostic Logging
        ↓
Log Analytics
        ↓
Microsoft Sentinel
        ↓
KQL Analysis
        ↓
Detection Logic
        ↓
Scheduled Analytics Rule
        ↓
Security Alert
        ↓
Microsoft Defender Incident
        ↓
Investigation
        ↓
Evidence-Based Decision
        ↓
Incident Resolution
```

The project strengthened my understanding of identity security, authentication telemetry, KQL, Microsoft Sentinel, detection engineering, and the complete SOC incident investigation process.
