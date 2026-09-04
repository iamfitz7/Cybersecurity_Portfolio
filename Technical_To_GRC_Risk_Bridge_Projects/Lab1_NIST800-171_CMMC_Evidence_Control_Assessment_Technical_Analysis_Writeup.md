PROFESSIONAL TECHNICAL ANALYSIS LAB WRITE-UP

PROJECT OVERVIEW

In this project, I built and assessed a small cybersecurity environment designed to simulate part of a NIST SP 800-171 and CMMC Level 2 compliance assessment.

Unlike many of my previous cybersecurity labs that focused mainly on investigating alerts or suspicious activity, this project focused on security controls, technical evidence, compliance assessment, risk, and remediation.

I worked through the project from three different points of view:

System Administrator

Security Analyst

Compliance Assessor

First, I configured security controls inside the lab.

Next, I collected evidence showing whether those controls were actually working.

Finally, I reviewed the evidence and decided whether each selected security requirement was MET or NOT MET.

The main lesson of this project was that having a security tool or configuration does not automatically mean that a security requirement has been satisfied. The evidence has to actually support the assessment result.

PROJECT OBJECTIVE

The goal of this project was to gain hands-on experience with the technical side of NIST SP 800-171 and CMMC-style compliance work.

I wanted to understand how security requirements connect to real technology such as:

Active Directory

Windows Server

Group Policy

Windows Security Event Logs

Microsoft Defender Antivirus

Microsoft Defender for Endpoint

pfSense

Windows authentication

User and computer accounts

Firewall rules

Audit policies

Security monitoring

I also wanted to practice documenting security gaps instead of incorrectly marking every control as successful.

This was a simulated home-lab project.

It was not an official CMMC assessment, official certification, or assessment of a real defense contractor.

No real Controlled Unclassified Information was used.

LAB ENVIRONMENT

The project was built in Oracle VirtualBox.

The main systems were:

pfSense Firewall and Router

Windows Server 2022 Domain Controller

Windows 11 Enterprise Endpoint

Active Directory Domain Services

Windows DNS

Group Policy

Microsoft Defender Antivirus

Microsoft Defender for Endpoint

Windows Security Event Logs

The Active Directory domain used in the project was:

homelab.local

The main network addresses were:

pfSense LAN:
10.20.30.1

Domain Controller:
10.20.30.10

Windows 11 Enterprise Endpoint:
10.20.30.20

The internal protected network used the 10.20.30.0/24 range.

pfSense acted as the security boundary between the internal CMMC lab environment and outside connectivity.

The Windows Server system provided Active Directory, DNS, identity management, and Group Policy.

The Windows 11 Enterprise system acted as the managed endpoint where security policies, authentication activity, event logging, Defender protection, and endpoint monitoring could be tested.

This architecture allowed me to connect identity, endpoint security, network security, monitoring, and compliance evidence into one project.

ASSESSMENT SCOPE

Systems and technologies included in the assessment were:

pfSense

Windows Server 2022

Active Directory

Windows DNS

Group Policy

Windows 11 Enterprise

Windows Security Event Logs

Microsoft Defender Antivirus

Microsoft Defender for Endpoint

Systems outside the assessment scope included:

My physical HP Spectre laptop

Kali Linux

Ubuntu

Other unrelated virtual machines

My personal home network

My physical HP Spectre was only used to save documentation, screenshots, and project files.

The actual security configuration and testing occurred inside the isolated virtual lab environment.

ASSESSMENT METHOD

For each security control, I followed a basic assessment process:

Requirement

Implementation

Evidence

Assessment Result

Gap

Risk

Remediation

Owner

Status

This process was important because it forced me to answer more than just:

“Did I configure something?”

Instead, I had to ask:

“What exactly does the security requirement expect?”

“What did I configure to support it?”

“What evidence proves the configuration exists?”

“Does that evidence actually satisfy the requirement?”

“What is missing?”

“What security risk does that create?”

“What should be done to fix it?”

“Who would normally be responsible for fixing it?”

That made the project much closer to real compliance and security engineering work than simply reading a security framework.

