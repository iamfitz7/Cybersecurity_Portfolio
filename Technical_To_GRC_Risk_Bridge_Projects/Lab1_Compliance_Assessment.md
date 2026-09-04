# Week 20 Lab 1 — NIST SP 800-171 / CMMC Evidence and Control Assessment

## Project Overview

This project was a simulated NIST SP 800-171 and CMMC Level 2 control assessment performed inside my isolated cybersecurity home lab.

The purpose of the project was not to claim that my home lab is officially CMMC compliant. The purpose was to practice the process of taking security requirements, implementing technical controls, collecting evidence, reviewing that evidence, identifying gaps, and documenting remediation.

The lab focused on selected requirements from NIST SP 800-171 Revision 2 that are currently used as part of the CMMC Level 2 model.

NIST SP 800-171 Revision 3 is a newer NIST publication, but this project used Revision 2 requirements because the project was specifically designed around the current CMMC Level 2 assessment baseline.

This was a simulated training environment only. No real Controlled Unclassified Information (CUI), government data, customer data, or sensitive company information was used.

---

## Environment

The lab environment included the following systems and technologies:

- pfSense firewall/router
- Windows Server 2022 domain controller
- Active Directory Domain Services
- Windows DNS
- Group Policy
- Windows 11 Enterprise workstation
- Microsoft Defender Antivirus
- Microsoft Defender for Endpoint
- Windows Security Event Logs
- Oracle VirtualBox

The internal lab network used the following addressing:

pfSense LAN:
10.20.30.1

Domain Controller / DNS:
10.20.30.10

Windows 11 Enterprise Endpoint:
10.20.30.20

Domain:
homelab.local

The Windows 11 Enterprise endpoint was joined to the homelab.local Active Directory domain.

---

## Assessment Scope

### In Scope

The following systems and technologies were included in the assessment:

- pfSense
- DC01
- Windows 11 Enterprise MDE endpoint
- Active Directory
- Windows DNS
- Group Policy
- Windows Security Event Logs
- Microsoft Defender Antivirus
- Microsoft Defender for Endpoint

### Out of Scope

The following systems were not included in the assessment:

- Physical HP Spectre host computer
- Kali Linux VM
- Ubuntu VM
- Other unrelated virtual machines
- Personal home network
- Personal accounts and personal files

The physical HP Spectre was only used to store screenshots, documentation, evidence files, and the project portfolio.

---

## Assessment Methodology

For each control, I followed this process:

Requirement
→ Implementation
→ Evidence
→ Assessment
→ MET or NOT MET
→ Gap
→ Risk
→ Remediation
→ Owner
→ Status

This process helped me understand that simply having a security technology installed does not automatically mean that a security requirement has been satisfied.

For example, having Active Directory does not automatically prove that least privilege is being followed. I had to review the actual user accounts and group memberships.

The same idea applied to auditing. It was not enough to say that Windows creates logs. I configured auditing through Group Policy, forced the policy to the endpoint, verified the effective audit settings, generated user activity, and reviewed the resulting Security events.

---

## Selected Controls

The following ten NIST SP 800-171 / CMMC Level 2 requirements were included in this lab:

AC.L2-3.1.1 — Authorized Access

AC.L2-3.1.5 — Least Privilege

AU.L2-3.3.1 — System Auditing

AU.L2-3.3.2 — User Activity Traceability

IA.L2-3.5.1 — User and Device Identification

IA.L2-3.5.3 — Multifactor Authentication

SC.L2-3.13.1 — Boundary Protection

SI.L2-3.14.1 — Flaw Remediation

SI.L2-3.14.2 — Malicious Code Protection

SI.L2-3.14.6 — System and Network Monitoring

---

# Control Assessments

## AC.L2-3.1.1 — Authorized Access

### Requirement

Limit system access to authorized users, authorized processes acting on behalf of authorized users, and authorized devices.

### Implementation

Active Directory was used as the centralized identity system for the lab.

Two domain user accounts were created:

lab.user
lab.admin

The Windows 11 Enterprise endpoint was also joined to the homelab.local Active Directory domain and represented by a computer object in Active Directory.

The lab.user account was created as the standard user account.

