# Incident Case Study — Repeated Failed Microsoft Entra Sign-ins

## Week 17 — Lab 4

### Executive Summary

This case study documents my investigation of repeated failed Microsoft Entra authentication attempts using Microsoft Sentinel and KQL.

The goal was to simulate suspicious authentication activity, collect the related identity telemetry, investigate the evidence, create an automated detection, and work the resulting security incident through its full lifecycle.

Multiple failed sign-in attempts were generated against a dedicated lab account.

The events were collected through Microsoft Entra ID and sent to my Log Analytics workspace. I used Microsoft Sentinel and KQL to investigate the affected user, source IP address, timestamps, authentication results, and failure information.

I then created a scheduled analytics rule to detect repeated failed Microsoft Entra sign-ins.

During testing, I discovered that the original detection logic separated closely timed authentication events into different five-minute groups. This prevented the rule from triggering.

I investigated the issue, corrected the KQL, validated the new detection logic, and successfully generated a Microsoft Defender security incident.

The final investigation determined that the activity was expected because I intentionally generated the authentication failures during an authorized lab exercise.

The incident was resolved as expected confirmed activity.

---

# Case Information

**Incident Name:** Repeated Failed Microsoft Entra Sign-ins

**Security Area:** Identity and Authentication

**SIEM:** Microsoft Sentinel

**Incident Platform:** Microsoft Defender

**Identity Platform:** Microsoft Entra ID

**Query Language:** KQL

**Log Source:** Microsoft Entra Sign-in Logs

**Primary Table:** `SigninLogs`

**Severity:** Medium

**Final Status:** Resolved

**Final Classification:** Informational, expected activity — Confirmed activity

**Confirmed Account Compromise:** No

---

# Security Problem

Repeated failed authentication attempts can be an early sign of an identity attack.

Possible causes include:

* Password guessing
* Brute-force attacks
* Password spraying
* Credential attacks
* Incorrect stored credentials
* User mistakes
* Automated authentication failures
* Authorized security testing

Because failed authentication can have both normal and malicious explanations, the events needed to be investigated before making a final decision.

The main question for this investigation was:

> Do these repeated failed authentication attempts represent suspicious or malicious activity, or is there a legitimate explanation?

---

# Detection Objective

The detection was designed to identify:

> Three or more failed Microsoft Entra sign-in attempts associated with the same user and source IP address during the analytics rule's recent lookback period.

The goal was not to automatically declare the user compromised.

The goal was to identify behavior that deserved investigation.

This distinction is important.

A detection tells the analyst:

> "Something happened that matches our security logic."

An investigation determines:

> "What does that activity actually mean?"

---

# Data Collection

The investigation depended on Microsoft Entra authentication telemetry.

Microsoft Entra diagnostic settings were configured to send identity logs to the Log Analytics workspace used by Microsoft Sentinel.

Important log categories included:

* `SignInLogs`
* `AuditLogs`

The data path was:

```text
User authentication attempt
        ↓
Microsoft Entra ID
        ↓
Sign-in telemetry
        ↓
Diagnostic settings
        ↓
Log Analytics Workspace
        ↓
Microsoft Sentinel
```

Without this data pipeline, the SIEM would not have the authentication evidence needed for the investigation.

---

# Initial Investigation

I began by querying:

```kusto
SigninLogs
```

I focused on unsuccessful authentication attempts using:

```kusto
| where ResultType != 0
```

I then reviewed important fields including:

```text
TimeGenerated
UserPrincipalName
IPAddress
AppDisplayName
ResultType
ResultDescription
Location
```

These fields helped answer the main investigation questions.

---

# Investigation Question 1 — Which account was involved?

I reviewed:

```text
UserPrincipalName
```

This identified the account associated with the failed authentication attempts.

The account was a dedicated lab account used for controlled security testing.

This was important information, but I did not use that fact alone to close the investigation.

I continued validating the actual telemetry.