SELECTED SECURITY REQUIREMENTS

I assessed ten selected requirements across several security areas.

The controls covered:

Access Control

Audit and Accountability

Identification and Authentication

System and Communications Protection

System and Information Integrity

The selected controls were:

AC.L2-3.1.1
Authorized Access

AC.L2-3.1.5
Least Privilege

AU.L2-3.3.1
System Auditing

AU.L2-3.3.2
User Activity Traceability

IA.L2-3.5.1
Identification

IA.L2-3.5.3
Multifactor Authentication

SC.L2-3.13.1
Boundary Protection

SI.L2-3.14.1
Flaw Remediation

SI.L2-3.14.2
Malicious Code Protection

SI.L2-3.14.6
System and Network Monitoring

ACTIVE DIRECTORY AND AUTHORIZED ACCESS

One of the first parts of the project was building an organized identity structure inside Active Directory.

I created a main Organizational Unit named:

CMMC-Lab

Inside it, I created:

CMMC-Users

CMMC-Computers

I then created two separate domain accounts:

lab.user

lab.admin

The purpose of creating two accounts was to separate normal user activity from privileged administrative activity.

The lab.user account represented a standard user.

The lab.admin account represented a dedicated administrative identity.

LEAST PRIVILEGE

I reviewed the group memberships of both accounts.

The standard lab.user account remained a normal domain user and was not given unnecessary administrative access.

The lab.admin account was used for administrative functions.

This demonstrated an important security principle:

A normal user should not automatically have administrative privileges.

Using separate accounts reduces the amount of privileged access being used during normal activity.

This helps reduce the damage that could occur if a normal user session or credential became compromised.

The project therefore demonstrated basic privileged-account separation and least privilege.

AUTHORIZED DEVICE MANAGEMENT

The Windows 11 Enterprise workstation was successfully joined to the homelab.local Active Directory domain.

The computer object was then placed inside the CMMC-Computers Organizational Unit.

This allowed the endpoint to be centrally identified and managed through Active Directory.

It also allowed Group Policy settings to be applied specifically to the managed workstation.

The environment therefore had both centralized user identities and a centralized computer identity.

WINDOWS GROUP POLICY AUDITING

A major part of the project involved creating a domain Group Policy Object named:

CMMC-Audit-Policy

The policy was linked to the Organizational Unit containing the Windows workstation.

I configured advanced Windows security auditing so the endpoint could create useful security records.

Audit categories included activity such as:

User logons

User logoffs

Account lockouts

Account management

Authentication changes

Policy changes

Process creation

System security activity

I then forced Group Policy to update on the Windows workstation using:

gpupdate /force

I also reviewed the actual applied Group Policy and audit configuration using tools such as:

gpresult

auditpol /get /category:*

This was important because there is a difference between configuring a policy on the Domain Controller and proving that the endpoint actually received the policy.

WINDOWS EVENT LOG INVESTIGATION

After applying the audit policy, I created real authentication and system activity and reviewed Windows Security Event Logs.

I investigated several important Windows Event IDs.

Event ID 4624 represented successful logon activity.

Event ID 4625 represented failed logon activity.

Event ID 4688 represented process creation activity.

The project showed that Windows logs could provide useful information such as:

User account

Account domain

Computer

Time

Logon information

Process activity

Authentication results

This allowed security activity to be connected back to specific users and systems.

The evidence collected from these events supported the audit and accountability portion of the assessment. The project specifically used Events 4624, 4625, and 4688 as evidence for user traceability and monitoring.

MICROSOFT DEFENDER ANTIVIRUS VALIDATION

I verified that Microsoft Defender Antivirus protections were active on the Windows 11 Enterprise endpoint.

Using PowerShell, I confirmed the following values:

AntivirusEnabled = True

RealTimeProtectionEnabled = True

BehaviorMonitorEnabled = True

IoavProtectionEnabled = True

AntispywareEnabled = True

This provided direct technical evidence that the endpoint had active Microsoft anti-malware protection.

