Week 17 Lab #1 — Professional Cybersecurity Technical Analysis

## 1. Context: What Area This Touches & Why It Matters

Modern security monitoring depends on several systems working together. An endpoint must collect activity, a cloud security platform must receive and analyze that activity, and analysts need a place to review alerts, incidents, and supporting evidence.

This project focused on Microsoft Entra ID, Azure, Log Analytics, Microsoft Sentinel, Microsoft Defender for Endpoint, and Microsoft Defender XDR. These services form an important part of Microsoft’s security ecosystem.

A monitoring platform can appear ready while still failing to collect useful data. Licensing, user permissions, operating system support, endpoint services, cloud connectivity, and workspace configuration all affect whether security telemetry reaches the analyst.

## 2. Realistic Scenario: When This Knowledge Is Useful

A security team is preparing a Windows workstation for endpoint monitoring. The cloud tenant and security portal are available, but the device does not appear in the device inventory after the onboarding package is executed.

The onboarding script reports that the Microsoft Defender for Endpoint service cannot start. The service name is invalid, and the expected SENSE service is missing.

At this point, several explanations are possible:

- The onboarding package may be incorrect.
- The script may not have administrative permissions.
- A required Windows service may be missing.
- The endpoint may be using an unsupported Windows edition.
- The license or tenant may not be fully provisioned.
- The endpoint may not be able to communicate with Microsoft’s cloud services.

The issue cannot be solved responsibly by repeatedly running the same script. The environment must be checked to identify which requirement is not being met.

## 3. Thinking Process: How I Approached the Problem

I expected the Windows endpoint to complete the onboarding process, start the Microsoft Defender for Endpoint sensor, and appear in the Defender device inventory.

Before troubleshooting the endpoint, I first built the supporting cloud environment. This included creating an Azure subscription, a dedicated resource group, a Log Analytics workspace, and enabling Microsoft Sentinel. I also created a Microsoft Entra organizational account because the original personal Microsoft account could not access all Defender features.

The licensing process also required validation. Microsoft Defender for Endpoint P2 was activated and assigned to the organizational administrator account. This gave the tenant access to endpoint onboarding and investigation features.

The first major technical problem appeared when I attempted to onboard my original Windows 11 VM. The script returned Error ID 15 and stated that the Microsoft Defender for Endpoint service could not be started because the service name was invalid.

Instead of assuming the script itself was broken, I checked whether the SENSE service existed. The service query returned Error 1060, confirming that the service was not installed.

I then checked the Windows edition. The system reported `Current Edition: Core`, which means Windows 11 Home. This changed my understanding of the problem. The issue was not mainly a bad command or missing permission. The endpoint itself was using an unsupported Windows edition for this Defender for Endpoint configuration.

I considered whether to force the service onto the existing system, but that would not have been a reliable solution. I kept the original VM and created a separate Windows 11 Enterprise Evaluation VM instead.

The Enterprise VM was configured with appropriate processors, memory, storage, and NAT internet access. I verified that the operating system was Enterprise Evaluation and confirmed that the SENSE service existed.

After running the onboarding package on the new endpoint, the script reported successful onboarding. I then verified that both the SENSE service and Microsoft Defender Antivirus service were running. The device also appeared in Microsoft Defender’s device inventory.

The next challenge involved the official EDR detection test. The test expected a file to be served locally through TCP port 80. The first attempt failed because no local web service was listening on that port.

I enabled only the IIS components required for the controlled test, created the local test file, and confirmed that the World Wide Web Publishing Service was running. I tested the port and received a successful TCP result. I also confirmed that the local web server returned an HTTP 200 response for the hosted file.

The official detection command did not provide clear output because it suppressed errors and launched a separate PowerShell process. The temporary `invoice.exe` file was not present afterward, which initially created uncertainty.

Instead of treating the missing file as proof of failure, I checked the actual monitoring platform. Microsoft Defender later marked the first device detection test as completed. It also created two EDR alerts and grouped them into a medium-severity incident called `Execution incident on one endpoint`.

This confirmed that the important result was not whether the temporary file remained on disk. The important result was whether the endpoint sensor observed the behavior and reported it to Defender.

## 4. What Actually Mattered: Signal vs. Noise

The most important troubleshooting signal was the combination of these results:

- The SENSE service did not exist.
- The Windows edition was reported as Core.

Those two observations explained why the original onboarding attempt failed. Repeatedly downloading the package, restarting the script, or changing unrelated VirtualBox settings would not have corrected an unsupported endpoint edition.

