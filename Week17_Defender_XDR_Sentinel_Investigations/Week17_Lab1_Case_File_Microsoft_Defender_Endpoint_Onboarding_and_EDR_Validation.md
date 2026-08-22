Week 17 Lab #1: Case File: Microsoft Defender Endpoint Onboarding and EDR Validation

## Case Information

**Case title:** Microsoft Defender Endpoint Onboarding and EDR Validation  
**Environment:** Microsoft Azure, Microsoft Entra ID, Microsoft Sentinel, Log Analytics, Microsoft Defender XDR, Microsoft Defender for Endpoint, Oracle VirtualBox  
**Primary endpoint:** Windows 11 Enterprise Evaluation VM  
**Affected device:** desktop-3sepq1q  
**Alert title:** Suspicious PowerShell command line  
**Incident title:** Execution incident on one endpoint  
**Severity:** Medium  
**Detection source:** EDR  
**Service source:** Microsoft Defender for Endpoint  
**Incident status:** Resolved  
**Final classification:** Informational, expected activity — Security testing  

---

## 1. Context: What Area This Touches & Why It Matters

This case involved endpoint security monitoring and cloud-based incident detection. The main concern was whether a Windows endpoint could be properly onboarded into Microsoft Defender for Endpoint and send usable telemetry into Microsoft Defender XDR and the connected Sentinel workspace.

Reliable endpoint visibility matters because an analyst cannot investigate activity that was never collected. A device may appear normal locally while its sensor, license, identity, or cloud connection is not functioning correctly.

---

## 2. Realistic Scenario: When This Knowledge Is Useful

An organization is deploying Microsoft Defender for Endpoint to a newly prepared Windows workstation.

The administrator runs the onboarding package, but the process fails. The expected endpoint service is missing, and the workstation does not appear in the Defender device inventory.

After a replacement endpoint is onboarded, the team needs to confirm that it can detect suspicious activity, report it to the cloud, create an alert, and preserve enough evidence for an analyst to investigate.

The main questions are:

- Why did the original endpoint fail to onboard?
- Is the replacement endpoint supported?
- Are the Defender services running?
- Is the device reporting to Microsoft Defender?
- Can controlled suspicious behavior create an alert?
- Does the platform preserve useful process and command-line evidence?
- Can the incident be reasonably classified and closed?

---

## 3. Thinking Process: How I Approached the Problem

The original Windows 11 VM failed during Defender onboarding. The script returned Error ID 15 and reported that the Microsoft Defender for Endpoint service could not start because the service name was invalid.

I checked the SENSE service instead of immediately running the script again. The service query returned Error 1060, meaning the service did not exist.

I then checked the operating system edition. The result was:

`Current Edition: Core`

This identified the system as Windows 11 Home. The combination of the unsupported edition and missing SENSE service explained the failed onboarding attempt.

I decided not to force the original VM into an unsupported configuration. A separate Windows 11 Enterprise Evaluation VM was created for the Defender environment.

Before onboarding the new endpoint, I verified:

- The operating system was Windows 11 Enterprise Evaluation.
- The VM had internet access through NAT.
- The organizational Microsoft Entra account could sign in.
- The Defender for Endpoint P2 license was active.
- The SENSE service existed.
- Windows updates and Defender components were available.

The local onboarding package was then downloaded and executed with administrative privileges. The onboarding script reported success.

I verified that:

- The SENSE service was running.
- Microsoft Defender Antivirus was running.
- The onboarding registry state was `0x1`.
- The endpoint appeared in Microsoft Defender’s device inventory.

To validate detection, I prepared the environment for Microsoft’s official EDR test. The test depended on a local web service listening on TCP port 80.

The first connectivity check failed because no web server was active. I enabled the required IIS components, created the local test file, started the World Wide Web Publishing Service, and confirmed:

- W3SVC was running.
- TCP port 80 was reachable locally.
- The hosted file existed.
- The local web request returned HTTP 200 OK.

I ran the official detection command once. The temporary file was not present afterward, so I did not assume the test had failed. I checked Microsoft Defender for the actual result.

The portal later showed:

`First device detection test: Completed`