---

# Investigation Question 2 — Where did the activity come from?

I reviewed:

```text
IPAddress
```

The failed authentication attempts were associated with the same source IP address.

This increased the relationship between the events.

Instead of several unrelated failures from completely different locations, I was looking at a group of authentication attempts sharing common characteristics.

---

# Investigation Question 3 — When did the activity happen?

I reviewed:

```text
TimeGenerated
```

and sorted the events by time.

The authentication failures occurred close together.

This was important because frequency changes the meaning of authentication activity.

For example:

```text
3 failures over several months
```

is very different from:

```text
3 failures within a few minutes
```

The second pattern is much more useful for a repeated failed login detection.

---

# Investigation Question 4 — Why did authentication fail?

I reviewed:

```text
ResultType
```

and:

```text
ResultDescription
```

This helped confirm that I was looking at actual authentication failures and provided additional information about why the sign-ins were unsuccessful.

Understanding the reason for a failed login is important because not every authentication failure represents the same behavior.

---

# Investigation Question 5 — What application was involved?

I reviewed:

```text
AppDisplayName
```

This provided context about which Microsoft service or application was involved in the authentication attempt.

Application context helps an analyst better understand what the user or possible attacker was attempting to access.

---

# Controlled Testing

To validate the detection, I intentionally generated multiple failed authentication attempts against the dedicated lab account.

Incorrect credentials were entered several times within a short period.

Afterward, I returned to Microsoft Sentinel and searched the recent authentication telemetry.

The events appeared successfully.

This confirmed:

```text
The activity occurred.
        ↓
Microsoft Entra recorded it.
        ↓
The logs reached Log Analytics.
        ↓
Microsoft Sentinel could query it.
```

---

# KQL Investigation

One of the queries used to review recent failures followed this structure:

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

This provided a clean view of recent failed authentication events.

I could now correlate:

```text
USER
+
SOURCE IP
+
TIME
+
APPLICATION
+
RESULT
+
FAILURE REASON
```

This gave me much stronger evidence than simply reading an alert title.

---

# Building the Automated Detection

After validating the raw telemetry, I created a Microsoft Sentinel scheduled analytics rule.

The rule was named:

**Repeated Failed Microsoft Entra Sign-ins**

The rule was designed to count repeated failed authentication attempts associated with the same user and source IP.

Important information was included in the detection results:

* User
* Source IP
* Failed attempt count
* First attempt
* Last attempt

I also configured entity mappings so Microsoft Sentinel could recognize the account and IP address as investigation entities.

---

# Detection Engineering Problem

The first version of my detection did not work as expected.

The raw logs clearly showed three failed authentication attempts.

However, the detection returned zero results.

Instead of immediately changing random settings, I compared the source telemetry with the KQL logic.

I discovered that the query used:

```kusto
bin(TimeGenerated, 5m)
```

This divided events into fixed five-minute clock periods.

The failures occurred around times similar to:

```text
8:29
8:32
8:33
```

A person would naturally look at those events and say:

> "Those three failures happened within only a few minutes."

However, the query grouped them differently:

```text
8:25–8:30 → 1 failure

8:30–8:35 → 2 failures
```

The detection threshold required:

```text
FailedAttempts >= 3
```

Neither fixed time group contained three events.

Therefore, the detection did not trigger.

---

# Root Cause

The problem was not missing authentication telemetry.

The problem was not Microsoft Sentinel failing to receive data.

The problem was not the authentication events themselves.

The root cause was:

> The detection's fixed time grouping did not correctly represent the behavior I intended to detect.

This was an important detection engineering lesson.

---

# Detection Correction

I corrected the KQL so the repeated authentication events could be evaluated across the scheduled rule's recent lookback period.

The updated detection logic was:

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

The corrected query returned a matching result during validation.

This confirmed that the new logic successfully detected the authentication pattern.

---

# Alert Generation

