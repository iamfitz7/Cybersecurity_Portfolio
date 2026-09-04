Plan of Action and Milestones (POA&M)

## POA&M Overview

This document tracks security weaknesses identified during the simulated NIST SP 800-171 / CMMC Level 2 assessment.

A POA&M is used to record deficiencies that have not yet been fully corrected.

The purpose is to clearly document:

- What is wrong
- Why it matters
- What needs to be done
- Who is responsible
- How important the issue is
- How the fix will be validated
- Whether the issue is still open or has been closed

---

# POAM-001

POA&M ID:
POAM-001

Control ID:
IA.L2-3.5.3

Control Family:
Identification and Authentication

Weakness:
Required multifactor authentication coverage has not been fully implemented or demonstrated for applicable privileged and network authentication scenarios.

Current Condition:
The homelab.local Active Directory environment currently allows domain authentication using a username and password.

A second independent authentication factor was not fully demonstrated for the required access scenarios.

Assessment Result:
NOT MET

Risk:
High

Risk Description:
If an attacker obtains a valid username and password, the attacker could potentially authenticate without needing an additional authentication factor.

This increases the risk of unauthorized system access if credentials are stolen through phishing, password reuse, credential dumping, brute force, or another attack.

Required Action:
Implement and validate an MFA solution that covers applicable privileged and network authentication scenarios.

Possible enterprise approaches may include:

- Smart card authentication
- Certificate-based authentication
- Windows Hello for Business
- Another approved enterprise MFA solution

The final technology should be selected based on the actual environment and authentication requirements.

Owner:
Identity / Systems Administrator

Priority:
High

Current Status:
Open

Target Date:
Future lab improvement

Validation Method:
After MFA is implemented, perform an authentication test and collect evidence showing that a second authentication factor is required.

Validation should include:

- Authentication configuration evidence
- Successful MFA sign-in evidence
- Test of privileged authentication
- Test of applicable network authentication
- Updated control assessment
- Closure evidence

Closure Evidence:
Not available yet.

Reason:
The control has not yet been remediated.

---

## POA&M Status Summary

Total Open Findings:
1

High Priority:
1

Medium Priority:
0

Low Priority:
0

Closed:
0

Current Open Control:
IA.L2-3.5.3 — Multifactor Authentication

---

## Final POA&M Statement

The MFA finding was intentionally left open because the available lab evidence did not demonstrate complete multifactor authentication coverage.

The control was not marked MET simply to make the assessment appear successful.

The weakness was documented with its associated risk, recommended corrective action, responsible owner, priority, validation method, and current status.

This reflects the assessment approach used throughout the project:

Requirement
→ Evidence
→ Assessment
→ Gap
→ Risk
→ Remediation
→ Owner
→ Validation