A new medium-severity incident also appeared. It contained two EDR alerts involving the same endpoint and user.

---

## 4. What Actually Mattered: Signal vs. Noise

### Meaningful signal 1: Unsupported operating system

The original endpoint produced two important findings:

- The SENSE service was missing.
- The Windows edition was Core, meaning Windows 11 Home.

These findings explained the onboarding failure. Other possibilities, such as VirtualBox networking or a damaged ZIP file, were less important after the unsupported edition was confirmed.

### Meaningful signal 2: Cloud detection and telemetry

The strongest success indicators were not local files. They were the results inside Microsoft Defender:

- Detection test status changed to Completed.
- The device appeared in inventory.
- Two EDR alerts were created.
- The alerts were grouped into one incident.
- The alert identified suspicious PowerShell activity.
- The process tree showed the execution path.
- Advanced Hunting returned seven related events.
- The events included PowerShell, `invoice.exe`, and localhost activity.

This evidence confirmed that the endpoint sensor observed and reported the controlled behavior.

---

## 5. Decision: What Made Sense Based on the Information

The original Windows 11 Home endpoint was not used for Defender onboarding because it did not meet the operating system requirement.

The Windows 11 Enterprise endpoint was accepted as the correct monitoring target after the operating system, SENSE service, onboarding state, running services, and device inventory were verified.

The resulting incident was reviewed as an authorized detection test. Its activity matched the Microsoft-provided command and occurred at the expected time on the expected device.

The incident was resolved with the following decision:

- **Status:** Resolved
- **Classification:** Informational, expected activity
- **Determination:** Security testing

The resolution explanation stated that the incident was created intentionally using the official Defender for Endpoint detection test, the endpoint reported successfully, the resulting alerts were reviewed, and no real compromise occurred.

This decision preserved the fact that the detection was valid while documenting that the activity was authorized.

---

## 6. Risks, Trade-Offs, and Limitations

The original onboarding failure showed that endpoint compatibility must be verified before deployment. Attempting to bypass platform requirements could create an unstable or unsupported security configuration.

The setup used a privileged administrator account. This was reasonable for the initial configuration, but daily investigation work should use lower-privilege accounts.

The controlled detection used PowerShell, localhost traffic, and a harmless executable. It tested the monitoring pipeline, but it did not represent the full complexity of a real attack.

The incident contained two alerts because the test activity occurred more than once during troubleshooting. In a real environment, repeated testing can create extra alerts and make the incident timeline harder to interpret.

IIS introduced a temporary listening service on TCP port 80. Keeping it enabled after testing would have added unnecessary exposure. The service and test files were therefore removed after evidence was collected.

The environment also depended on trial licensing and an Enterprise evaluation operating system. These are useful for training but are not permanent production solutions.

---

## 7. Common Beginner Mistake

A common beginner mistake is treating the alert name as the final answer without reviewing the surrounding evidence.

The alert was named `Suspicious PowerShell command line`, but PowerShell is not automatically malicious. Administrators, software installers, support tools, and security tests can all use PowerShell.

The proper conclusion came from context:

- The command matched the approved detection test.
- It ran on the expected endpoint.
- It occurred during the documented testing period.
- The involved account was the lab administrator.
- The process and command-line evidence matched the planned activity.
- There were no signs of an unknown attacker or unrelated follow-on behavior.

Without this context, the incident could have been incorrectly treated as a real compromise.

Another beginner mistake is closing the incident as a false positive. The alert was not technically false. Defender correctly detected suspicious behavior. The correct classification was expected security testing.

---

## 8. One Practical Improvement

A practical improvement would be to document all approved security tests before running them.

The record should include:

- Person performing the test
- Device name
- User account
- Expected start time
- Expected end time
- Commands or tools being used
- Expected alerts
- Cleanup actions
- Person authorized to resolve the incident

This would make it easier for an analyst to distinguish approved testing from real suspicious activity.

Another improvement would be to use a dedicated test account rather than the primary administrator identity. This would reduce privilege exposure and make security-test activity easier to identify.

---

## 9. Professional Summary

