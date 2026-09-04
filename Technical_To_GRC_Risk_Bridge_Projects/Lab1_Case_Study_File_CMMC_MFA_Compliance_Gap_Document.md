NIST SP 800-171 / CMMC MFA COMPLIANCE GAP

SECURITY FINDING CASE STUDY

CASE STUDY OVERVIEW

During a simulated NIST SP 800-171 and CMMC Level 2 security assessment of my home lab, I identified an authentication weakness involving multifactor authentication.

The environment used Active Directory for centralized authentication and included separate standard and administrative user accounts.

During the assessment, I reviewed whether the available evidence was enough to satisfy the multifactor authentication requirement.

The evidence showed that users could authenticate using a username and password.

However, I could not demonstrate a complete second authentication factor for all required privileged and network authentication situations.

Because of this, I assessed the MFA requirement as NOT MET and created a formal remediation record instead of incorrectly marking the requirement as compliant.

CASE TYPE

Compliance Control Deficiency

Authentication Security Gap

Identity and Access Management Finding

RELATED CONTROL

IA.L2-3.5.3

Multifactor Authentication

ENVIRONMENT

Active Directory Domain:

homelab.local

Domain Controller:

Windows Server 2022

Domain Controller IP:

10.20.30.10

Windows Endpoint:

Windows 11 Enterprise

Endpoint IP:

10.20.30.20

Firewall:

pfSense

Firewall LAN:

10.20.30.1

Security Technologies:

Active Directory

Windows Group Policy

Windows Security Event Logs

Microsoft Defender Antivirus

Microsoft Defender for Endpoint

pfSense

ACCOUNTS INVOLVED

Standard account:

lab.user

Administrative account:

lab.admin

The two accounts were intentionally separated so normal activity and administrative activity would not use the same privilege level.

BACKGROUND

As part of the larger project, I selected ten NIST SP 800-171 / CMMC-style security requirements for assessment.

The goal was not simply to configure technologies.

For every requirement, I asked whether the technical evidence actually proved that the requirement had been satisfied.

The assessment process was:

Requirement

Implementation

Evidence

Assessment

Gap

Risk

Remediation

Owner

Status

This approach became especially important when I reached the multifactor authentication requirement.

INITIAL CONDITION

The Active Directory environment successfully supported username and password authentication.

The Windows 11 Enterprise workstation was joined to homelab.local.

The workstation could locate and communicate with the Domain Controller.

Domain identities could authenticate successfully.

This proved that centralized domain authentication was working.

However, successful authentication alone did not prove multifactor authentication.

OBSERVED AUTHENTICATION METHOD

The lab environment used:

Username

Password

A separate second authentication factor was not demonstrated for all required privileged and network authentication situations.

This meant that the available evidence only showed single-factor password authentication for the tested Active Directory logons.

ASSESSMENT QUESTION

The main question was:

Does the available evidence demonstrate that multifactor authentication is required for the applicable privileged and network authentication scenarios?

The answer was:

No.

ASSESSMENT RESULT

Control:

IA.L2-3.5.3

Result:

NOT MET

Reason:

A complete second authentication factor was not demonstrated.

This result was intentionally left as NOT MET.

I did not assume that MFA existed simply because Microsoft security products were present in the environment.

WHY THIS MATTERED

A username and password are one authentication factor.

If an attacker obtains both pieces of information through credential theft, phishing, password reuse, credential dumping, or guessing, the attacker may be able to authenticate as the victim.

A second authentication factor makes this type of attack more difficult because stealing only the password may no longer be enough to gain access.

SECURITY RISK

The main risk was unauthorized authentication using stolen or compromised credentials.

Possible causes could include:

Phishing

Password reuse

Credential dumping

Brute-force activity

Password spraying

Malware

Social engineering

Weak passwords

Exposed credentials

If a privileged account were compromised, the impact could be much more serious because the attacker may gain administrative access to systems or Active Directory.

This made the MFA deficiency especially important for privileged access.

RELATED LEAST PRIVILEGE PROTECTION

The lab already had one useful protection in place.

Standard activity and administrative activity were separated between:

lab.user

and

lab.admin

The lab.user account remained a standard identity.

The lab.admin account was the dedicated privileged identity.

This reduced unnecessary administrative use.

However, least privilege and MFA solve different security problems.

Least privilege limits what an account can do.

MFA helps strengthen how the user proves their identity.

Having least privilege did not remove the MFA gap.

EVIDENCE REVIEWED

The evidence supporting the finding included:

Active Directory user accounts

Successful domain membership

Domain authentication

lab.user authentication

lab.admin account configuration

Windows Security logon events

Username and password authentication

No demonstrated complete second factor

The key evidence was not only what was present.

The missing evidence was also important.

There was no proof that a second authentication factor was required for all applicable access situations.

WINDOWS SECURITY LOG EVIDENCE

Windows Security auditing was configured through Group Policy as part of the project.

This provided evidence of authentication activity.

