NIST 800-171 / CMMC Control Evidence Tracker

Assessment Environment:
homelab.local

Assessment Type:
Simulated NIST SP 800-171 / CMMC Level 2 Home Lab Assessment

Assessment Scope:
pfSense, DC01, Active Directory, Windows DNS, Group Policy, Windows 11 Enterprise, Windows Security Logs, Microsoft Defender Antivirus, and Microsoft Defender for Endpoint.

---

## E-001

Control:
AC.L2-3.1.1

Family:
Access Control

Requirement:
Limit system access to authorized users, processes, and devices.

Implementation:
Active Directory was used to centrally identify lab users and the authorized Windows workstation.

Evidence:
Active Directory user objects for lab.user and lab.admin.
Active Directory computer object for the Windows 11 Enterprise workstation.
Windows domain membership.

Assessment Result:
MET

Gap:
No major gap identified within the defined lab scope.

Owner:
Identity Administrator

Status:
Implemented

---

## E-002

Control:
AC.L2-3.1.5

Family:
Access Control

Requirement:
Apply least privilege, including to privileged accounts.

Implementation:
Separate standard and privileged identities were created. lab.user was used as the standard account and lab.admin was used for administrative functions.

Evidence:
lab.user group membership.
lab.admin group membership.

Assessment Result:
MET

Gap:
A real enterprise environment would need additional privileged access management and periodic access reviews.

Owner:
Identity Administrator

Status:
Implemented

---

## E-003

Control:
AU.L2-3.3.1

Family:
Audit and Accountability

Requirement:
Create and retain audit records for monitoring and investigation.

Implementation:
Advanced Windows auditing was configured using the CMMC-Audit-Policy Group Policy Object.

Evidence:
CMMC-Audit-Policy configuration.
gpupdate /force.
gpresult output.
auditpol /get /category:* output.

Assessment Result:
MET

Gap:
No centralized enterprise log retention system is deployed in the home lab.

Remediation:
Forward important security logs to a protected centralized logging or SIEM platform in a production environment.

Owner:
Security / Systems Administrator

Status:
Implemented in lab

---

## E-004

Control:
AU.L2-3.3.2

Family:
Audit and Accountability

Requirement:
Make user activity traceable to individual users.

Implementation:
Domain user activity was generated and investigated through Windows Security logging.

Evidence:
Event ID 4624 successful logon.
Event ID 4625 failed logon.
Event ID 4688 process creation.

Assessment Result:
MET

Gap:
The home lab does not provide enterprise-wide log correlation.

Remediation:
Use centralized security monitoring in a production environment.

Owner:
Security Operations

Status:
Implemented

---

## E-005

Control:
IA.L2-3.5.1

Family:
Identification and Authentication

Requirement:
Identify users and devices before access is granted.

Implementation:
Active Directory users and computer objects were used to identify authorized users and the managed Windows workstation.

Evidence:
lab.user.
lab.admin.
Windows workstation domain computer object.
HOMELAB\lab.user login.

Assessment Result:
MET

Gap:
None identified within the tested scope.

Owner:
Identity Administrator

Status:
Implemented

---

## E-006

Control:
IA.L2-3.5.3

Family:
Identification and Authentication

Requirement:
Use multifactor authentication for applicable privileged and network access.

Implementation:
The lab currently uses username and password authentication for Active Directory domain access.

Evidence:
Domain authentication was demonstrated using a username and password.
A complete second authentication factor was not demonstrated.

Assessment Result:
NOT MET

Gap:
Required MFA coverage has not been demonstrated for applicable privileged and network authentication scenarios.

Risk:
A stolen or guessed password could permit unauthorized authentication without a second authentication factor.

Remediation:
Implement and validate an appropriate MFA solution covering the required authentication scenarios.

Owner:
Identity / Systems Administrator

Status:
Open

---

## E-007

Control:
SC.L2-3.13.1

Family:
System and Communications Protection

Requirement:
Monitor and protect communications at system boundaries.

Implementation:
pfSense was used as the firewall and router protecting the isolated CMMC lab network.

Evidence:
pfSense WAN/LAN configuration.
pfSense LAN address 10.20.30.1.
pfSense firewall rules.
pfSense firewall logs.

Assessment Result:
MET

Gap:
Enterprise-level segmentation and centralized firewall management are not present.

Remediation:
Regularly review firewall rules and monitor permitted and blocked traffic.

Owner:
Network / Security Administrator

Status:
Implemented in lab

---

## E-008

Control:
SI.L2-3.14.1

Family:
System and Information Integrity

Requirement:
Identify and remediate system flaws.

Implementation:
Microsoft security tools and Windows security information were used to review endpoint security findings and update condition.

Evidence:
Microsoft Defender for Endpoint or Windows security recommendation, vulnerability, or update evidence collected during the lab.

Assessment Result:
MET for demonstrated lab process

Gap:
No formal enterprise remediation SLA or ticketing workflow exists in the home lab.

Remediation:
Assign findings an owner, priority, remediation deadline, and post-remediation validation.

Owner:
Security / Systems Administrator

Status:
Implemented for training

---

## E-009

Control:
SI.L2-3.14.2

Family:
System and Information Integrity

Requirement:
Provide malicious-code protection.

Implementation:
Microsoft Defender Antivirus and Microsoft Defender for Endpoint were enabled on the Windows 11 Enterprise endpoint.

Evidence:
AntivirusEnabled = True
RealTimeProtectionEnabled = True
BehaviorMonitorEnabled = True
IoavProtectionEnabled = True
AntispywareEnabled = True

Microsoft Defender for Endpoint Sense service:
STATE = RUNNING

Assessment Result:
MET

Gap:
The project covers one primary endpoint rather than an enterprise fleet.

Owner:
Security / Endpoint Administrator

Status:
Implemented

---

## E-010

Control:
SI.L2-3.14.6

Family:
System and Information Integrity

Requirement:
Monitor systems and network communications for attacks or indicators of attack.

Implementation:
Windows Event Viewer, Microsoft Defender for Endpoint, and pfSense provided endpoint and network monitoring.

Evidence:
Windows Security Logs.
Event IDs 4624, 4625, and 4688.
Microsoft Defender security status.
MDE Sense service.
pfSense firewall logging.

Assessment Result:
MET

Gap:
No full enterprise SIEM or 24/7 SOC monitoring is implemented.

Remediation:
Forward security events into a centralized monitoring and correlation platform in a production environment.

Owner:
Security Operations

Status:
Implemented for lab demonstration

---

# Assessment Summary

Total Controls Assessed:
10

MET:
9

NOT MET:
1

Open Finding:
IA.L2-3.5.3 — Multifactor Authentication

Overall Conclusion:
The lab successfully demonstrated technical implementation and evidence collection across access control, identity, auditing, boundary protection, system integrity, malware protection, and monitoring.

The environment should not be described as fully CMMC compliant.

The assessment identified a legitimate MFA gap that requires remediation.