The lab.admin account was created as the dedicated privileged administrative account.

### Evidence

Evidence included:

- Active Directory Users and Computers showing lab.user and lab.admin
- Active Directory computer object for the Windows 11 Enterprise workstation
- Windows domain membership evidence
- Successful domain authentication

### Assessment Result

MET — within the defined lab scope.

### Reason

The lab demonstrated centralized identification of authorized users and the authorized Windows workstation.

### Gap

No major gap identified within the limited lab scope.

### Status

Implemented

---

## AC.L2-3.1.5 — Least Privilege

### Requirement

Use the principle of least privilege and limit privileged functions to authorized accounts.

### Implementation

Separate standard and privileged domain identities were used.

lab.user was used as the standard user account.

lab.admin was used as the dedicated administrative identity.

The standard account was not intentionally given administrative privileges.

This allowed normal activity to be separated from administrative work.

### Evidence

Evidence included:

- lab.user group membership
- lab.admin privileged group membership
- Active Directory account configuration

### Assessment Result

MET — within the defined lab scope.

### Reason

The project demonstrated separation between a normal account and an administrative account.

### Gap

A larger real organization would require additional privileged access controls, formal access reviews, and stronger privileged account management.

### Risk

If privileged accounts are used for normal daily activity, credential theft or user mistakes could have a greater impact.

### Remediation

Continue separating normal and privileged identities and periodically review privileged group membership.

### Owner

Identity / Systems Administrator

### Status

Implemented

---

## AU.L2-3.3.1 — System Auditing

### Requirement

Create and retain system audit records that support monitoring, analysis, investigation, and reporting.

### Implementation

A domain Group Policy Object named CMMC-Audit-Policy was created and linked to the Windows workstation OU.

Advanced Windows auditing was configured for security-relevant activities including:

- Logon activity
- Logoff activity
- Account lockout activity
- Account management
- Policy changes
- Process creation
- System integrity events

The advanced audit policy override setting was enabled so the advanced settings would take precedence over older audit policy settings.

The policy was forced to the workstation using:

gpupdate /force

The effective audit policy was then verified using:

auditpol /get /category:*

### Evidence

Evidence included:

- CMMC-Audit-Policy Group Policy configuration
- Successful gpupdate output
- gpresult showing CMMC-Audit-Policy applied
- auditpol output showing active audit settings
- Windows Security Event Log entries

### Assessment Result

MET — within the defined lab scope.

### Reason

Audit policy was centrally configured and the effective endpoint configuration was verified.

### Gap

The lab does not use a centralized enterprise SIEM retention architecture for all audit logs.

### Risk

Local-only logs could be lost if the endpoint is damaged, compromised, or the log is overwritten.

### Remediation

A production environment should forward important security logs to a centralized protected logging platform or SIEM with an established retention policy.

### Owner

Security / Systems Administrator

### Status

Implemented in lab

---

## AU.L2-3.3.2 — User Activity Traceability

### Requirement

Make sure actions performed by individual users can be traced back to those users.

### Implementation

The lab.user domain account was used to create real authentication activity on the Windows 11 Enterprise workstation.

Windows Event Viewer was then used to review Security events.

Event ID 4624 was used to review successful authentication activity.

Event ID 4625 was used to review failed authentication activity.

Event ID 4688 was also reviewed for process creation activity.

### Evidence

Evidence included:

- Successful logon Event ID 4624
- Failed logon Event ID 4625
- Process creation Event ID 4688
- Domain account information
- Logon timestamps
- Computer information
- Logon identifiers

### Assessment Result

MET — within the defined lab scope.

### Reason

Security events could be associated with user identities and endpoint activity.

### Gap

The lab does not provide full enterprise-wide correlation across many endpoints.

### Risk

Without centralized correlation, investigators may need to manually review multiple systems during an incident.

### Remediation

Forward endpoint audit events to a centralized security monitoring platform in a larger environment.

### Owner

Security Operations / Systems Administrator

### Status

Implemented

---

## IA.L2-3.5.1 — Identification

### Requirement

Identify system users, processes acting on behalf of users, and devices before allowing access.

### Implementation

Active Directory provided centralized identities for users and the managed workstation.