Instead of only relying on the Windows Security graphical interface, I used PowerShell to validate the actual protection state.

MICROSOFT DEFENDER FOR ENDPOINT VALIDATION

I also verified the Microsoft Defender for Endpoint Sense service.

The command used was:

sc.exe query sense

The service returned:

STATE : 4 RUNNING

This confirmed that the Defender for Endpoint sensor service was running on the endpoint.

This strengthened the project because I was able to show that both Microsoft Defender Antivirus and Defender for Endpoint components were active.

BOUNDARY PROTECTION WITH PFSENSE

pfSense was used as the firewall and router protecting the virtual lab.

Its LAN address was:

10.20.30.1

The internal systems communicated through the protected CMMC lab network.

I reviewed:

WAN and LAN interfaces

Firewall rules

Network boundary configuration

Firewall logs

This provided evidence for the boundary protection requirement.

The lab did not attempt to represent a complete enterprise firewall architecture.

However, it demonstrated the core idea of creating a defined protected network and using a firewall to control communication at that boundary.

The assessment marked this requirement as MET within the lab scope while also documenting that enterprise-level segmentation and centralized firewall administration were outside the lab environment.

SYSTEM AND NETWORK MONITORING

The lab included several different sources of monitoring.

Windows Security Event Logs provided endpoint authentication and system activity.

Microsoft Defender provided endpoint protection information.

Microsoft Defender for Endpoint provided endpoint monitoring capability.

pfSense provided network firewall logging and visibility.

Together, these tools created both endpoint and network visibility.

The lab did not include a full enterprise SIEM or a 24/7 Security Operations Center.

That limitation was documented instead of being ignored.

In a production environment, an improvement would be to forward important endpoint, authentication, and firewall events into a centralized SIEM for correlation, retention, and investigation.

FLAW REMEDIATION

The project also included reviewing Microsoft security and Windows security information for endpoint security weaknesses, security recommendations, vulnerabilities, or update conditions.

The purpose was to demonstrate the process of:

Identifying a security weakness

Reviewing its risk

Determining a remediation action

Assigning responsibility

Tracking status

Validating the result

A full enterprise vulnerability management system was outside the scope of the home lab.

There was no formal business ticketing system or company remediation SLA.

In a real environment, identified weaknesses should be assigned an owner, priority, remediation deadline, status, and validation process.

THE MFA FINDING

The most important assessment result in the project was the multifactor authentication control.

Control:

IA.L2-3.5.3

The Active Directory environment used username and password authentication.

The evidence did not demonstrate a complete second authentication factor for all applicable privileged and network access situations.

Because of that, I did not mark the control as successful.

Assessment Result:

NOT MET

The gap was:

Required MFA coverage had not been fully demonstrated.

The risk was:

If an attacker obtained or guessed a valid password, that credential could potentially be used without requiring another authentication factor.

The recommended remediation was to implement and validate an appropriate MFA solution for the required authentication scenarios.

Possible technologies in a real environment could include:

Smart card authentication

Certificate-backed authentication

Windows Hello for Business

Another approved enterprise MFA solution

The owner was documented as:

Identity / Systems Administrator

The status was:

Open

This finding was then documented in a Plan of Action and Milestones.

I believe this was one of the most important parts of the project because I did not try to make every control appear compliant.

I made the assessment based on the evidence that actually existed. The finished assessment recorded IA.L2-3.5.3 as NOT MET because a complete second authentication factor had not been demonstrated.

POA&M AND REMEDIATION TRACKING

After identifying the MFA gap, I created a Plan of Action and Milestones document.

The POA&M documented:

Control ID

Weakness

Risk

Required remediation

Owner

Priority

Current status

Validation method

Closure evidence

The MFA finding remained open because the second authentication factor had not been implemented and validated.

The purpose of the POA&M was to show that an identified security gap should not simply be forgotten.

It should be formally tracked until remediation is completed and verified.

COMPLIANCE DOCUMENTATION

I completed several supporting compliance documents for this project.

These included:

Compliance Assessment

Control Evidence Tracker

Plan of Action and Milestones