The second important signal was the completed detection status in Microsoft Defender. The temporary `invoice.exe` file was missing, but Defender still created the expected incident and recorded the activity.

Advanced Hunting returned seven related process events. The results included `powershell.exe`, `invoice.exe`, and the localhost download activity. This proved that endpoint telemetry had reached the security platform even though the local file was no longer present.

The alert details also showed:

- Alert title: Suspicious PowerShell command line
- Severity: Medium
- Detection source: EDR
- Service source: Microsoft Defender for Endpoint
- Affected Windows 11 device
- Associated Microsoft Entra user
- Related process activity

These details were stronger evidence than the behavior of a temporary test file.

## 5. Decision: What Made Sense Based on the Information

The reasonable decision was to stop attempting to onboard the unsupported Windows 11 Home VM and create a supported Windows 11 Enterprise endpoint.

This decision avoided forcing an unsupported configuration and gave the environment a more reliable foundation for future Defender and Sentinel work.

After the controlled test created the expected alerts, I reviewed the incident, process tree, affected device, user identity, detection source, and Advanced Hunting results. The activity matched the official Microsoft Defender detection test.

I resolved the incident as:

- Status: Resolved
- Classification: Informational, expected activity
- Determination: Security testing

I documented that the incident was generated intentionally, the endpoint reported correctly, and no real compromise occurred.

After collecting the required evidence, I removed the temporary IIS test file and folder, stopped the web service, disabled the IIS web-server role, and confirmed that TCP port 80 was closed again.

In a real environment, I would follow the same general decision process:

1. Confirm the endpoint and identity involved.
2. Review the alert evidence and process activity.
3. Determine whether the behavior was authorized.
4. Record the reasoning behind the classification.
5. Remove temporary testing configurations.
6. Continue monitoring the device for unrelated activity.

## 6. Risks, Trade-Offs, and Limitations

Microsoft security services depend on licensing, cloud subscriptions, permissions, internet access, supported operating systems, and correct tenant configuration. A problem in any one of these areas can stop telemetry from reaching the security platform.

Using a highly privileged administrator account made the initial environment setup easier, but it would not be appropriate for daily analyst work. A production environment should use separate accounts and least-privilege roles.

The Microsoft Defender for Endpoint trial also has a limited duration. Future work depends on keeping the required licensing active or moving to another authorized environment.

The official detection test was controlled and harmless. It demonstrated collection and alert generation, but it did not fully represent a real attacker. A real incident could involve persistence, credential access, lateral movement, data theft, or attempts to disable security tools.

The environment used one Windows endpoint. A business environment may contain thousands of devices, multiple operating systems, remote users, network controls, and different data-retention requirements.

The temporary IIS service was required only for the detection test. Leaving it enabled after the test would have created an unnecessary service and listening port. This is why cleanup was part of the project.

## 7. Common Beginner Mistake

A common beginner mistake is repeatedly running an onboarding script without checking whether the endpoint meets the platform requirements.

The original error could have been mistaken for a permission issue or a bad download. Repeating the same action would not have installed a supported Windows edition.

A better approach is to stop after a repeatable error and validate the environment in a logical order:

- Confirm the operating system edition.
- Check whether the required service exists.
- Verify administrative access.
- Confirm licensing.
- Confirm internet connectivity.
- Review the exact error instead of guessing.

Another beginner mistake is assuming that a missing test file means the detection failed. Security software may block, remove, or quarantine a file while still reporting the behavior successfully. The security portal, alert evidence, and endpoint telemetry provide a more reliable answer.

## 8. One Practical Improvement

A practical improvement would be to create a written pre-onboarding checklist for every future endpoint.

The checklist should include:

- Supported operating system and edition
- Current Windows updates
- Active Defender license
- Correct Microsoft Entra tenant
- Required user permissions
- Internet connectivity
- Presence of the SENSE service
- Deployment method
- Expected device name
- Cleanup requirements for any temporary test services

This would reduce troubleshooting time and make the setup easier to repeat.

A separate low-privilege analyst account should also be created for normal investigation work. The highly privileged administrator account should be reserved for configuration changes that truly require it.

## 9. Professional Summary

This project gave me practical experience building and validating a Microsoft security monitoring environment. I learned that successful endpoint monitoring depends on more than running an onboarding package because licensing, identity, operating system support, services, and cloud configuration must all work together. I identified an unsupported Windows edition, replaced it with a supported Enterprise endpoint, validated telemetry through Defender alerts and Advanced Hunting, and resolved the controlled incident with a clear explanation. I also removed the temporary web-server configuration after testing, which reinforced the importance of returning systems to a secure state.