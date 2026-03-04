Week 13 - Lab 1: Windows Event Log Analysis Write up:

1. Context: What Area This Touches & Why It Matters

Windows systems generate thousands of internal records that describe what the operating system and applications are doing. These records are stored as event logs and are commonly used by system administrators and security teams to understand system behavior.

The three main logs — Application, Security, and System — provide visibility into different types of activity. Application logs show software behavior, Security logs record authentication and access activity, and System logs track operating system events.

In real environments, these logs are one of the first places analysts check when something unexpected happens on a system.

2. Realistic Scenario: When This Knowledge Is Actually Used

Imagine a situation where a workstation begins behaving differently than normal. A user reports that they were unexpectedly logged out, or an alert indicates unusual login activity.

At that point, the question is not immediately “Was this an attack?” but rather:

What actually happened on this system?

Before jumping to conclusions, an analyst would begin by reviewing the system’s event logs to see whether anything unusual occurred — failed logins, service errors, application crashes, or system warnings.

This kind of investigation usually begins with basic log visibility and filtering, which helps separate important signals from thousands of routine system messages.

3. Thinking Process: How I Approached the Problem

When I started reviewing the system, my first thought was that Windows probably records far more activity than most people realize. My goal was to understand what kinds of information these logs actually contain and how they are organized.

The first thing I checked was the Windows Logs section in Event Viewer, which contains the Application, Security, and System logs. I expected these categories to show different types of activity depending on what part of the system generated the event.

At first, the amount of data was overwhelming. Each log contained thousands of entries, and most of them looked similar at first glance. That made it difficult to immediately tell which events were important and which were just normal background activity.

Because of that, I decided to narrow the view by filtering the logs. Instead of looking at everything, I filtered for critical, error, and warning events. This helped reduce the noise and made it easier to identify entries that might require attention.

When reviewing the Security logs, I also filtered for common authentication-related event IDs such as 4624, 4625, 4634, and 4672, which are commonly associated with login activity.

This process helped me better understand how Windows records authentication events and system behavior over time.

4. Signals That Actually Mattered (Evidence Over Noise)

While reviewing the logs, two types of signals stood out as particularly useful.

Authentication Events (Event ID 4624)

One of the most informative entries was Event ID 4624, which represents a successful logon event.

These entries showed details such as:

Account name

Logon ID

Time of authentication

Authentication source

Even though these events are common and expected, they are extremely valuable during investigations because they show who accessed the system and when.

This type of information becomes critical if an analyst is trying to confirm whether a login was legitimate or suspicious.

System Warnings and Errors

In the System log, several warning events appeared related to system services and disk operations.

Examples included warnings from services such as:

time synchronization

storage or disk operations

system services

While many of these warnings are not security incidents, they can indicate operational issues that might affect system performance or stability.

The key takeaway was that not every warning or error represents a security problem, but they still provide useful operational context.

5. Decision: What Action Made Sense Based on the Evidence

Based on the evidence observed in the logs, there was no clear indication of malicious activity.

The authentication events appeared normal and matched expected system behavior. The system warnings were also consistent with routine system messages that can occur during normal operation.

Because of that, the appropriate action was to:

document the findings

confirm that the observed events were expected

continue monitoring the logs for unusual patterns

If this were a production system, the next step would likely involve adding log monitoring or alerting rules so that suspicious authentication patterns could be detected automatically.

6. Risks, Trade-Offs, and Limitations

One challenge with Windows event logs is the sheer volume of data they generate.

A typical system can record thousands of events per day, which makes it difficult to manually review everything.

There is also a trade-off between visibility and noise. Enabling detailed logging improves visibility but can create large amounts of data that analysts must filter through.

Another limitation is that event logs alone may not provide full context. In real investigations, analysts often combine system logs with other sources such as:

SIEM platforms

endpoint detection tools

network logs

This lab focused mainly on understanding where the logs are and what they contain, rather than performing full threat detection.

7. Common Beginner Mistake

A common mistake beginners make when reviewing logs is assuming that every warning or error indicates a security problem.

In reality, operating systems generate many warnings that are simply part of normal system behavior.

If someone treats every warning as an incident, it can lead to wasted investigation time and alert fatigue.

Learning to recognize which events are routine and which ones require investigation is an important skill when working with system logs.

8. One Practical Improvement

One useful improvement for systems like this would be basic authentication monitoring.

For example, monitoring for patterns such as:

repeated failed login attempts

unusual login times

administrative logins from unexpected accounts

Even simple alerting rules around these patterns can significantly improve early detection of suspicious activity.

9. Summary

This analysis focused on reviewing Windows event logs to understand how operating system activity is recorded and monitored. By examining the Application, Security, and System logs, it became clear how authentication events, application behavior, and system activity are tracked within Windows.

Filtering logs helped reduce noise and made it easier to identify meaningful events such as successful authentication records and system warnings.

Understanding how to navigate and interpret these logs is an important foundation for troubleshooting system issues and investigating potential security events.