The compliance assessment explained how the selected security requirements were evaluated.

The evidence tracker connected each requirement to implementation details and technical evidence.

The POA&M documented the unresolved MFA weakness and its remediation plan.

FINAL ASSESSMENT RESULTS

Total controls assessed:

10

Controls assessed as MET:

9

Controls assessed as NOT MET:

1

Open finding:

IA.L2-3.5.3 — Multifactor Authentication

The assessment demonstrated security controls in areas including:

Access control

Least privilege

Identity management

Windows security auditing

User traceability

Boundary protection

Malware protection

Endpoint monitoring

Network monitoring

Flaw remediation

Compliance documentation

The environment should not be described as fully CMMC compliant.

It was a simulated security assessment performed inside a controlled home lab.

The MFA gap remained an open finding requiring remediation.

TECHNICAL CHALLENGES AND TROUBLESHOOTING

This project also required troubleshooting.

One important issue involved Active Directory and DNS.

The Windows endpoint originally had trouble locating the Active Directory domain correctly.

I tested DNS using commands such as nslookup and checked the domain controller locator process.

I verified DNS SRV records for Active Directory services.

I also used:

nltest /dsgetdc:homelab.local

This helped confirm that the workstation could locate the domain controller.

Another challenge occurred because the Windows 11 Enterprise system was originally joined to Microsoft Entra ID.

That prevented it from immediately joining the traditional Active Directory domain.

I created a local administrative account so I could safely disconnect the device from Entra ID and continue the lab.

After disconnecting it, I successfully joined the workstation to:

homelab.local

This troubleshooting was valuable because it showed how identity systems, DNS, local administration, Entra ID, and Active Directory can affect each other.

WHAT I LEARNED

One of the biggest lessons I learned is that security implementation and security evidence are not the same thing.

For example:

Having Active Directory does not automatically prove least privilege.

I had to review accounts and group memberships.

Having Windows Event Viewer does not automatically prove auditing is configured properly.

I had to configure the audit policy, apply it, create activity, and review the resulting events.

Having Microsoft Defender installed does not automatically prove malware protection is active.

I verified its protection status and checked that the Defender for Endpoint Sense service was running.

Having a firewall does not automatically prove boundary protection.

I reviewed the pfSense network design, interfaces, rules, and logs.

Most importantly, I learned that finding a NOT MET control is not the same as failing the project.

The job of an assessment is to identify what the evidence actually supports.

The MFA requirement was intentionally marked NOT MET because the evidence did not demonstrate the required second authentication factor.

The correct next step was to document the gap, risk, remediation, owner, and status.

SECURITY SKILLS PRACTICED

This project gave me hands-on practice with:

NIST SP 800-171 control mapping

CMMC-style evidence assessment

Active Directory

Windows Server 2022

Windows 11 Enterprise

Domain joining

DNS troubleshooting

Organizational Units

Domain user administration

Least privilege

Privileged account separation

Group Policy

Advanced Windows Audit Policy

Windows Event Viewer

Windows Security Event IDs

Microsoft Defender Antivirus

Microsoft Defender for Endpoint

PowerShell security validation

pfSense firewall administration

Boundary protection

Security monitoring

Evidence collection

Control assessment

Gap analysis

Risk documentation

Remediation planning

POA&M documentation

FINAL CONCLUSION

This project helped me move beyond simply configuring cybersecurity tools.

I practiced connecting technical configuration to security requirements and then proving the implementation through evidence.

I built a small Microsoft domain environment, configured identity and auditing controls, validated endpoint protection, reviewed security events, used pfSense for network boundary protection, assessed ten selected security requirements, documented an MFA deficiency, and created remediation documentation.

The final assessment resulted in nine controls being sufficiently demonstrated for the defined lab scope and one control being marked NOT MET.

The most important part of the project was learning to make evidence-based security decisions instead of claiming compliance without proof.

This project represents hands-on NIST SP 800-171/CMMC control mapping, Microsoft security configuration, evidence collection, assessment documentation, gap analysis, and remediation tracking in an isolated training environment.