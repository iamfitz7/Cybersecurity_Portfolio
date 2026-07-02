Incident Summary

High-severity Elastic Security alert indicating suspicious PowerShell download activity.

Initial Observation

Elastic detected PowerShell executing a download command using Invoke-WebRequest.

Investigation
Reviewed alert metadata.
Examined process tree.
Analyzed parent-child relationships.
Reviewed PowerShell command-line arguments.
Identified external download destination.
Validated URL reputation using VirusTotal.
Correlated findings with process timeline.
Findings
PowerShell downloaded test.bin.
URL: speed.hetzner.de
VirusTotal: 0 detections.
Process execution appeared consistent with testing activity.
Assessment

No evidence was found indicating confirmed malicious execution based on the available telemetry.

Recommendation

Document findings, retain evidence, continue monitoring, and gather additional endpoint context if similar alerts occur.