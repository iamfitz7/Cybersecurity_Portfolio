Incident Case Study File

SVCHOST Dump Creation Alert Investigation

Case Overview

This case study documents the investigation of an Elastic Security alert titled “SVCHOST Dump creation attempt on endpoints.” The alert initially appeared concerning because process dump files can sometimes be associated with suspicious activity, memory collection, or credential access techniques.

However, the purpose of this investigation was not to assume the alert title was correct. The goal was to validate what actually happened by reviewing the alert details, process information, file activity, process lineage, command-line arguments, and surrounding alert context.

After reviewing the available evidence, the activity was assessed as benign Windows cleanup behavior rather than malicious dump creation.

Final Classification: False Positive / Benign Activity

Confidence Level: High

Alert Summary

Alert Name: SVCHOST Dump creation attempt on endpoints

Severity: Medium

Risk Score: 47

Host: desktop-il6ukq9

User: DELL

Process: cleanmgr.exe

Process Path: C:\Windows\System32\cleanmgr.exe

File Name: svchost.DMP

File Path: C:\Users\DELL\AppData\Local\Temp\svchost.DMP

Observed Event Action: Deletion

Initial Investigation Question

The main question was:

Did someone or something create a dump of svchost.exe, or did Windows Disk Cleanup delete an existing dump file during normal cleanup activity?

This distinction mattered because the alert title suggested dump creation, but the underlying event details pointed toward deletion activity involving cleanmgr.exe.

A strong investigation needed to separate the alert name from the actual endpoint telemetry.

Investigation Process

The investigation began by reviewing the alert overview in Elastic Security. The alert was medium severity with a risk score of 47. The affected endpoint was desktop-il6ukq9, and the user associated with the activity was DELL.

The file involved was svchost.DMP, located in the user’s temporary directory. Since dump files can sometimes be connected to suspicious process memory collection, this file required careful review.

The next step was reviewing the process responsible for the file event. The process was cleanmgr.exe, located at C:\Windows\System32\cleanmgr.exe. This is the Windows Disk Cleanup utility. That finding immediately created a reasonable benign explanation, but it was not enough by itself to close the alert.

The investigation then focused on the actual event action. The evidence showed that the event action was deletion, not creation. This was one of the most important findings in the case. The alert title suggested dump creation, but the telemetry showed that cleanmgr.exe deleted svchost.DMP from the Temp directory.

After that, the process lineage was reviewed in Elastic Process Analyzer. The observed chain included svchost.exe leading to cleanmgr.exe and then DismHost.exe. This process relationship was consistent with Windows cleanup or maintenance behavior.

The cleanmgr.exe command-line arguments were also reviewed. The process ran with arguments showing Storage Sense cleanup behavior, including /autocleanstoragesense and /d C:. This strongly supported the conclusion that Windows cleanup activity was occurring on the C: drive.

A broader DMP-related alert search was also performed for the affected host. The search did not identify a wider pattern of matching dump-related alerts in the searched alert view. This helped support the conclusion that the activity was isolated rather than part of a broader suspicious pattern.

Key Evidence Collected

The strongest evidence was that the event action was deletion. The observed activity showed cleanmgr.exe deleting svchost.DMP, not creating it.

The second major piece of evidence was the process path. cleanmgr.exe was located in C:\Windows\System32, which is the expected location for the Windows Disk Cleanup utility.

The third major piece of evidence was the command line. The arguments /autocleanstoragesense and /d C: were consistent with automated Windows Storage Sense cleanup activity.

The fourth major piece of evidence was the process lineage. The chain involving svchost.exe, cleanmgr.exe, and DismHost.exe was consistent with Windows maintenance behavior.

The fifth major piece of evidence was the lack of supporting suspicious alert activity in the performed searches. No additional alert evidence was identified showing malicious dump tooling, PowerShell abuse, malware execution, credential access, or follow-on compromise.

Analysis

At first, the alert title made the event sound more suspicious than the underlying telemetry supported. A file named svchost.DMP can be worth investigating because dump files may contain memory data. However, the actual file action and process context changed the meaning of the alert.

The evidence showed that cleanmgr.exe deleted a dump file from the Temp directory during cleanup activity. The command-line arguments were consistent with Storage Sense cleanup, and the process lineage did not show suspicious dumping tools or unusual execution behavior.

This investigation reinforced an important SOC concept: the alert name is only a hypothesis. The analyst still has to validate the raw details before making a decision.

The best explanation for the available evidence was that Windows Disk Cleanup or Storage Sense removed an existing dump file from the user’s Temp directory.

Final Assessment

Classification: False Positive / Benign Activity

Confidence: High

The alert was generated because activity involving svchost.DMP matched detection logic. However, the underlying event showed deletion of the file by Microsoft Windows Disk Cleanup. The process path, command-line arguments, process lineage, and lack of corroborating suspicious alerts all supported benign cleanup activity.

There was not enough evidence to support malicious dump creation, credential access, malware execution, or broader endpoint compromise.

The alert should be documented and closed as benign activity.

Lessons Learned

This investigation showed why analysts should not rely only on alert titles. The phrase “dump creation attempt” sounded serious, but the actual telemetry showed deletion by cleanmgr.exe.

The investigation also showed the value of command-line analysis. The arguments /autocleanstoragesense and /d C: helped explain what cleanmgr.exe was doing.

Another important lesson was that benign findings still matter. Correctly identifying false positives prevents unnecessary escalation and helps improve alert triage quality.

The strongest conclusion is the one that best fits all available evidence, not the one that sounds most serious.