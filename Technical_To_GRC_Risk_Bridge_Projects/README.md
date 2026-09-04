# NIST SP 800-171 / CMMC Evidence & Control Assessment Lab

## Technical To GRC Risk Bridge Projects

### Technical Security Controls | Evidence Collection | Risk Assessment | Compliance Documentation | Remediation Tracking

---

## Project Overview

This project is a hands-on security control assessment built around selected **NIST SP 800-171 requirements mapped to CMMC Level 2**.

The goal was not simply to read a security framework or create a checklist.

I built a working virtual environment, configured security controls, collected technical evidence, tested those controls, reviewed the results, identified security gaps, and documented remediation actions.

The project connects technical cybersecurity work with security governance and risk by following this process:

**Requirement → Implementation → Evidence → Assessment → Gap → Risk → Remediation → Owner → Status**

The environment included:

- pfSense firewall and router
- Windows Server 2022
- Active Directory Domain Services
- Windows DNS
- Group Policy
- Windows 11 Enterprise
- Microsoft Defender Antivirus
- Microsoft Defender for Endpoint
- Windows Security Event Logs
- PowerShell
- Oracle VirtualBox

A total of **10 selected security requirements** were assessed.

### Final Assessment Results

| Result | Total |
|---|---:|
| Controls Assessed | 10 |
| MET | 9 |
| NOT MET | 1 |
| Open Findings | 1 |

The open finding involved **multifactor authentication (MFA)**.

Instead of marking every requirement as successful, I documented the MFA requirement as **NOT MET** because the evidence did not demonstrate a complete second authentication factor for the applicable authentication scenarios.

This project was completed in an isolated home lab for training purposes. It is **not an official CMMC assessment or certification**, and no real Controlled Unclassified Information (CUI) was used.

---

# Table of Contents

