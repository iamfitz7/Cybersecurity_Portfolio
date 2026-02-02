SOC Case File — Mission Control Alert Intake & Workflow Validation

Case ID: ES-00030
Case Title: Suspicious Process Activity — Initial Alert Intake & Workflow Handling
Date Observed: January 31, 2026
Platform: Splunk Enterprise Security (Mission Control)

Alert Summary:

A high-urgency alert triggered for suspicious process activity on an endpoint associated with potential LOLBAS behavior. The alert required analyst intake, ownership assignment, and workflow validation prior to any deep investigation.

This case focuses on SOC hygiene and alert handling, not incident confirmation.


Data Sources:

SIEM: Splunk Enterprise Security

Indexes: Endpoint-related data (correlation search output)

Detection Source: Correlation search

Log Context: Endpoint process activity related to PowerShell usage


Timeline of Analyst Actions:

Alert appeared in Mission Control with High urgency and New status

Alert initially unassigned

Alert reviewed in Analyst Queue

Ownership assigned to self

Status updated to In Progress

Detection name reviewed to understand trigger context

Correlation search located via Security Content

SPL copied into Search & Reporting

Logs reviewed at a high level to confirm alert context

No escalation or closure performed at this stage

Splunk Searches Used:
(index related to endpoint activity)
| <correlation search SPL copied from Security Content>


(Search used only to validate detection behavior and data presence — no deep investigation performed.)


Analyst Reasoning & Observations

Initial expectation was that the detection name would directly link to the rule, which it did not

Instead of forcing changes or assuming a misconfiguration, alert workflow was prioritized first

Ownership and status updates ensured the alert was tracked correctly

Locating the correlation search through Security Content clarified how Enterprise Security separates analyst workflow from detection management

Running the SPL confirmed that relevant endpoint activity existed, supporting the alert’s validity

This reinforced that alert intake discipline comes before investigation depth.


Key Signals Reviewed:

Alert urgency and timestamp

Correlation search logic (SPL)

Presence of endpoint process data during the alert window

These signals were sufficient to validate the alert without escalating prematurely.


Decision & Justification:

Decision:
Continue monitoring and document findings. Do not escalate or close at this stage.

Justification:
The alert was valid, but the lab objective was workflow handling and detection validation. No additional evidence was gathered to justify escalation or resolution.


Risks & Limitations:

Skipping ownership or status updates can cause alerts to be missed or worked multiple times

Jumping into deep investigation too early can waste analyst time

This case did not include containment, response, or enrichment by design


Common Beginner Mistake:

A frequent mistake is immediately diving into logs without assigning the alert or updating status. This breaks SOC metrics (MTTA/MTTR) and causes confusion across shifts. Proper workflow handling prevents this.


One Practical Improvement:

Improve UI linking between alerts and correlation searches so analysts can reach detection logic faster without relying on prior ES navigation knowledge.


Case Outcome:

Alert properly acknowledged and owned

Workflow discipline maintained

Detection logic validated

No escalation or closure performed

Case Status: Monitoring / Documented