This case confirmed that the Microsoft endpoint monitoring pipeline was working from the Windows device through Microsoft Defender’s cloud investigation tools. I identified that the original onboarding failure was caused by an unsupported Windows edition, created a supported Enterprise endpoint, and verified its services, onboarding state, and device visibility. The controlled PowerShell test produced a medium-severity EDR incident with supporting process and command-line telemetry, which I reviewed through the alert story and Advanced Hunting. The incident was classified as expected security testing, resolved with documentation, and the temporary IIS test configuration was removed.

---

# Technical Findings

## Original Endpoint Finding

**Observed condition:** Defender onboarding failed with Error ID 15.

**Supporting evidence:**

- Microsoft Defender for Endpoint service could not start.
- The service name was invalid.
- `sc query sense` returned Error 1060.
- Windows reported `Current Edition: Core`.
- The operating system was Windows 11 Home.

**Conclusion:** The original endpoint did not meet the operating system requirement for the planned Defender for Endpoint configuration.

**Action taken:** The endpoint was not forced into an unsupported state. A Windows 11 Enterprise Evaluation VM was created.

---

## Enterprise Endpoint Validation

The replacement endpoint was checked before and after onboarding.

**Verified conditions:**

- Windows 11 Enterprise Evaluation installed
- VirtualBox NAT enabled
- Internet connectivity available
- Microsoft Entra organizational account used
- Defender for Endpoint P2 license active
- SENSE service present
- Onboarding package executed successfully
- SENSE service running
- WinDefend service running
- Onboarding state equal to `0x1`
- Device visible in Microsoft Defender inventory

**Conclusion:** The Enterprise endpoint was properly onboarded and reporting to Microsoft Defender.

---

## Detection Test Findings

**Controlled activity:** Official Microsoft Defender for Endpoint EDR detection test

**Local preparation:**

- IIS web-server components temporarily enabled
- W3SVC started
- TCP port 80 confirmed open
- Local file hosted through IIS
- HTTP response confirmed as `200 OK`
- Content type returned as `application/octet-stream`

**Detection outcome:**

- First device detection test marked Completed
- Incident generated: `Execution incident on one endpoint`
- Severity: Medium
- Two related EDR alerts
- Primary alert: `Suspicious PowerShell command line`
- Affected device: `desktop-3sepq1q`
- Associated user: Microsoft Entra lab administrator
- Detection source: EDR
- Service source: Microsoft Defender for Endpoint

---

## Process and Telemetry Findings

The alert story included a process chain beginning with normal user-session processes and continuing into command and PowerShell activity.

Observed process context included:

- `userinit.exe`
- `explorer.exe`
- `cmd.exe`
- `powershell.exe`
- `invoice.exe`

Advanced Hunting returned seven related process events. The results connected the endpoint, user, PowerShell activity, test executable, and localhost download behavior.

**Conclusion:** The alert was supported by endpoint telemetry and was not only a portal notification.

---

## Incident Disposition

**Final status:** Resolved  
**Classification:** Informational, expected activity  
**Determination:** Security testing  

**Resolution note:**

This incident was generated intentionally using the official Microsoft Defender for Endpoint detection test. The Windows 11 Enterprise endpoint was successfully onboarded, the test telemetry was reviewed, and the resulting alerts were validated. No real compromise occurred.

---

## Cleanup Verification

After the investigation was completed:

- W3SVC was stopped.
- The temporary hosted file was removed.
- The test directory was removed.
- IIS Web Server Role was disabled.
- TCP port 80 returned `TcpTestSucceeded: False`.
- `C:\inetpub\wwwroot\1.exe` returned `False`.
- W3SVC showed `Stopped`.
- Windows reported that a restart was required to finish applying the feature change.

**Conclusion:** The temporary detection-test infrastructure was removed, and the endpoint was returned to a safer state.

---

# Final Case Conclusion

The Microsoft security environment functioned as intended. The supported Windows 11 Enterprise endpoint successfully sent telemetry to Microsoft Defender for Endpoint, Defender generated and correlated the expected EDR alerts, and the related activity was available for investigation through the alert story and Advanced Hunting.

The incident was not a real compromise. It was a valid security detection caused by authorized testing. The case was appropriately documented, classified, resolved, and followed by cleanup of the temporary test services.