1. [Project Goals](#project-goals)
2. [What This Project Demonstrates](#what-this-project-demonstrates)
3. [Lab Architecture](#lab-architecture)
4. [Environment and Technologies](#environment-and-technologies)
5. [Assessment Method](#assessment-method)
6. [Security Requirements Assessed](#security-requirements-assessed)
7. [Active Directory and Identity Management](#active-directory-and-identity-management)
8. [Least Privilege and Privileged Account Separation](#least-privilege-and-privileged-account-separation)
9. [Managed Device Identification](#managed-device-identification)
10. [Group Policy and Security Auditing](#group-policy-and-security-auditing)
11. [Windows Security Event Analysis](#windows-security-event-analysis)
12. [Microsoft Defender Validation](#microsoft-defender-validation)
13. [Microsoft Defender for Endpoint Validation](#microsoft-defender-for-endpoint-validation)
14. [Network Boundary Protection](#network-boundary-protection)
15. [System and Network Monitoring](#system-and-network-monitoring)
16. [Flaw Remediation](#flaw-remediation)
17. [Major Security Finding](#major-security-finding)
18. [Risk Analysis](#risk-analysis)
19. [Remediation Plan](#remediation-plan)
20. [POA&M](#poam)
21. [Assessment Results](#assessment-results)
22. [Evidence Collection](#evidence-collection)
23. [Troubleshooting](#troubleshooting)
24. [Project Deliverables](#project-deliverables)
25. [Skills Demonstrated](#skills-demonstrated)
26. [Key Lessons Learned](#key-lessons-learned)
27. [Project Limitations](#project-limitations)
28. [Final Conclusion](#final-conclusion)

---

# Project Goals

The main goal of this project was to understand how security requirements connect to real technical systems.

Instead of only asking:

> "Is this security technology installed?"

I wanted to answer:

> "Can I prove through evidence that the security control is actually implemented and working?"

The project focused on learning how to:

- Understand a security requirement
- Identify the technical controls related to that requirement
- Configure those controls
- Collect evidence
- Test the implementation
- Review Windows and network security information
- Decide whether the available evidence supports the requirement
- Identify missing security controls
- Explain the risk created by a gap
- Recommend remediation
- Assign responsibility
- Track unresolved findings
- Create clear security documentation

Another major goal was learning not to automatically mark every security requirement as successful.

If the evidence did not support the requirement, the correct response was to document the weakness.

---

# What This Project Demonstrates

This project combines several areas of cybersecurity into one environment.

It demonstrates practical experience with:

### Identity Security

- Active Directory
- Domain authentication
- User account management
- Computer account management
- Administrative account separation
- Least privilege
- Identity verification

### Endpoint Security

- Windows 11 Enterprise
- Microsoft Defender Antivirus
- Microsoft Defender for Endpoint
- Endpoint security validation
- Security configuration review

### Windows Security

- Group Policy
- Advanced Audit Policy
- Windows Event Viewer
- Authentication logs
- Process creation auditing
- Security event analysis

### Network Security

- pfSense
- Network segmentation
- Firewall rules
- Firewall logs
- Boundary protection
- Internal network design

### Security Monitoring

- Windows Security Logs
- Authentication monitoring
- Process monitoring
- Endpoint security monitoring
- Firewall monitoring

### Risk and Compliance

- NIST SP 800-171 control mapping
- CMMC-related assessment concepts
- Evidence collection
- Control testing
- Gap analysis
- Risk documentation
- Remediation planning
- POA&M tracking

### Technical Documentation

- Assessment reporting
- Evidence tracking
- Security finding documentation
- Remediation documentation
- Technical analysis
- Case study development

---

# Lab Architecture

The environment was built inside **Oracle VirtualBox**.

The main architecture was:

```text
                         INTERNET
                             |
                             |
                       VirtualBox NAT
                             |
                          pfSense
                       WAN       LAN
                                  |
                              10.20.30.1
                                  |
                       CMMC-LAB NETWORK
                           10.20.30.0/24
                                  |
                 +----------------+----------------+
                 |                                 |
               DC01                       Windows 11 Enterprise
            10.20.30.10                        10.20.30.20
                 |                                 |
       Active Directory                       Domain Joined
             DNS                           Microsoft Defender
         Group Policy                  Defender for Endpoint
                                           Security Logs
```

The internal `10.20.30.0/24` network represented the protected lab environment.

No real CUI was stored or processed.

---

# Environment and Technologies

## Hypervisor

**Oracle VirtualBox**

Used to create and isolate the virtual systems used throughout the project.

---

## Firewall

**pfSense**

LAN IP:

```text
10.20.30.1
```

Responsibilities:

- Network routing
- Firewall protection
- Boundary protection
- Network traffic logging
- Controlled communication between networks

---

## Domain Controller

**Windows Server 2022**

IP address:

```text
10.20.30.10
```

Domain:

```text
homelab.local
```

Responsibilities:

- Active Directory Domain Services
- DNS
- Domain identity management
- Computer management
- Group Policy
- Security policy management

---

## Managed Endpoint

**Windows 11 Enterprise**

IP address:

```text
10.20.30.20
```

Responsibilities:

- Domain authentication
- Group Policy enforcement
- Windows security auditing
- Microsoft Defender Antivirus
- Microsoft Defender for Endpoint
- Security event generation
- Endpoint security testing

---

# Assessment Method

For every selected requirement, I followed the same general assessment process.

## 1. Requirement

What does the security requirement expect the environment to accomplish?

## 2. Implementation

What technology, configuration, or process was used to support the requirement?

## 3. Evidence

What technical evidence proves that the implementation exists?

## 4. Assessment

Does the available evidence actually demonstrate the requirement?

## 5. Gap

What is missing?

## 6. Risk

What security problem could result from the gap?

## 7. Remediation

What should be changed or implemented?

## 8. Owner

Who would normally be responsible for addressing the issue?

## 9. Status

Is the control implemented, partially demonstrated, or still open?

This approach made the project much more than a configuration exercise.

It required both technical implementation and evidence-based assessment.

---

# Security Requirements Assessed

The project assessed **10 selected security requirements** across multiple security families.

| ID | Security Area | Assessment |
|---|---|---|
| AC.L2-3.1.1 | Authorized Access | MET |
| AC.L2-3.1.5 | Least Privilege | MET |
| AU.L2-3.3.1 | System Auditing | MET |
| AU.L2-3.3.2 | User Activity Traceability | MET |
| IA.L2-3.5.1 | Identification | MET |
| IA.L2-3.5.3 | Multifactor Authentication | **NOT MET** |
| SC.L2-3.13.1 | Boundary Protection | MET |
| SI.L2-3.14.1 | Flaw Remediation | MET for demonstrated lab process |
| SI.L2-3.14.2 | Malicious Code Protection | MET |
| SI.L2-3.14.6 | System and Network Monitoring | MET |

---

# Active Directory and Identity Management

A major part of the project involved creating an organized identity structure using Active Directory.

The main Organizational Unit was:

```text
CMMC-Lab
```

Inside this structure, I created:

```text
CMMC-Users
CMMC-Computers
```

This separated users from managed computer objects.

Two domain identities were created:

```text
lab.user
lab.admin
```

The purpose of these accounts was to separate normal activity from administrative activity.

---

# Least Privilege and Privileged Account Separation

The standard account was:

```text
lab.user
```

This account remained a normal domain user.

It was not added to:

```text
Domain Admins
```

The privileged account was:

```text
lab.admin
```

This account was used as the administrative lab identity.

The result was a basic separation between:

```text
lab.user  → Standard user activity

lab.admin → Administrative activity
```

This demonstrated an important security idea:

> A normal user should not automatically receive powerful administrative permissions.

Separating privileged and non-privileged accounts can reduce unnecessary administrative exposure.

It also provides clearer accountability when reviewing user activity.

---

# Managed Device Identification

The Windows 11 Enterprise workstation was successfully joined to:

```text
homelab.local
```

The computer object was placed inside:

```text
CMMC-Lab
└── CMMC-Computers
```

This provided a centralized identity for the managed workstation.

The endpoint could then receive policies from the Domain Controller.

This helped demonstrate that both users and devices could be identified before interacting with the managed environment.

---

# Group Policy and Security Auditing

A major technical part of this project was configuring Windows security auditing through Group Policy.

A Group Policy Object was created named:

```text
CMMC-Audit-Policy
```

The policy was linked to the Organizational Unit containing the managed Windows workstation.

Advanced Windows auditing was configured to provide visibility into important security activity.

Examples included:

- Successful logons
- Failed logons
- Authentication activity
- Account management
- Policy changes
- Process creation
- Other Windows security activity

After configuration, Group Policy was refreshed on the endpoint.

```powershell
gpupdate /force
```

The applied policy could then be reviewed using:

```powershell
gpresult
```

and:

```powershell
auditpol /get /category:*
```

This was an important part of the project because creating a policy on a server does not automatically prove that the endpoint received and applied it.

The endpoint itself needed to be checked.

---

# Windows Security Event Analysis

After configuring auditing, I generated and reviewed Windows security events.

Several important Event IDs were used as evidence.

## Event ID 4624

```text
4624 — Successful Logon
```

This event can provide information about successful authentication activity.

Useful information can include:

- Account name
- Account domain
- Logon information
- Authentication details
- System involved

---

## Event ID 4625

```text
4625 — Failed Logon
```

This event provides evidence of unsuccessful authentication attempts.

Failed authentication events are useful when reviewing activity such as:

- Incorrect passwords
- Unauthorized authentication attempts
- Password guessing
- Account access problems
- Suspicious login behavior

---

## Event ID 4688

```text
4688 — Process Creation
```

This event records process creation when the proper auditing is enabled.

Process creation events can help connect user activity to programs and commands executed on a Windows system.

---

# Why the Event Logs Mattered

The goal was not simply to find Event IDs.

The goal was to demonstrate that user and system activity could be recorded and reviewed.

This supported:

- Security monitoring
- Accountability
- User activity traceability
- Authentication investigation
- Process investigation
- Evidence collection

It also demonstrated the connection between security configuration and actual telemetry.

The process was:

```text
Configure Audit Policy
        ↓
Apply Policy
        ↓
Generate Activity
        ↓
Create Security Events
        ↓
Review Events
        ↓
Collect Evidence
        ↓
Assess Control
```

---

# Microsoft Defender Validation

Microsoft Defender Antivirus was reviewed on the Windows 11 Enterprise endpoint.

Instead of only checking the graphical interface, PowerShell was used to validate the security state.

The following protection settings were confirmed:

```text
AntivirusEnabled = True
RealTimeProtectionEnabled = True
BehaviorMonitorEnabled = True
IoavProtectionEnabled = True
AntispywareEnabled = True
```

These values provided technical evidence that important Microsoft Defender protections were active.

This helped support the malicious-code protection assessment.

---

# Microsoft Defender for Endpoint Validation

Microsoft Defender for Endpoint was also reviewed.

The Windows Sense service was checked using:

```powershell
sc.exe query sense
```

The service returned:

```text
STATE : 4 RUNNING
```

This provided evidence that the Microsoft Defender for Endpoint sensor service was running.

This was important because simply assuming that endpoint monitoring is active is not enough.

The security service itself should be validated.

---

# Network Boundary Protection

pfSense acted as the firewall and router for the isolated lab network.

The LAN interface was configured as:

```text
10.20.30.1
```

The protected internal network was:

```text
10.20.30.0/24
```

Evidence reviewed included:

- WAN configuration
- LAN configuration
- Firewall rules
- Firewall logs
- Internal network addressing

pfSense provided a defined network boundary around the systems included in the project.

The boundary protection requirement was assessed as:

```text
MET — within the defined lab scope
```

The lab does not represent a full enterprise firewall architecture.

Enterprise-level network segmentation and centralized firewall management were outside the scope of this project.

---

# System and Network Monitoring

The project used several different sources of security visibility.

## Endpoint Visibility

Provided by:

- Windows Security Event Logs
- Microsoft Defender Antivirus
- Microsoft Defender for Endpoint

## Network Visibility

Provided by:

- pfSense
- Firewall rules
- Firewall logs

Together, these technologies provided both endpoint and network security information.

The project did not include a full production Security Information and Event Management platform collecting every source.

Because of that limitation, centralized enterprise event correlation was documented as a gap in the lab environment.

A larger production environment could improve this by forwarding security events into a centralized security monitoring platform.

---

# Flaw Remediation

Another part of the assessment involved reviewing security findings and considering how weaknesses should be managed.

The basic remediation process followed was:

```text
Identify Finding
      ↓
Review Risk
      ↓
Determine Priority
      ↓
Assign Owner
      ↓
Define Remediation
      ↓
Track Status
      ↓
Validate Fix
```

The home lab did not include a complete enterprise vulnerability management program, formal ticketing system, or business remediation SLA.

Those limitations were documented.

In a larger environment, security findings should normally include information such as:

- Finding
- Affected system
- Risk
- Severity
- Priority
- Responsible owner
- Remediation action
- Due date
- Current status
- Validation evidence

---

# Major Security Finding

## IA.L2-3.5.3 — Multifactor Authentication

The most important open finding from the assessment involved multifactor authentication.

The environment successfully supported Active Directory authentication.

Users could authenticate using:

```text
Username
+
Password
```

However, the evidence did not demonstrate a complete second authentication factor for all applicable privileged and network authentication scenarios.

Because of this, the control was assessed as:

```text
NOT MET
```

This result was intentional.

I did not want to mark a security requirement as successful when the available evidence did not support that conclusion.

---

# Why the MFA Finding Matters

Passwords can be compromised in many different ways.

Examples include:

- Phishing
- Password reuse
- Credential theft
- Malware
- Credential dumping
- Brute-force attempts
- Password spraying
- Social engineering
- Exposed credentials

If a password is the only factor protecting an account, possession of that password may be enough to authenticate.

A second factor can make unauthorized authentication more difficult.

This becomes especially important when dealing with privileged accounts.

---

# Risk Analysis

## Finding

Incomplete MFA coverage.

## Security Risk

A stolen or guessed password could potentially allow unauthorized authentication without requiring another authentication factor.

## Possible Impact

Depending on the account involved, unauthorized access could potentially affect:

- User accounts
- Administrative access
- Systems
- Security configurations
- Sensitive resources
- Network access

## Priority

```text
High
```

## Owner

```text
Identity / Systems Administrator
```

## Status

```text
Open
```

---

# Remediation Plan

The recommended remediation was to implement and validate an appropriate MFA solution for the required authentication scenarios.

Depending on the real environment, possible technologies could include:

- Smart card authentication
- Certificate-backed authentication
- Windows Hello for Business
- Another approved enterprise MFA solution

The exact technology would depend on the organization's architecture and requirements.

Most importantly, installing an MFA product would not automatically close the finding.

The implementation would need to be tested.

---

# Remediation Validation

Before closing the finding, evidence should demonstrate that MFA actually protects the required authentication scenarios.

Possible validation evidence could include:

- MFA configuration
- Authentication policy
- Successful MFA authentication
- Privileged authentication testing
- Applicable network authentication testing
- User authentication evidence
- Administrative authentication evidence
- Updated control assessment
- Updated evidence tracker
- Closure documentation

The basic process would be:

```text
Open Finding
      ↓
Implement MFA
      ↓
Test MFA
      ↓
Collect Evidence
      ↓
Validate Coverage
      ↓
Reassess Control
      ↓
Close Finding
```

---

# POA&M

The unresolved MFA finding was documented in a **Plan of Action and Milestones (POA&M)**.

The purpose of the POA&M was to make sure the weakness was formally tracked instead of simply being mentioned and forgotten.

## POA&M Finding

```text
POAM-001
```

## Related Control

```text
IA.L2-3.5.3
```

## Weakness

Required MFA coverage was not fully demonstrated.

## Risk

Compromised passwords could allow unauthorized authentication without an additional authentication factor.

## Required Action

Implement and validate an appropriate MFA solution for the applicable authentication scenarios.

## Priority

```text
High
```

## Owner

```text
Identity / Systems Administrator
```

## Status

```text
Open
```

The finding should remain open until technical evidence proves that the required remediation has been completed.

---

# Assessment Results

The final assessment produced the following results:

```text
Total Controls Assessed: 10

MET: 9

NOT MET: 1

Open Findings: 1
```

The open finding was:

```text
IA.L2-3.5.3 — Multifactor Authentication
```

The final result was not:

> "Everything is compliant."

The actual result was:

> Nine selected requirements were demonstrated sufficiently within the defined home-lab scope, while one MFA requirement remained NOT MET and required remediation.

That distinction was one of the most important parts of the project.

---

# Evidence Collection

Evidence was collected throughout the project instead of waiting until the end.

Examples of evidence included:

### Network Evidence

- pfSense WAN configuration
- pfSense LAN configuration
- Firewall rules
- Firewall logs
- Network addressing

### Identity Evidence

- Active Directory Organizational Units
- Domain user accounts
- Standard user group membership
- Administrative account group membership
- Managed computer object
- Domain authentication

### Audit Evidence

- Group Policy configuration
- Advanced Audit Policy
- Applied policy information
- Event ID 4624
- Event ID 4625
- Event ID 4688

### Endpoint Security Evidence

- Microsoft Defender status
- Real-time protection status
- Behavior monitoring status
- Antispyware status
- Microsoft Defender for Endpoint Sense service

### Assessment Evidence

- Control assessment
- Control evidence tracker
- Gap documentation
- Risk documentation
- Remediation actions
- POA&M

---

# Evidence Philosophy

One of the most important ideas I practiced throughout this project was:

> Evidence should prove the claim being made.

For example:

### Claim

Least privilege is implemented.

### Better Evidence

Show that the standard account is not a Domain Administrator and that administrative access is separated.

---

### Claim

Windows auditing is implemented.

### Better Evidence

Show the audit policy, prove that the endpoint received it, generate activity, and show the resulting Windows events.

---

### Claim

Malware protection is enabled.

### Better Evidence

Validate Microsoft Defender protection states and confirm that the required services are running.

---

### Claim

Boundary protection exists.

### Better Evidence

Show the firewall architecture, interfaces, rules, and logging.

---

### Claim

MFA is implemented.

### Required Evidence

Demonstrate a real second authentication factor for the required authentication scenarios.

In this project, that final evidence did not exist.

Therefore:

```text
IA.L2-3.5.3 = NOT MET
```

---

# Troubleshooting

The project required troubleshooting across networking, DNS, Active Directory, identity, and Windows configuration.

## DNS and Domain Controller Discovery

The Windows workstation needed to correctly locate the Active Directory Domain Controller.

DNS was especially important because Active Directory relies heavily on DNS for domain services.

Testing included commands such as:

```powershell
nslookup
```

and:

```powershell
nltest /dsgetdc:homelab.local
```

I also reviewed Active Directory DNS information and domain controller discovery.

This reinforced an important lesson:

> A system being able to reach another IP address does not automatically mean that Active Directory services will work correctly.

DNS configuration must also be correct.

---

# Domain Join Troubleshooting

The Windows 11 Enterprise endpoint also required identity troubleshooting before it could be joined to the traditional Active Directory domain.

The endpoint had previously been connected to Microsoft Entra ID.

A local administrative account was created so the system could be safely disconnected from the previous identity configuration.

After the identity configuration was corrected, the workstation was successfully joined to:

```text
homelab.local
```

This troubleshooting provided additional experience with:

- Local accounts
- Administrative access
- Microsoft Entra ID
- Active Directory
- Domain joining
- DNS
- Windows identity configuration

---

# Project Deliverables

The project produced multiple security and compliance artifacts.

These included:

```text
README.md
```

```text
Week20_Lab1_Compliance_Assessment.md
```

```text
Week20_Lab1_Control_Evidence_Tracker.md
```

```text
Week20_Lab1_POAM.md
```

```text
Week_20_NIST800-171_CMMC_Evidence_Control_Assessment_Technical_Analysis_writeup.md
```

```text
Week_20_NIST800-171_CMMC_MFA_Compliance_Gap_Incident_Case_Study.md
```

The project also includes screenshots collected during the technical implementation and assessment process.

---

# Suggested Repository Structure

```text
NIST-800-171-CMMC-Evidence-Control-Assessment/
│
├── README.md
│
├── documentation/
│   ├── Week20_Lab1_Compliance_Assessment.md
│   ├── Week20_Lab1_Control_Evidence_Tracker.md
│   ├── Week20_Lab1_POAM.md
│   ├── Week_20_NIST800-171_CMMC_Evidence_Control_Assessment_Technical_Analysis_writeup.md
│   └── Week_20_NIST800-171_CMMC_MFA_Compliance_Gap_Incident_Case_Study.md
│
└── screenshots/
    ├── network/
    ├── active-directory/
    ├── group-policy/
    ├── event-logs/
    ├── defender/
    ├── pfsense/
    └── assessment/
```

---

# Skills Demonstrated

## Security Control Assessment

- Requirement interpretation
- Technical control mapping
- Evidence collection
- Evidence validation
- Control assessment
- MET / NOT MET decisions
- Gap analysis

## Identity and Access Management

- Active Directory
- Domain accounts
- Computer objects
- Organizational Units
- Domain authentication
- Least privilege
- Privileged account separation
- User and device identification

## Windows Security Administration

- Windows Server 2022
- Windows 11 Enterprise
- Group Policy
- Advanced Audit Policy
- Domain joining
- Windows security configuration
- Windows administration

## Security Monitoring

- Windows Security Event Logs
- Authentication monitoring
- Process creation monitoring
- Firewall monitoring
- Endpoint monitoring

## Event Analysis

- Event ID 4624
- Event ID 4625
- Event ID 4688
- Authentication analysis
- User activity traceability
- Process activity review

## Endpoint Security

- Microsoft Defender Antivirus
- Microsoft Defender for Endpoint
- PowerShell security validation
- Real-time protection validation
- Endpoint service validation

## Network Security

- pfSense
- Firewall rules
- Firewall logging
- Network segmentation
- Boundary protection
- Virtual networking

## Risk Management

- Security gap identification
- Risk analysis
- Priority assignment
- Remediation planning
- Finding ownership
- Status tracking
- Validation planning

## Security Documentation

- Assessment reporting
- Evidence tracking
- POA&M documentation
- Technical analysis
- Security finding documentation
- Remediation documentation

## Troubleshooting

- DNS troubleshooting
- Domain Controller discovery
- Active Directory troubleshooting
- Domain join troubleshooting
- Identity troubleshooting
- Network troubleshooting
- Policy validation

---

# Key Lessons Learned

## 1. Installing a Security Tool Does Not Prove a Security Control

One of the biggest lessons from this project was learning the difference between having technology and proving that the technology satisfies a security requirement.

For example:

```text
Active Directory exists
```

does not automatically mean:

```text
Least privilege is proven
```

The permissions and group memberships must be reviewed.

---

## 2. Creating a Group Policy Does Not Prove It Reached the Endpoint

A policy can exist on the Domain Controller without being correctly applied to the workstation.

That is why endpoint-side validation matters.

---

## 3. Security Logs Need Context

Finding Event ID 4624 or 4625 is only the beginning.

The event needs to be connected to:

- A user
- A system
- A time
- An authentication result
- A security question

This turns raw logging into useful evidence.

---

## 4. Endpoint Protection Should Be Validated

Seeing Microsoft Defender installed is not enough.

Protection settings and services should be checked to confirm that the protection is actually active.

---

## 5. A Firewall Is More Than an Appliance

Boundary protection requires understanding:

- What network is being protected
- Where the boundary exists
- Which interfaces are involved
- What traffic is allowed
- What traffic is blocked
- What activity is logged

---

## 6. A NOT MET Finding Can Be Valuable

The MFA result was one of the most useful parts of the entire project.

It would have been easy to make the project look cleaner by calling every control successful.

That would not have reflected the evidence.

The better assessment was:

```text
Evidence does not prove complete MFA coverage.

Result: NOT MET
```

Then I documented the risk and remediation.

---

## 7. Findings Need Ownership

A security weakness without an owner can remain unresolved.

Documenting an owner makes it clear who should be responsible for moving the remediation forward.

---

## 8. Remediation Needs Validation

A finding should not be closed simply because someone says:

> "We fixed it."

The fix should be tested and supported by new evidence.

---

## 9. Technical Security and Risk Are Connected

A missing technical control can create a business or security risk.

This project helped connect:

```text
Technical Weakness
       ↓
Security Risk
       ↓
Remediation
       ↓
Validation
       ↓
Risk Reduction
```

---

# Project Limitations

This project was completed in a home lab.

It does not represent a complete enterprise implementation.

Important limitations include:

- No real Controlled Unclassified Information
- No real defense contractor environment
- No official CMMC assessment
- No official certification
- No enterprise production network
- No large endpoint fleet
- No complete enterprise SIEM implementation
- No 24/7 monitoring operation
- No formal enterprise ticketing workflow
- No production remediation SLA
- No complete enterprise MFA deployment
- Only selected security requirements were assessed

These limitations are intentionally documented so the project does not overstate what was completed.

---

# Important Scope Statement

This project should be understood as:

> A hands-on training environment used to practice NIST SP 800-171/CMMC control mapping, Microsoft security configuration, evidence collection, security assessment, gap analysis, risk documentation, and remediation tracking.

It should **not** be understood as:

> An official CMMC certification or assessment of a real organization.

---

# How This Project Connects Technical Security to Risk

One of the reasons I built this project was to connect technical security work with the larger security assessment process.

For example:

```text
Active Directory
        ↓
Identity Management
        ↓
Access Control
        ↓
Evidence
        ↓
Assessment
        ↓
Risk
```

Another example:

```text
Windows Group Policy
        ↓
Audit Configuration
        ↓
Security Events
        ↓
Monitoring Evidence
        ↓
Assessment
```

Another:

```text
Missing MFA
        ↓
Authentication Weakness
        ↓
Credential Risk
        ↓
NOT MET Finding
        ↓
POA&M
        ↓
Remediation
        ↓
Future Validation
```

This helped me understand cybersecurity as more than individual tools.

The tools, configurations, logs, evidence, risks, and documentation all connect to each other.

---

# Technical Takeaway

The most important technical takeaway from this project was:

> A security requirement should be supported by evidence that directly proves the requirement.

That means the process cannot stop at configuration.

A stronger workflow is:

```text
Understand
    ↓
Configure
    ↓
Test
    ↓
Collect
    ↓
Analyze
    ↓
Assess
    ↓
Document
    ↓
Remediate
    ↓
Validate
```

---

# Final Assessment Summary

| Category | Result |
|---|---|
| Assessment Type | Simulated NIST SP 800-171 / CMMC Control Assessment |
| Environment | Isolated Virtual Home Lab |
| Domain | homelab.local |
| Controls Assessed | 10 |
| Controls MET | 9 |
| Controls NOT MET | 1 |
| Open Finding | MFA |
| Firewall | pfSense |
| Identity Platform | Active Directory |
| Managed Endpoint | Windows 11 Enterprise |
| Endpoint Protection | Microsoft Defender / MDE |
| Audit Source | Windows Security Event Logs |
| Remediation Tracking | POA&M |

---

# Final Conclusion

This project gave me hands-on experience connecting security technology with security requirements, evidence, risk, and remediation.

I did more than configure Active Directory, Windows auditing, Microsoft Defender, and pfSense.

I had to determine what each selected requirement expected, identify the supporting technical control, collect evidence, test the implementation, review the results, identify gaps, document risk, and create remediation actions.

The project included:

- Building an isolated security lab
- Configuring Active Directory
- Creating standard and privileged identities
- Applying least privilege
- Managing a domain-joined Windows endpoint
- Creating Windows audit policies through Group Policy
- Reviewing Windows authentication events
- Reviewing process creation events
- Validating Microsoft Defender protections
- Validating Microsoft Defender for Endpoint
- Reviewing pfSense boundary controls
- Reviewing firewall logs
- Assessing security requirements
- Collecting technical evidence
- Identifying an MFA security gap
- Performing risk analysis
- Creating remediation documentation
- Building a POA&M
- Producing formal assessment documentation

The final assessment resulted in:

```text
10 Controls Assessed
9 MET
1 NOT MET
1 Open MFA Finding
```

Most importantly, this project taught me not to confuse a configured security tool with a proven security control.

A strong assessment should be based on what the evidence actually demonstrates.

When the evidence did not demonstrate complete MFA coverage, I documented the control as **NOT MET**, explained the risk, assigned an owner, created a remediation plan, and tracked the finding through a POA&M.

That process helped me better understand how technical cybersecurity, security operations, identity security, endpoint security, network security, risk management, compliance, and documentation work together in a real security program.

---

## Disclaimer

This repository documents a cybersecurity training project completed in an isolated home-lab environment.

No real Controlled Unclassified Information (CUI), government information, production credentials, customer information, or other sensitive data was used.

The project is not an official CMMC assessment, certification, or statement of compliance.

All findings and assessment results apply only to the defined training environment and project scope.

---

**Week 20 Lab 2 — NIST SP 800-171 / CMMC Evidence & Control Assessment**
