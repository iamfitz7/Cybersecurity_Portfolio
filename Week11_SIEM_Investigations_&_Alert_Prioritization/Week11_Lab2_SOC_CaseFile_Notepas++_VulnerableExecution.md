CASE FILE — Vulnerable Notepad++ Execution Review

Alert Summary

Multiple executions of vulnerable Notepad++ versions were observed across several Windows hosts. Given recent public disclosures involving compromised Notepad++ update infrastructure, the activity required validation to determine scope, execution context, and potential risk.

Data Sources

SIEM: Splunk Enterprise

Index: npp-sysmon

Log Source: Windows Sysmon (process creation, registry, execution context)

Timeline (Condensed)

Initial review confirmed Sysmon data availability and consistency

Multiple Notepad++ versions identified executing in the environment

Hosts were ranked by execution frequency to establish investigation priority

Total impacted host count was measured to determine blast radius

Network activity related to Notepad++ domains was checked

Parent process and execution directories were reviewed

Findings were evaluated for signs of abnormal or malicious behavior

Key Splunk Searches
index="npp-sysmon" Image="*notepad++*" | stats count by Image, FileVersion

index="npp-sysmon" Image="*notepad++*" | stats count by Computer | sort -count

index="npp-sysmon" Image="*notepad++*" | stats dc(Computer) as Total_Impacted_Hosts

index="npp-sysmon" Image="*notepad++*" | stats count by ParentImage | sort -count

index="npp-sysmon" Image="*notepad++*" | stats count by CurrentDirectory | sort -count

Findings

Three vulnerable Notepad++ versions were observed executing

Five hosts were impacted in total

Execution frequency was highest on a single host, indicating prioritization

Parent processes were consistent with normal user behavior

Execution directories were primarily trusted system paths

No Notepad++-related network connections were observable in available telemetry

MITRE ATT&CK (High-Level)

T1204 – User Execution

T1105 – Ingress Tool Transfer (contextual relevance only, not observed)

Decision

Closed with justification.
Observed activity aligned with expected user-driven execution and showed no indicators of malicious process lineage or suspicious execution context. Patch awareness and version consistency were recommended to reduce exposure.

Next Actions (If Production)

Validate Notepad++ versions against current security guidance

Encourage manual updates to secure versions

Continue monitoring for abnormal update-related behavior