The following user identities were created:

lab.user

lab.admin

The Windows 11 Enterprise workstation was joined to homelab.local and registered as a domain computer.

### Evidence

Evidence included:

- Active Directory user objects
- Active Directory computer object
- Windows domain membership
- Successful domain login as HOMELAB\lab.user

### Assessment Result

MET — within the defined lab scope.

### Reason

Users and the Windows workstation were represented by unique identities in Active Directory.

### Gap

No major gap identified for the limited identity test.

### Owner

Identity Administrator

### Status

Implemented

---

## IA.L2-3.5.3 — Multifactor Authentication

### Requirement

Use multifactor authentication for applicable privileged and network access scenarios.

### Implementation

The Active Directory lab currently uses username and password authentication for normal domain access.

A second authentication factor was not fully demonstrated for all applicable privileged and network authentication scenarios.

### Evidence

The tested domain authentication process used a username and password.

No complete second authentication factor was demonstrated for the required access scenarios.

### Assessment Result

NOT MET

### Gap

Required multifactor authentication coverage has not been fully implemented or demonstrated for applicable privileged and network authentication scenarios.

### Risk

If an attacker steals or guesses a valid password, the attacker may be able to authenticate without needing a second independent authentication factor.

### Remediation

Implement and validate an enterprise MFA solution that covers the required privileged and network authentication scenarios.

Possible technologies in a properly designed enterprise environment could include certificate-based authentication, smart cards, Windows Hello for Business, or another approved MFA solution.

### Owner

Identity / Systems Administrator

### Priority

High

### Status

Open

---

## SC.L2-3.13.1 — Boundary Protection

### Requirement

Monitor, control, and protect communications at external and important internal system boundaries.

### Implementation

pfSense was used as the boundary firewall and router for the isolated CMMC lab network.

The lab systems were placed behind the pfSense LAN interface.

The internal lab network was separated from the physical home network.

The Windows 11 Enterprise endpoint and domain controller communicated through the controlled virtual network.

### Evidence

Evidence included:

- pfSense WAN and LAN configuration
- pfSense LAN address 10.20.30.1
- pfSense firewall rules
- Internal CMMC lab network design
- Firewall log activity

### Assessment Result

MET — within the defined lab scope.

### Reason

A defined network boundary existed and traffic passed through the pfSense firewall/router.

### Gap

The home lab does not provide the same level of network segmentation, high availability, centralized firewall management, or enterprise monitoring expected in a production environment.

### Risk

Weak or incorrectly configured boundary rules could allow unnecessary traffic into or out of protected systems.

### Remediation

Regularly review firewall rules, remove unnecessary access, log important traffic, and document approved network paths.

### Owner

Network / Security Administrator

### Status

Implemented in lab

---

## SI.L2-3.14.1 — Flaw Remediation

### Requirement

Identify, report, and correct system flaws in a timely manner.

### Implementation

Microsoft Defender for Endpoint and Windows security tools were used to review the security condition of the Windows 11 Enterprise endpoint.

The lab included review of security recommendations, vulnerability information, software condition, and update status where available.

### Evidence

Evidence included available Microsoft Defender for Endpoint or Windows security information showing endpoint security status, vulnerability information, or security recommendations.

### Assessment Result

MET for the demonstrated lab process.

### Reason

The project demonstrated the process of identifying security weaknesses and reviewing remediation information.

### Gap

The home lab does not have a formal company-wide vulnerability remediation SLA or enterprise ticketing process.

### Risk

Known vulnerabilities that remain unpatched could be exploited by attackers.

### Remediation

In a production environment, vulnerability findings should be assigned severity, an owner, a remediation deadline, and validation after the fix is applied.

### Owner

Security / Systems Administrator

### Status

Implemented for lab demonstration

---

## SI.L2-3.14.2 — Malicious Code Protection

### Requirement

Provide protection from malicious code at appropriate locations within the environment.

### Implementation

Microsoft Defender Antivirus was enabled on the Windows 11 Enterprise endpoint.

PowerShell validation confirmed:

AntivirusEnabled = True

RealTimeProtectionEnabled = True

BehaviorMonitorEnabled = True

IoavProtectionEnabled = True