After the corrected rule was enabled and fresh test activity was generated, Microsoft Sentinel successfully triggered the detection.

A security alert was generated for:

**Repeated Failed Microsoft Entra Sign-ins**

The alert contained the account and authentication evidence needed for investigation.

This proved that the detection had moved beyond manual KQL hunting and was functioning as an automated security control.

---

# Incident Creation

The security alert generated a Microsoft Defender incident.

The incident appeared as:

**Repeated Failed Microsoft Entra Sign-ins**

with a severity of:

**Medium**

I opened the incident and reviewed the available evidence.

---

# Incident Evidence

The investigation included evidence related to:

* The affected account
* Source IP address
* Authentication failures
* Failed attempt count
* First attempt timestamp
* Last attempt timestamp
* Analytics rule
* Alert
* Query results
* Incident activity

The evidence confirmed that the repeated authentication failures actually occurred.

---

# Analysis

At this stage, the technical detection was confirmed.

However, a correct detection does not automatically mean an attack occurred.

I considered two different questions.

### Question 1

Did the behavior detected by Sentinel actually happen?

**Yes.**

Multiple failed authentication attempts occurred.

### Question 2

Was the activity unauthorized or malicious?

**No evidence showed that it was.**

I had intentionally generated the failed authentication attempts as part of the authorized security lab.

Therefore, the rule correctly detected real behavior, but the behavior itself had a legitimate explanation.

---

# Analyst Decision

I determined that the incident represented expected testing activity.

The reasoning was:

```text
Repeated failed authentications occurred
                ↓
Telemetry confirmed the events
                ↓
KQL confirmed the pattern
                ↓
Analytics rule correctly detected the activity
                ↓
Alert correctly generated
                ↓
Incident correctly generated
                ↓
Investigation confirmed authorized lab testing
                ↓
No evidence of unauthorized compromise
                ↓
RESOLVE
```

---

# Final Disposition

**Status:** Resolved

**Classification:** Informational, expected activity — Confirmed activity

**Severity:** Medium

**Account Compromise:** Not identified

**Malicious Activity Confirmed:** No

**Detection Functioned Correctly:** Yes

---

# Why I Did Not Treat the Alert as an Attack

Repeated failed authentication attempts can be suspicious.

However, security analysts should not make decisions based only on what an alert is called.

The alert title told me:

> Repeated failed Microsoft Entra sign-ins were detected.

That was true.

It did not tell me:

> A malicious attacker definitely attempted to compromise this account.

That conclusion required investigation.

After reviewing the identity, IP address, timestamps, authentication results, and testing context, I had enough evidence to explain the behavior.

The activity was intentionally generated during the lab.

Therefore, escalation or containment was not required.

---

# Containment Decision

No containment actions were required.

I did not need to:

* Disable the account.
* Reset the password.
* Block the IP address.
* Revoke sessions.
* Isolate a device.
* Escalate to incident response.

Those actions would not have been justified by the evidence because the authentication failures were authorized test activity.

In a real environment, those actions could become appropriate if additional evidence showed an actual credential attack or account compromise.

---

# What Would Make This More Serious in a Real Environment?

I would increase my concern if I discovered evidence such as:

* A large number of failed authentication attempts.
* Many accounts targeted from the same IP.
* One account targeted from many unusual IP addresses.
* Authentication attempts from unexpected countries.
* A successful login after many failures.
* Successful authentication from a new location.
* MFA changes after authentication.
* Password reset activity.
* New device registration.
* Privilege changes.
* Suspicious mailbox activity.
* Unusual cloud resource access.
* Impossible travel.
* Known malicious IP addresses.
* Additional alerts involving the same account.

Any of these could change the investigation from a simple failed login case into a possible account compromise.

---

# Lessons Learned

## 1. Alerts are the beginning of an investigation

An alert is not automatically proof of an attack.

The analyst still needs to investigate the evidence.

---

