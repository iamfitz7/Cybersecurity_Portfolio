# Case Study — Enterprise Linux Server Administration, Hardening & Secure Remote Access

## Case Overview

This case focused on preparing an Ubuntu Linux server for more controlled and secure administration.

The environment consisted of an Ubuntu server and a Kali Linux administration and testing workstation. The systems maintained normal Internet connectivity while also communicating through a separate private management network.

The main concern was not responding to an active compromise. Instead, the goal was to determine whether a working Linux server could be placed into a more controlled state where administrative access, network exposure, sensitive files, services, and security activity could be clearly understood and validated.

---

## Initial Condition

The starting server needed a clear administrative and security baseline.

Before making changes, several areas required review:

* system and operating system information
* network interfaces and routing
* user and group access
* privileged administration
* filesystem permissions
* installed and running services
* remote administration
* listening network ports
* host firewall configuration
* security logging and auditing
* patch and update status

The main issue was uncertainty.

A system being operational does not automatically answer whether its access controls and network exposure are appropriate.

The first objective was therefore to understand the environment before attempting to improve it.

---

## Environment

The case used two primary virtual machines:

**Ubuntu Linux Server — `linux-srv01`**

The Ubuntu system represented the server being administered and hardened.

**Kali Linux — Administration/Testing Workstation**

Kali represented a separate system used to connect to and validate the Ubuntu server from another host.

The network design separated normal Internet access from private administration traffic. This allowed the server to receive updates while maintaining a dedicated lab network for management and testing.

This design was useful because it allowed controls to be tested externally rather than relying only on observations made from the server itself.

---

## Assessment

I started by establishing a baseline of the Ubuntu server.

I reviewed host identity, operating system information, networking, routing, storage, memory, users, packages, services, and listening sockets.

The purpose was to answer a basic question:

**What is currently running, reachable, and available before I change anything?**

From there, I reviewed access from several perspectives.

For identity, I considered which accounts should represent system administration, security review, web administration, and service activity.

For filesystem access, I examined how ownership, groups, and permission values affected access to sensitive data.

For remote administration, I reviewed whether SSH access was available and how authentication and authorization could be made more restrictive.

For network exposure, I compared locally running services with the services that were actually reachable from another system.

This assessment created a clearer picture of what needed to be protected and what could be reduced.

---

## Key Finding

One of the clearest examples involved sensitive security data.

A test security file was placed into an intentionally unsafe state where its permissions allowed access far beyond what was required. This represented the type of permission problem that could expose or allow modification of sensitive information.

The important finding was not simply that the permission number was incorrect.

The real problem was that the permission state did not match the purpose of the file.

Security investigation information should not be writable by every account on the system.

The file was therefore changed to a more restrictive model where the owner retained required access, the appropriate security group received limited access, and unrelated users received no access.

I then tested the behavior using different accounts.

That validation confirmed that authorization matched the intended roles rather than simply looking correct in the permission listing.

---

## Hardening Actions

Several related controls were applied after the assessment.

### Identity and Privilege Management

Separate users and groups were created around different responsibilities.

Administrative privilege was provided through controlled sudo access rather than treating every account as a full administrator. This supported better separation between normal user activity and privileged actions.

### Filesystem Protection

Sensitive organizational directories were assigned appropriate ownership and group permissions.

Overly broad file access was reduced, and access tests were performed with different accounts to confirm that authorized users could access the required resources while unrelated users could not.

ACLs were also reviewed as a way to provide specific access without permanently changing broader group membership.

### Secure Remote Administration

SSH was used to administer the Ubuntu server from the separate Kali workstation.

After confirming basic connectivity, public-key authentication was configured for the administrative account.

The SSH configuration was then restricted to reduce unnecessary access. Direct root SSH access was disabled, normal password-based SSH authentication was disabled after key authentication was validated, and remote access was limited to the approved administrative group.

The effective SSH configuration was checked after the changes.

### Host Firewall

A host-based firewall was used to control inbound network access.