AntispywareEnabled = True

The Microsoft Defender for Endpoint Sense service was also verified as running.

The command used was:

sc.exe query sense

The service returned:

STATE : 4 RUNNING

### Evidence

Evidence included:

- Windows Security protection settings
- Microsoft Defender Antivirus PowerShell status
- Real-time protection status
- Behavior monitoring status
- Antispyware status
- Microsoft Defender for Endpoint Sense service running

### Assessment Result

MET — within the defined lab scope.

### Reason

The endpoint had active Microsoft anti-malware and endpoint security protection.

### Gap

This home lab does not represent full enterprise anti-malware deployment across a large number of endpoints.

### Risk

Endpoints without active anti-malware controls could allow malicious files or behavior to execute without detection.

### Remediation

Maintain Defender protection, current security intelligence, endpoint onboarding, tamper protection, and security monitoring.

### Owner

Security / Endpoint Administrator

### Status

Implemented

---

## SI.L2-3.14.6 — System and Network Monitoring

### Requirement

Monitor systems and network communications to detect attacks and indicators of potential attacks.

### Implementation

Multiple monitoring sources were used in the lab.

Windows Security Event Logs provided endpoint security events.

Microsoft Defender for Endpoint provided endpoint protection and monitoring capabilities.

pfSense provided network boundary logging and monitoring.

The combination provided both endpoint and network visibility.

### Evidence

Evidence included:

- Windows Security Event Viewer
- Event IDs 4624, 4625, and 4688
- Microsoft Defender for Endpoint service status
- Microsoft Defender security information
- pfSense firewall logs

### Assessment Result

MET — within the defined lab scope.

### Reason

The lab demonstrated both host-based and network-based monitoring capabilities.

### Gap

The environment does not have a full enterprise SOC or centralized correlation platform monitoring all sources continuously.

### Risk

Security events may be harder to correlate when evidence is stored across separate systems.

### Remediation

A production environment should forward logs and alerts into a centralized SIEM or security operations platform.

### Owner

Security Operations

### Status

Implemented for lab demonstration

---

# Overall Assessment Result

The project assessed ten selected NIST SP 800-171 / CMMC Level 2 requirements.

Nine controls were demonstrated sufficiently for the defined home-lab scope.

One control was identified as NOT MET:

IA.L2-3.5.3 — Multifactor Authentication

The MFA gap was intentionally documented instead of being incorrectly marked compliant.

This was important because the goal of the project was not to make every control appear successful. The goal was to assess the environment based on the evidence that actually existed.

---

# Major Finding

The most important open finding is incomplete MFA coverage.

The current lab uses username and password authentication for Active Directory domain access.

That means the environment does not fully demonstrate multifactor authentication for the required privileged and network authentication scenarios.

This weakness is documented in the project POA&M.

---

# Key Lessons Learned

One of the biggest things I learned from this project is that implementing a security technology is different from proving that a security requirement has been satisfied.

For example, Active Directory alone does not prove least privilege.

I had to review account roles and group memberships.

Windows Event Viewer alone does not prove accountability.

I had to configure audit policy, apply it to the endpoint, generate activity, and confirm that user actions produced useful audit events.

Microsoft Defender being installed does not automatically prove malicious-code protection.

I verified that the important protection features were enabled and confirmed that the Defender for Endpoint Sense service was running.

I also learned that a real assessment should not automatically mark every control as compliant.

The MFA control was marked NOT MET because the evidence did not demonstrate the required second authentication factor.

The correct response was to document the gap, risk, remediation, owner, and status.

---

# Final Project Statement

This project provided hands-on experience with NIST SP 800-171 / CMMC control mapping, Active Directory administration, Windows security configuration, Group Policy, audit policy, Windows event analysis, Microsoft Defender, Microsoft Defender for Endpoint, pfSense, evidence collection, gap analysis, remediation planning, and compliance documentation.

This was a simulated home-lab assessment and should not be interpreted as an official CMMC certification, official CMMC assessment, or implementation for a real defense contractor.

The project demonstrates my ability to understand a security requirement, implement technical controls, collect evidence, review that evidence, identify deficiencies, and document remediation in a structured way.