## 2. Raw logs matter

When my detection initially returned zero results, the raw `SigninLogs` proved that the authentication events existed.

That allowed me to focus on the query rather than incorrectly blaming data collection.

---

## 3. Time matters

Authentication events need to be examined as a timeline.

Several failures close together can mean something very different from failures spread across a long period.

---

## 4. Detection logic must match actual behavior

My original five-minute time buckets were technically valid KQL.

However, they did not represent the behavior I wanted to detect correctly.

This taught me that detection engineering requires more than writing code that runs without errors.

---

## 5. Context changes the final decision

Sentinel correctly detected repeated failed authentication attempts.

The investigation determined that those attempts were authorized testing.

Both statements can be true.

---

## 6. Good analysts separate facts from conclusions

The facts were:

* Failed authentication attempts occurred.
* They involved the lab account.
* They shared a source IP.
* They occurred close together.
* The detection threshold was reached.

The conclusion was:

* The activity was expected because it was intentionally generated for the lab.
* There was no evidence of unauthorized account compromise.

Keeping facts and conclusions separate makes an investigation easier to defend.

---

# MITRE ATT&CK Connection

Repeated password guessing behavior can relate to:

**T1110 — Brute Force**

This does not mean every failed login is automatically a brute-force attack.

MITRE ATT&CK provides a way to describe behaviors that could be associated with attacker techniques.

The actual security conclusion still depends on the evidence and context.

---

# Skills Demonstrated

This case study demonstrates experience with:

* Microsoft Sentinel
* Microsoft Defender
* Microsoft Entra ID
* Log Analytics
* KQL
* Authentication telemetry
* Identity investigation
* Failed login investigation
* SIEM monitoring
* Detection engineering
* Scheduled analytics rules
* Entity mapping
* Alert enrichment
* MITRE ATT&CK
* Alert triage
* Incident investigation
* Timeline reconstruction
* Evidence correlation
* Root cause analysis
* Detection troubleshooting
* Incident classification
* Incident resolution
* Security documentation

---

# Interview-Ready Explanation

> In this lab, I investigated repeated failed Microsoft Entra sign-ins using Microsoft Sentinel. I first made sure Entra sign-in telemetry was reaching my Log Analytics workspace. Then I used KQL against `SigninLogs` to investigate failed authentication events and analyze the account, source IP, timestamps, application, failure reason, and number of attempts.
>
> I created a scheduled analytics rule to automatically detect three or more failed sign-ins associated with the same account and source IP. During testing, the rule initially failed to trigger even though I could see the three events in the raw logs.
>
> I troubleshot the query and discovered that I was grouping events into fixed five-minute time buckets. My authentication attempts happened across a bucket boundary, so one event went into one group and two went into another. Neither group reached my threshold of three.
>
> I corrected the KQL, validated the new query, and successfully generated an alert and Microsoft Defender incident.
>
> I then investigated the incident and confirmed that the authentication failures were activity I intentionally generated during the lab. Because the detection was accurate but the activity was authorized, I resolved the incident as expected confirmed activity.
>
> The main lesson I learned was that a detection can be technically correct but still have logic that does not represent the behavior correctly. I also learned that an alert is not the final answer. The analyst has to use the evidence and context to determine what actually happened.

---

# Final Case Conclusion

The investigation successfully demonstrated the full lifecycle of an identity security detection.

Repeated Microsoft Entra authentication failures were generated, collected, queried, correlated, detected, alerted on, and investigated.

The initial detection logic failed because fixed time buckets divided the related authentication events. After troubleshooting the raw logs and KQL, the detection logic was corrected.

The updated rule successfully generated a security incident.

The final investigation determined that the activity was authorized lab testing and no evidence of unauthorized account compromise was identified.

The incident was resolved as expected confirmed activity.

**Case Status: Closed / Resolved**

**Final Result: Detection Successful — No Confirmed Security Compromise**