Instead of allowing services from any source, access was limited based on the private management network and the services that were actually needed.

This reduced unnecessary exposure while keeping required administration available.

### Service Management

Systemd was used to review, start, stop, restart, enable, and inspect services.

This helped connect two different questions:

**Is the service running?**

and

**Should this service be running and reachable?**

Those are not always the same question.

### Logging and Auditing

Linux auditing was configured to watch important areas such as identity files, sudo configuration, SSH configuration, and sensitive security data.

Test activity was generated so that audit records could be searched and reviewed.

SSH and authentication logs were also examined after permitted and restricted access attempts.

This provided evidence that the server was not only enforcing controls but also recording activity that could support later investigation.

### Additional Security Visibility

AppArmor status was reviewed to understand the additional application-level restrictions available beyond normal UNIX file permissions.

The goal was not to force unnecessary policy changes. Instead, the focus was understanding whether the control was active and how mandatory access controls differ from traditional ownership and permission settings.

### Bash Automation

A Bash-based security health check was created to collect useful server information in a repeatable way.

The checks included areas such as system information, resource usage, network state, listening services, firewall status, service health, and security information.

A separate reporting process was also used to create timestamped health reports, and cron was used to demonstrate scheduled collection.

This reduced the need to repeat the same manual checks every time the server needed a basic review.

---

## Validation

The most important part of the case was validating the final state.

I did not treat a saved configuration file as enough evidence that a control worked.

Validation included checking:

* effective SSH security settings
* firewall status
* listening network services
* audit rules
* AppArmor status
* running and failed systemd services
* remote SSH behavior
* file access using different accounts
* external network exposure from Kali
* generated authentication and audit events

Testing from Kali was particularly useful because it provided another perspective.

The Ubuntu server could report that a firewall was active, but an external connection or service scan helped confirm what another system could actually reach.

The same idea applied to authentication. An SSH policy was more meaningful when an approved administrative account could connect while an unauthorized account could not.

---

## Outcome

The final server state was more controlled and easier to understand than the initial state.

Administrative access had a defined path. Remote administration used stronger authentication and authorization controls. Sensitive files had more appropriate permissions. Inbound network access was restricted. Important configuration and security activity had additional logging and auditing. System health and security information could also be collected through repeatable scripts.

The largest improvement was not any single security setting.

It was the ability to answer important questions about the server with evidence:

**Who should have access?**

**What services should be reachable?**

**What privileges should each account have?**

**What happened when a security-related change occurred?**

**How can I verify that the controls actually work?**

---

## Limitations

This case was completed in a controlled virtual environment rather than a production network.

The environment did not include centralized enterprise identity, large-scale server management, production workloads, high availability, centralized log collection, or multiple administrative teams.

The scheduled reporting also represents a basic automation approach. A larger environment would require stronger privilege handling, centralized monitoring, configuration management, alerting, and more formal change control.

These limitations are important because a successful control in a two-system environment does not automatically mean the same design should be copied directly into production.

---

## Lessons Learned

The main lesson from this case was that Linux hardening is not about applying the largest possible number of restrictions.

Each control should have a reason.

Permissions should reflect who actually needs the data. Firewall rules should reflect required network communication. SSH should allow legitimate administration without providing unnecessary authentication options. Logging should collect information that could help explain important activity.

I also learned that verification is one of the most important parts of technical work.

Seeing the expected text in a configuration file is useful, but testing the resulting behavior provides stronger evidence.

That approach helped me move from asking:

**"Did I configure it?"**

to asking:

**"Can I prove it works as intended?"**

---

## Case Conclusion

This case gave me practical experience connecting Linux system administration with security hardening and validation. I worked with identity, permissions, remote administration, firewalling, services, auditing, logging, AppArmor, networking, and Bash automation as related parts of the same system. The strongest lesson was that a secure configuration should be understandable, limited to what is needed, and tested from more than one perspective. The project improved my ability to approach Linux systems by observing first, making controlled changes, and verifying the final behavior.