Event ID 4624 was reviewed for successful logon activity.

Event ID 4625 was reviewed for failed logon activity.

These events helped demonstrate that authentication activity was being recorded.

The project also reviewed Event ID 4688 for process creation.

These logs improved accountability and traceability, but they did not prove that MFA was enabled.

That difference was important.

Logging an authentication event and requiring multiple authentication factors are separate security controls.

ROOT CAUSE

The root cause was not a technical failure or broken system.

The lab simply had not been designed with a complete MFA solution for the required authentication scenarios.

The Active Directory setup primarily relied on username and password authentication.

As a result, the environment did not have enough evidence to support a MET determination for the MFA requirement.

SECURITY IMPACT

If the same weakness existed in a real organization, compromised passwords could increase the chance of unauthorized account access.

The risk would be especially important for:

Administrative accounts

Remote access

Network authentication

Privileged functions

Systems containing sensitive information

A compromised privileged account could potentially provide an attacker with much more access than a standard account.

SEVERITY

Priority:

High

The issue was given a high priority because authentication protects access to the rest of the environment.

A weakness at the authentication layer could affect multiple systems and security controls.

RECOMMENDED REMEDIATION

The recommendation was to implement and test a proper multifactor authentication solution that covers all required access situations.

Possible enterprise solutions could include:

Smart card authentication

Certificate-backed authentication

Windows Hello for Business

Another approved enterprise MFA solution

The correct solution would depend on the actual organization's architecture, identity platform, business requirements, security requirements, and supported authentication methods.

REMEDIATION OWNER

Identity Administrator

or

Systems Administrator

CURRENT STATUS

Open

The issue remained open because the project did not include full deployment and validation of a second authentication factor.

POA&M ACTION

The security gap was documented in a Plan of Action and Milestones.

POA&M ID:

POAM-001

Control:

IA.L2-3.5.3

Weakness:

Required MFA coverage was not fully implemented or demonstrated.

Risk:

Compromised passwords could allow unauthorized authentication without an additional authentication factor.

Required Action:

Deploy and validate MFA for all applicable privileged and network authentication scenarios.

Priority:

High

Status:

Open

VALIDATION REQUIREMENTS

The finding should not be closed simply because an MFA product is installed.

Before closing the finding, evidence should demonstrate that MFA actually applies to the required access situations.

Validation evidence should include:

MFA configuration

Authentication policy

Successful MFA authentication

Privileged authentication test

Applicable network authentication test

User or administrative sign-in evidence

Updated control assessment

Updated evidence tracker

Closure documentation

Only after this evidence exists should the control be reassessed.

LESSONS LEARNED

The most important lesson from this case was that the absence of evidence can change an assessment result.

The environment had Active Directory.

It had domain authentication.

It had separate standard and administrative identities.

It had Windows audit logging.

It had Microsoft Defender.

It had Microsoft Defender for Endpoint.

Those technologies did not automatically prove that MFA was satisfied.

I had to evaluate the specific requirement and compare it with the evidence actually available.

Because the evidence only demonstrated username and password authentication, I marked the requirement NOT MET.

This was a better result than forcing the control into a successful status without proof.

COMPLIANCE LESSON

This case also helped me understand the difference between a technical configuration and a compliance conclusion.

Technical configuration answers:

“What technology or security setting exists?”

Compliance assessment asks:

“Does the evidence prove the security requirement?”

Those are not always the same thing.

The MFA case showed why an assessor needs to remain objective.

RISK MANAGEMENT LESSON

Another important lesson was that identifying a weakness is only the beginning.

After identifying the gap, I documented:

The weakness

The risk

The recommended fix

The responsible role

The priority

The current status

The validation method

This turned the finding from a simple observation into something that could actually be tracked and managed.

WHY I DID NOT MARK IT COMPLIANT

I intentionally did not claim that the lab was fully CMMC compliant.

Nine of the ten selected controls were demonstrated sufficiently for the defined home-lab scope.

The MFA requirement remained NOT MET.

Calling the entire environment compliant would have overstated what the evidence supported.

This project was a training simulation, not an official CMMC assessment.

The purpose was to practice the assessment process correctly.

FINAL CASE ASSESSMENT

Finding:

Incomplete Multifactor Authentication Coverage

Control:

IA.L2-3.5.3

Assessment:

NOT MET

Risk:

Unauthorized authentication using compromised credentials

Priority:

High

Owner:

Identity / Systems Administrator

Status:

Open

Recommended Action:

Implement and validate an approved MFA solution across the required privileged and network authentication scenarios.

FINAL CONCLUSION

This case study showed me that compliance work requires more than checking whether security tools exist.

I had to review the security requirement, examine the technical evidence, identify what was missing, understand the risk, recommend remediation, assign responsibility, and document the issue in a POA&M.

The MFA gap was one of the most valuable findings in the entire project because it forced me to make an evidence-based decision instead of trying to make the environment look perfect.

The final result was a documented security weakness with a clear risk and remediation path.