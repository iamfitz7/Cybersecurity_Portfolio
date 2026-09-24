# 🐧 Linux Systems Administration, Security Hardening & Secure Application Operations

![Linux](https://img.shields.io/badge/Linux-Ubuntu-E95420?logo=ubuntu&logoColor=white)
![Bash](https://img.shields.io/badge/Scripting-Bash-4EAA25?logo=gnubash&logoColor=white)
![Python](https://img.shields.io/badge/Python-FastAPI-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/API-FastAPI-009688?logo=fastapi&logoColor=white)
![Nginx](https://img.shields.io/badge/Web-Nginx-009639?logo=nginx&logoColor=white)
![SSH](https://img.shields.io/badge/Remote%20Access-OpenSSH-black)
![Firewall](https://img.shields.io/badge/Firewall-UFW-red)
![Auditd](https://img.shields.io/badge/Auditing-Auditd-purple)
![TLS](https://img.shields.io/badge/Encryption-TLS-blue)
![VirtualBox](https://img.shields.io/badge/Virtualization-VirtualBox-183A61?logo=virtualbox&logoColor=white)

## Enterprise Linux Administration → Security Hardening → Secure Application Deployment → Failure Recovery

This repository documents a multi-stage hands-on Linux engineering and security project completed in a controlled virtual lab.

The project progresses through two major environments:

**Week 21 — Lab 1: Enterprise Linux Server Administration, Hardening & Secure Remote Access**

**Week 21 — Lab 2: Secure FastAPI Production Web Application Deployment & Recovery**

Together, the labs demonstrate practical experience with:

- Linux system administration
- Linux networking
- Identity and access management
- Least privilege
- Filesystem permissions and ACLs
- SSH administration and hardening
- Public-key authentication
- Host-based firewall configuration
- Network exposure validation
- Linux auditing and logging
- AppArmor
- Bash security automation
- Cron scheduling
- Python application deployment
- FastAPI and Uvicorn
- systemd service engineering
- Nginx reverse proxying
- HTTPS/TLS
- Application health monitoring
- Controlled failure simulation
- Log-based troubleshooting
- Automatic service recovery
- Post-reboot persistence validation

The goal was not simply to execute Linux commands.

The larger goal was to understand how a Linux system moves from:

```text
Basic Operating System
        |
        v
Configured Server
        |
        v
Hardened Server
        |
        v
Monitored Server
        |
        v
Automated Server
        |
        v
Application Host
        |
        v
Secure Service Architecture
        |
        v
Failure-Resilient Environment
```

A major theme throughout both labs was **evidence-based validation**.

I did not treat a configuration as successful simply because a command completed without an error.

I repeatedly compared:

```text
Intended Configuration
        |
        v
Effective Configuration
        |
        v
Actual System State
        |
        v
Real Access / Network Test
        |
        v
Logs & Evidence
        |
        v
Validated Result
```

---

# 📌 Executive Summary

This project demonstrates my ability to build and operate a Linux environment from the operating-system layer through the application layer.

In the first lab, I built and hardened an Ubuntu Linux server and a separate Kali Linux administration workstation. I implemented role-based identities, least-privilege sudo access, filesystem permissions, ACLs, ED25519 SSH authentication, SSH hardening, UFW firewall rules, Auditd monitoring, AppArmor validation, Bash security automation, scheduled reporting, and external service validation.

In the second lab, I used those Linux administration foundations to deploy and operate a FastAPI application. The application ran through Uvicorn under a dedicated `webapp` service account, was managed by systemd, proxied through Nginx, protected with UFW, served over HTTPS, and restricted so the backend listened only on localhost.

I then intentionally terminated the application's main Uvicorn process with `SIGKILL`.

Instead of manually restarting it, I investigated what happened through systemd and journald.

The service automatically recovered because of its configured restart policy. A replacement Uvicorn process started, and the `/health` endpoint returned successfully afterward.

This provided practical experience across the full lifecycle:

```text
Build
  |
  v
Configure
  |
  v
Harden
  |
  v
Deploy
  |
  v
Validate
  |
  v
Monitor
  |
  v
Break
  |
  v
Investigate
  |
  v
Recover
  |
  v
Validate Again
```

---

# 🧠 What This Repository Demonstrates

From a technical perspective, this project demonstrates experience across four closely related areas.

| Area | Demonstrated Experience |
|---|---|
| Linux Administration | Users, groups, permissions, packages, processes, services, networking, systemd |
| Linux Security | Least privilege, SSH hardening, UFW, ACLs, Auditd, AppArmor, secure service accounts |
| Application Operations | FastAPI, Uvicorn, Nginx, TLS, reverse proxying, health checks |
| Troubleshooting & Reliability | Logs, process analysis, port analysis, failure testing, automatic recovery |

---

# 🗺️ Project Progression

```text
WEEK 21 — LAB 1
Enterprise Linux Administration & Hardening
            |
            |
            +--> System Baseline
            |
            +--> Private Management Network
            |
            +--> Users & Groups
            |
            +--> Least Privilege
            |
            +--> Filesystem Security
            |
            +--> SSH Hardening
            |
            +--> UFW Firewall
            |
            +--> External Nmap Validation
            |
            +--> Auditd
            |
            +--> AppArmor
            |
            +--> Bash Automation
            |
            +--> Cron Reporting
            |
            v

WEEK 21 — LAB 2
Secure Application Operations
            |
            |
            +--> FastAPI
            |
            +--> Uvicorn
            |
            +--> Dedicated Service Account
            |
            +--> systemd
            |
            +--> Nginx
            |
            +--> HTTPS/TLS
            |
            +--> UFW
            |
            +--> Health Monitoring
            |
            +--> Failure Simulation
            |
            +--> Log Investigation
            |
            +--> Automatic Recovery
            |
            v

     SECURE LINUX APPLICATION ENVIRONMENT
```

---

# 🧰 Core Technologies

| Technology | Purpose |
|---|---|
| Ubuntu Linux | Server operating system |
| Kali Linux | Administrative and validation workstation |
| Oracle VirtualBox | Virtualized infrastructure |
| Bash | Linux administration and automation |
| Python | Application runtime |
| FastAPI | Web API framework |
| Uvicorn | ASGI application server |
| systemd | Service management and recovery |
| Nginx | Reverse proxy and HTTPS entry point |
| OpenSSH | Secure remote administration |
| ED25519 | SSH public-key authentication |
| UFW | Host-based firewall |
| Auditd | Security auditing |
| AppArmor | Mandatory access control |
| OpenSSL | TLS certificate generation and inspection |
| TLS | HTTPS encryption |
| Nmap | External service exposure validation |
| ACLs | Granular filesystem authorization |
| Cron | Scheduled automation |
| journald / `journalctl` | System and service logging |
| `ss` | Listening socket inspection |
| `curl` | HTTP/HTTPS validation |
| Netplan | Ubuntu networking |
| NetworkManager | Kali networking |
| APT / DPKG | Package management |

---

# ============================================================
# 🐧 WEEK 21 — LAB 1
# Enterprise Linux Server Administration, Hardening & Secure Remote Access
# ============================================================

# 📌 Lab Overview

The first phase of the project focused on building, administering, hardening, monitoring, and validating an Ubuntu Linux server.

I created two Linux virtual machines:

| System | Role | Management Address |
|---|---|---|
| `linux-srv01` | Ubuntu application/server environment | `192.168.50.10` |
| `kali-admin01` | Administration and security validation | `192.168.50.20` |

The systems used a dedicated VirtualBox internal network:

```text
192.168.50.0/24
```

while maintaining separate NAT interfaces for Internet connectivity.

This created a simple enterprise-style separation between:

```text
Internet / Package Access
        |
        v
VirtualBox NAT


Private Administration
        |
        v
LinuxLab 192.168.50.0/24
```

The goal was to learn not only **how to configure Linux**, but also how to verify that security controls actually behaved as expected.

---

# 🎯 Lab 1 Objectives

The main objectives were to:

1. Establish a documented Linux baseline.
2. Configure a private management network.
3. Create role-based Linux identities.
4. Implement least-privilege administration.
5. Harden filesystem permissions.
6. Practice ACL-based authorization.
7. Manage packages and services.
8. Configure secure SSH administration.
9. Implement ED25519 authentication.
10. Harden the OpenSSH server.
11. Restrict remote administration.
12. Configure a default-deny firewall.
13. Validate exposure externally.
14. Review automatic security updates.
15. Implement Auditd monitoring.
16. Investigate authentication activity.
17. Validate AppArmor.
18. Practice process and network troubleshooting.
19. Automate security checks with Bash.
20. Schedule security reporting.
21. Perform a final attack-surface review.

---

# 🌐 Lab 1 Network Architecture

```text
                           INTERNET
                              |
                     +--------+--------+
                     | VirtualBox NAT |
                     +--------+--------+
                              |
               +--------------+--------------+
               |                             |
               v                             v
        Ubuntu Server                    Kali Linux
         linux-srv01                    kali-admin01
               |                             |
       192.168.50.10                  192.168.50.20
               |                             |
               +--------------+--------------+
                              |
                              v
                    LinuxLab Internal Network
                       192.168.50.0/24
```

The NAT interfaces provided:

- Operating-system updates
- Package installation
- Internet connectivity

The private `LinuxLab` interfaces provided:

- SSH administration
- Connectivity testing
- Firewall testing
- Nmap validation
- Security testing

This separated private administrative traffic from normal Internet connectivity.

---

# 1️⃣ System Baseline & Host Configuration

Before hardening the server, I documented its starting state.

Commands included:

```bash
whoami
hostnamectl
cat /etc/os-release
uname -a
ip -br address
ip route
lsblk
df -h
free -h
```

This established information about:

- Current user
- Hostname
- Linux distribution
- Kernel
- Network interfaces
- Routes
- Storage
- Filesystem usage
- Memory

The Ubuntu server was assigned the hostname:

```text
linux-srv01
```

using:

```bash
sudo hostnamectl set-hostname linux-srv01
```

This established one of the most important workflows used throughout the project:

```text
Observe
   |
   v
Change
   |
   v
Verify
```

---

# 2️⃣ Private Management Network

A dedicated VirtualBox internal network named:

```text
LinuxLab
```

was configured.

Addressing:

```text
Network: 192.168.50.0/24

Ubuntu:   192.168.50.10
Kali:     192.168.50.20
```

The private interface did not receive another default gateway.

That allowed Internet traffic to continue using the NAT adapter while administrative traffic used the private network.

Connectivity was tested from Kali:

```bash
ping -c 4 192.168.50.10
```

and from Ubuntu:

```bash
ping -c 4 192.168.50.20
```

Successful two-way communication established the management path before additional security controls were introduced.

---

# 3️⃣ Role-Based Identity Administration

Instead of treating every Linux account the same, I created identities representing different responsibilities.

## Groups

```text
linux-admins
security-team
web-team
backup-team
```

## Users

| User | Primary Responsibility |
|---|---|
| `linuxadmin` | Linux administration |
| `securityanalyst` | Security information access |
| `webadmin` | Web administration |
| `backupsvc` | Backup operations |

Example creation:

```bash
sudo groupadd linux-admins
sudo groupadd security-team
sudo groupadd web-team
sudo groupadd backup-team
```

```bash
sudo useradd -m -s /bin/bash linuxadmin
sudo useradd -m -s /bin/bash securityanalyst
sudo useradd -m -s /bin/bash webadmin
sudo useradd -m -s /bin/bash backupsvc
```

Identity and membership were verified using:

```bash
id linuxadmin
id securityanalyst
id webadmin
id backupsvc
```

The purpose was to create an authorization model based on responsibility instead of giving every account broad access.

---

# 4️⃣ Least-Privilege Sudo Administration

Administrative privilege was limited to the:

```text
linux-admins
```

group.

A dedicated sudoers configuration was created:

```text
/etc/sudoers.d/linux-admins
```

containing:

```text
%linux-admins ALL=(ALL:ALL) ALL
```

The file was protected:

```bash
sudo chmod 440 /etc/sudoers.d/linux-admins
```

Before relying on the configuration, I validated sudoers syntax:

```bash
sudo visudo -c
```

I then tested privilege elevation through:

```bash
sudo whoami
```

Expected output:

```text
root
```

The key security concept was that the user normally operates as:

```text
linuxadmin
```

and elevates privileges only when necessary.

---

# 5️⃣ Filesystem Security & Least Privilege

A simulated organizational structure was created:

```text
/srv/company/
├── security/
├── web/
└── backups/
```

Group ownership reflected functional responsibilities:

```bash
sudo chown root:security-team /srv/company/security
sudo chown root:web-team /srv/company/web
sudo chown root:backup-team /srv/company/backups
```

Shared directories used:

```bash
chmod 2770
```

The `2` enabled the setgid bit so files created inside the directory could inherit its group.

---

## 🚨 Deliberately Insecure State

A security-related test file was intentionally assigned:

```text
777
rwxrwxrwx
```

This represented an overly permissive configuration.

Every user could potentially:

```text
Read
Write
Execute
```

the file.

---

## 🔒 Hardened State

The file was remediated:

```bash
sudo chown root:security-team \
/srv/company/security/security-investigation.txt

sudo chmod 640 \
/srv/company/security/security-investigation.txt
```

Final state:

```text
640
rw-r-----
```

| Identity | Access |
|---|---|
| Owner | Read + Write |
| `security-team` | Read |
| Others | None |

---

# 6️⃣ Permission Validation

The permissions were tested using real accounts.

The `securityanalyst` account could read the file because it belonged to:

```text
security-team
```

A modification attempt was denied because the group received read-only permission.

The `webadmin` account was unable to read the security file under the standard permission model.

This demonstrated the difference between:

```text
Configured Permission
```

and:

```text
Tested Authorization Behavior
```

The second provides stronger evidence.

---

# 7️⃣ Granular Authorization with ACLs

Access Control Lists were used to provide a temporary user-specific exception.

Existing ACL:

```bash
getfacl /srv/company/security/security-investigation.txt
```

Temporary read access:

```bash
sudo setfacl -m \
u:webadmin:r \
/srv/company/security/security-investigation.txt
```

The access was tested.

After validation, the exception was removed:

```bash
sudo setfacl -x \
u:webadmin \
/srv/company/security/security-investigation.txt
```

This demonstrated how ACLs can extend normal owner/group/other permissions without restructuring the primary group model.

---

# 8️⃣ Package & Software Administration

APT and DPKG were used for package management and software inventory.

Examples:

```bash
sudo apt update
sudo apt upgrade
apt search nginx
sudo apt install nginx
apt show nginx
dpkg -l | grep nginx
```

Security-related packages were also reviewed:

```bash
dpkg -l | grep -E \
'nginx|openssh|audit|apparmor|ufw'
```

Package inventory matters because installed software contributes to the system's attack surface and maintenance requirements.

---

# 9️⃣ Service Administration with systemd

Nginx was used to practice Linux service administration.

```bash
systemctl status nginx
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
systemctl is-enabled nginx
```

Logs:

```bash
journalctl -u nginx
```

Listening sockets:

```bash
sudo ss -tulpn
```

This reinforced that several different questions exist:

```text
Is the software installed?

Is the service running?

Is it enabled at boot?

Is it listening?

Is the firewall allowing it?

Can another host actually reach it?
```

Those questions are related, but they are not interchangeable.

---

# 🔑 1️⃣0️⃣ Secure Remote Administration

OpenSSH Server was configured on Ubuntu.

Initial connection from Kali:

```bash
ssh linuxadmin@192.168.50.10
```

Inside the remote session:

```bash
whoami
hostname
```

Expected:

```text
linuxadmin
linux-srv01
```

This established a working remote-management baseline before SSH hardening.

---

# 🔐 1️⃣1️⃣ ED25519 Authentication

An ED25519 key pair was created on Kali:

```bash
ssh-keygen -t ed25519
```

The public key was installed on Ubuntu:

```bash
ssh-copy-id linuxadmin@192.168.50.10
```

Key-based authentication was tested successfully **before password authentication was disabled**.

This order reduced the chance of locking myself out of the server.

> The SSH private key was never added to the repository.

---

# 🛡️ 1️⃣2️⃣ OpenSSH Hardening

A dedicated hardening file was used:

```text
/etc/ssh/sshd_config.d/99-lab-hardening.conf
```

Important settings included:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
MaxAuthTries 3
LoginGraceTime 30
AllowGroups linux-admins
X11Forwarding no
```

These settings:

- Disabled direct root SSH access
- Required public-key authentication
- Disabled SSH passwords
- Reduced authentication attempts
- Reduced login grace time
- Restricted SSH to authorized administrators
- Disabled unnecessary X11 forwarding

Syntax validation:

```bash
sudo sshd -t
```

Effective configuration:

```bash
sudo sshd -T | grep -E \
'permitrootlogin|pubkeyauthentication|passwordauthentication|maxauthtries|logingracetime|allowgroups|x11forwarding'
```

I also kept an existing SSH connection open while validating a second session.

That provided a recovery path if the new configuration caused a problem.

---

# 🚫 1️⃣3️⃣ Unauthorized SSH Test

`webadmin` was a valid Linux account.

However, it was not a member of:

```text
linux-admins
```

Because OpenSSH used:

```text
AllowGroups linux-admins
```

the account was denied remote SSH access.

This demonstrated:

```text
Valid Operating-System Account
             ≠
Authorized Remote Administrator
```

The final administrative path became:

```text
Kali Workstation
      |
      v
Private LinuxLab Network
      |
      v
UFW
      |
      v
OpenSSH
      |
      v
ED25519 Authentication
      |
      v
AllowGroups linux-admins
      |
      v
linuxadmin
      |
      v
sudo when required
```

---

# 🔥 1️⃣4️⃣ UFW Firewall Hardening

UFW was configured with:

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

SSH was restricted to the private network:

```bash
sudo ufw allow from 192.168.50.0/24 \
to any port 22 proto tcp
```

The firewall was enabled:

```bash
sudo ufw enable
```

Final state:

```bash
sudo ufw status verbose
```

The strategy was:

```text
Incoming Traffic
       |
       v
DENY by Default
       |
       +---- Explicitly Required Traffic
       |
       +---- Everything Else Denied
```

---

# 🔍 1️⃣5️⃣ External Exposure Validation with Nmap

Local configuration was not treated as proof of remote exposure.

From Kali:

```bash
nmap -sT 192.168.50.10
```

This allowed comparison between:

```text
LOCAL VIEW
ss -tulpn
```

and:

```text
REMOTE VIEW
Nmap
```

The purpose was to answer:

> **What can another system actually reach?**

This distinction is important because a running service does not automatically mean the service is reachable from another host.

---

# 🔄 1️⃣6️⃣ Automatic Security Update Review

Ubuntu's update mechanisms were inspected:

```bash
dpkg -l | grep unattended-upgrades

cat /etc/apt/apt.conf.d/20auto-upgrades

systemctl status apt-daily.timer

systemctl status apt-daily-upgrade.timer
```

This connected hardening with ongoing maintenance.

Security is not only an initial configuration activity; systems also need mechanisms for continued maintenance.

---

# 👁️ 1️⃣7️⃣ Auditd Security Monitoring

Auditd was configured to monitor sensitive resources.

Persistent rules included:

```text
-w /etc/passwd -p wa -k identity_changes
-w /etc/group -p wa -k identity_changes
-w /etc/shadow -p wa -k credential_changes
-w /etc/sudoers -p wa -k sudo_changes
-w /etc/sudoers.d/ -p wa -k sudo_changes
-w /etc/ssh/sshd_config -p wa -k ssh_config_changes
-w /etc/ssh/sshd_config.d/ -p wa -k ssh_config_changes
-w /srv/company/security/ -p wa -k security_data_changes
```

These monitored:

- Identity changes
- Group changes
- Credential-related files
- Sudo configuration
- SSH configuration
- Sensitive security data

Rules were loaded:

```bash
sudo augenrules --load
```

and verified:

```bash
sudo auditctl -l
```

---

# 🕵️ 1️⃣8️⃣ Audit Event Investigation

A controlled event was created:

```bash
sudo touch /srv/company/security/audit-test.txt
```

The corresponding audit data was investigated:

```bash
sudo ausearch -k security_data_changes -i
```

Summary information:

```bash
sudo aureport
```

This demonstrated:

```text
Sensitive Resource
       |
       v
Activity
       |
       v
Auditd
       |
       v
Audit Record
       |
       v
ausearch / aureport
       |
       v
Investigation
```

---

# 📜 1️⃣9️⃣ Authentication & Privileged Activity

SSH events were reviewed:

```bash
sudo journalctl -u ssh --since today
```

A controlled failed SSH attempt was generated from Kali using an account that was not authorized for remote access.

The resulting activity was reviewed for:

- Username
- Source system
- Timestamp
- Authentication result
- Related SSH activity

Privileged sudo activity was also reviewed.

This connected normal Linux administration with the evidence required for security investigation.

---

# 🧱 2️⃣0️⃣ AppArmor Validation

AppArmor was reviewed using:

```bash
sudo aa-status
```

This confirmed that the framework was loaded and profiles were being enforced.

Traditional Linux permissions answer:

```text
Can this USER access this resource?
```

AppArmor adds another question:

```text
Can this APPLICATION access this resource?
```

This introduced another layer of Linux access control.

---

# ⚙️ 2️⃣1️⃣ Process & Network Administration

Process inspection included:

```bash
ps aux
ps aux --sort=-%cpu | head
ps aux --sort=-%mem | head
pgrep -a nginx
pstree -p
top
```

Network inspection included:

```bash
ip -br address
ip route
ss -tulpn
resolvectl status
ping
getent hosts
```

Instead of memorizing commands in isolation, I used them to answer operational questions:

```text
What is running?

Who owns it?

What is consuming CPU?

What is consuming memory?

What process owns this port?

What interface has this address?

Where will traffic go?

Is DNS working?

Can another host reach this system?
```

---

# 🤖 2️⃣2️⃣ Bash Security Health Check

I built a Bash script to automate recurring security and administration checks.

The script gathered:

- Hostname
- System information
- Filesystem utilization
- Memory utilization
- IP addresses
- Listening sockets
- UFW status
- Failed systemd units
- SSH state
- Nginx state
- AppArmor state
- Recent failed authentication activity

The script used:

- Variables
- Command substitution
- Pipes
- `grep`
- `tail`
- Redirection
- Linux administration commands

Syntax was validated:

```bash
bash -n ~/scripts/security-health-check.sh
```

Execution permission was restricted:

```bash
chmod 750 ~/scripts/security-health-check.sh
```

The script converted multiple manual checks into a repeatable workflow.

---

# 📊 2️⃣3️⃣ Automated Security Reporting

A second Bash script created timestamped reports containing:

```text
System Information
Users with Login Shells
Administrative Group Information
Listening Services
Firewall Status
Failed Services
Disk Usage
Recent SSH Events
```

Report naming:

```text
linux-security-report-YYYYMMDD-HHMMSS.txt
```

This created historical security evidence that could be reviewed later.

---

# ⏰ 2️⃣4️⃣ Scheduled Reporting

Reports were stored under:

```text
~/admin-reports
```

The lab cron schedule was:

```text
*/15 * * * * /home/fitzgerald/scripts/scheduled-health-report.sh >> /home/fitzgerald/admin-reports/cron.log 2>&1
```

The 15-minute frequency was used for lab validation.

Cron configuration:

```bash
crontab -l
```

Service status:

```bash
systemctl is-active cron
```

Generated reports were inspected to prove that the scheduled job actually executed.

---

# 🔎 2️⃣5️⃣ Final Security Assessment

The final review included:

```bash
sudo ufw status verbose
sudo sshd -t
sudo sshd -T
sudo auditctl -l
sudo aa-status
sudo ss -tulpn
systemctl is-active ssh.socket
systemctl is-active nginx
systemctl is-active cron
systemctl --failed --no-pager
```

The assessment reviewed:

- Firewall state
- Effective SSH policy
- Audit rules
- AppArmor
- Listening services
- SSH availability
- Nginx
- Cron
- Failed systemd services

Remote validation was also performed from Kali.

---

# ✅ Lab 1 Final Security Posture

| Security Area | Final State |
|---|---|
| Hostname | `linux-srv01` |
| Private Network | `192.168.50.0/24` |
| Server IP | `192.168.50.10` |
| Admin Workstation | `192.168.50.20` |
| Internet Connectivity | Separate NAT interface |
| Identity | Role-based users/groups |
| Privilege | Controlled sudo |
| Sensitive File Access | Least privilege |
| Shared Directories | `2770` + setgid |
| ACLs | Tested |
| SSH Authentication | ED25519 |
| Password SSH | Disabled |
| Root SSH | Disabled |
| SSH Authorization | `linux-admins` |
| Host Firewall | UFW |
| Incoming Policy | Deny by default |
| Management Access | Private network |
| Auditing | Auditd |
| MAC | AppArmor |
| Automation | Bash |
| Reporting | Automated |
| Scheduling | Cron |
| Remote Validation | Kali + Nmap |

---

# 🧪 Lab 1 Validation Matrix

| Control | Configuration | Validation |
|---|---|---|
| Networking | Netplan / NetworkManager | `ip`, `ping`, routes |
| Identities | Users & groups | `id`, `getent` |
| Privilege | sudoers | `visudo -c`, access test |
| Permissions | chmod/chown | Actual user tests |
| ACL | `setfacl` | `getfacl`, user test |
| SSH | sshd config | `sshd -t`, `sshd -T`, remote test |
| SSH Authorization | `AllowGroups` | Unauthorized login test |
| Firewall | UFW | UFW + Nmap |
| Exposure | Services | `ss` + Nmap |
| Auditd | Audit rules | Test event + `ausearch` |
| AppArmor | Profiles | `aa-status` |
| Automation | Bash | Syntax + execution |
| Scheduling | Cron | Generated reports |

---

# 🚧 Lab 1 Troubleshooting Experience

The environment did not work perfectly on every first attempt.

Issues I worked through included:

- Identifying correct network interface names
- Separating NAT and management traffic
- Preventing an unwanted second default route
- Testing SSH keys before disabling passwords
- Maintaining an existing SSH session during hardening
- Confirming effective SSH settings
- Testing firewall behavior remotely
- Validating permissions with actual users
- Identifying processes behind listening ports
- Validating Bash syntax
- Correcting script paths and permissions
- Creating required reporting directories
- Correcting scheduled-reporting behavior
- Confirming cron generated actual reports
- Understanding SSH socket activation

These problems were useful because troubleshooting required understanding the system rather than only following a command sequence.

---

# 🏁 Week 21 Lab 1 Outcome

By the end of the lab, the environment had progressed through:

```text
Baseline
   |
   v
Networking
   |
   v
Identity
   |
   v
Least Privilege
   |
   v
Filesystem Security
   |
   v
Secure Remote Administration
   |
   v
Firewall Hardening
   |
   v
Logging & Auditing
   |
   v
Application Restrictions
   |
   v
Automation
   |
   v
External Validation
   |
   v
Final Security Assessment
```

The lab established the Linux administration and security foundation required for the next stage.

---

# ============================================================
# 🔐 WEEK 21 — LAB 2
# Secure FastAPI Production Web Application Deployment & Recovery
# ============================================================

# 📌 Lab Overview

The second phase moved from general Linux server hardening into **secure application operations**.

I deployed a FastAPI application and built the Linux infrastructure required to manage it.

The service stack became:

```text
FastAPI
   |
   v
Uvicorn
   |
   v
systemd
   |
   v
Nginx
   |
   v
TLS
   |
   v
UFW
```

The application was deliberately designed so Uvicorn was **not directly exposed to the network**.

Instead:

```text
Client
   |
   v
Nginx :443
   |
   v
127.0.0.1:8000
   |
   v
Uvicorn
   |
   v
FastAPI
```

I also tested what happened when the application's main process unexpectedly died.

systemd detected the failure and automatically recovered the application.

---

# 🎯 Lab 2 Objectives

The objectives were to:

1. Deploy FastAPI on Ubuntu.
2. Run the application through Uvicorn.
3. Create a dedicated application identity.
4. Avoid running the application as root.
5. Manage the application through systemd.
6. Configure automatic recovery.
7. Place Nginx in front of Uvicorn.
8. Keep the backend off the external network.
9. Configure HTTPS.
10. Redirect HTTP to HTTPS.
11. Protect the host with UFW.
12. Validate listening sockets.
13. Implement an application health endpoint.
14. Review service and web logs.
15. Simulate process failure.
16. Investigate failure evidence.
17. Verify automatic recovery.
18. Confirm application-level recovery.
19. Verify service persistence after reboot.

---

# 🏗️ Lab 2 Environment

| Component | Configuration |
|---|---|
| Server | Ubuntu Linux |
| Hostname | `linux-web01` |
| Application | FastAPI |
| Application Server | Uvicorn |
| Application Directory | `/opt/webapp` |
| Service Account | `webapp` |
| Service Manager | systemd |
| Reverse Proxy | Nginx |
| Backend | `127.0.0.1:8000` |
| HTTP | `80` |
| HTTPS | `443` |
| Firewall | UFW |
| Lab Network | `192.168.50.0/24` |
| TLS | Self-signed lab certificate |

---

# 🌐 Application Architecture

```text
                    CLIENT / LAB NETWORK
                              |
                   +----------+----------+
                   |                     |
                   v                     v
                HTTP :80              HTTPS :443
                   |                     |
                   |                Encrypted Traffic
                   |                     |
                   +----------+----------+
                              |
                              v
                       +-------------+
                       |    NGINX    |
                       |-------------|
                       | Redirect    |
                       | TLS         |
                       | Reverse     |
                       | Proxy       |
                       +------+------+
                              |
                              v
                       127.0.0.1:8000
                              |
                              v
                       +-------------+
                       |   Uvicorn   |
                       +------+------+
                              |
                              v
                       +-------------+
                       |   FastAPI   |
                       +------+------+
                              |
                      +-------+-------+
                      |               |
                      v               v
                     `/`          `/health`
```

Supporting controls:

```text
                       linux-web01
                            |
        +-------------------+-------------------+
        |                   |                   |
        v                   v                   v
     systemd               UFW               journald
        |                   |                   |
        v                   v                   v
 Service Lifecycle     Network Policy       Event Evidence
 Auto-Recovery         Default Deny         Failure Logs
 Boot Persistence      Port Control         Recovery Logs
 Hardening             Lab Restrictions     Troubleshooting
```

---

# 🐍 FastAPI Application

The application returned basic service information.

Example:

```json
{
  "status": "online",
  "server": "linux-web01",
  "service": "Week 21 Lab 2 Production Web Application"
}
```

A separate endpoint was created:

```text
/health
```

Successful response:

```json
{
  "status": "healthy"
}
```

This was important because:

```text
Process Exists
      ≠
Application Is Healthy
```

The health endpoint allowed validation at the application layer.

---

# 👤 Dedicated `webapp` Service Identity

The application ran as:

```text
webapp
```

rather than:

```text
root
```

or my normal Linux user.

Verification:

```bash
id webapp
```

Application directory:

```text
/opt/webapp
```

Process ownership was also inspected to confirm that Uvicorn actually ran as the intended service identity.

This applied least privilege to an application service, not just to human users.

---

# ⚙️ systemd Application Service

The application was managed through:

```text
webapp.service
```

Configuration:

```ini
[Unit]
Description=Week 21 Lab 2 FastAPI Production Web Application
After=network.target

[Service]
Type=simple
User=webapp
Group=webapp
WorkingDirectory=/opt/webapp
ExecStart=/opt/webapp/venv/bin/uvicorn app:app --host 127.0.0.1 --port 8000
Restart=on-failure
RestartSec=5
NoNewPrivileges=true
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

---

## Least-Privilege Identity

```ini
User=webapp
Group=webapp
```

The application did not require root for normal operation.

---

## Localhost Backend

```text
--host 127.0.0.1 --port 8000
```

Uvicorn listened only on the loopback interface.

This reduced direct backend exposure.

---

## Automatic Recovery

```ini
Restart=on-failure
RestartSec=5
```

systemd was configured to recover the application after an unexpected process failure.

---

## Service Hardening

```ini
NoNewPrivileges=true
PrivateTmp=true
```

These added restrictions beyond the service account itself.

---

# 🌐 Nginx Reverse Proxy

Nginx became the network-facing web server.

Instead of:

```text
Client ------> Uvicorn
```

the architecture used:

```text
Client
   |
   v
Nginx
   |
   v
Uvicorn
   |
   v
FastAPI
```

The backend target:

```nginx
proxy_pass http://127.0.0.1:8000;
```

Proxy headers included:

```nginx
proxy_set_header Host $host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
```

Configuration validation:

```bash
sudo nginx -t
```

This reinforced another operational workflow:

```text
Edit
  |
  v
Validate Syntax
  |
  v
Apply
  |
  v
Test
```

---

# 🚫 Backend Exposure Reduction

The final listening state included:

```text
0.0.0.0:80
0.0.0.0:443
127.0.0.1:8000
```

The important distinction:

```text
NETWORK-FACING

:80  → Nginx
:443 → Nginx
```

versus:

```text
LOCAL ONLY

127.0.0.1:8000 → Uvicorn
```

Validation:

```bash
sudo ss -lntp
```

This checked the **actual operating-system socket state**, not merely the intended configuration.

---

# 🔒 HTTPS & TLS

Nginx provided HTTPS using a self-signed TLS certificate.

The certificate was appropriate for this controlled lab and was not presented as a publicly trusted production certificate.

The exercise provided experience with:

- TLS
- HTTPS
- Certificate files
- TLS termination
- Private-key permissions
- Certificate inspection
- HTTP-to-HTTPS redirection

Certificate information was inspected with OpenSSL.

Example:

```bash
openssl s_client \
-connect 127.0.0.1:443 \
-servername linux-web01 \
</dev/null 2>/dev/null |
openssl x509 -noout -subject -issuer -dates
```

The certificate and private key were stored separately under system certificate locations.

Private-key permissions were restricted.

---

# ↪️ HTTP → HTTPS Redirect

HTTP requests were redirected to HTTPS.

Test:

```bash
curl -I http://127.0.0.1/
```

Result:

```text
HTTP/1.1 301 Moved Permanently
```

Request flow:

```text
HTTP :80
   |
   v
Nginx
   |
   | 301
   v
HTTPS :443
   |
   v
TLS
   |
   v
Nginx Reverse Proxy
   |
   v
Uvicorn
   |
   v
FastAPI
```

---

# 🔥 UFW Firewall

The server used a default-deny inbound firewall strategy.

Required traffic from:

```text
192.168.50.0/24
```

included:

| Port | Service | Purpose |
|---:|---|---|
| `22` | SSH | Administration |
| `80` | HTTP | HTTPS redirect |
| `443` | HTTPS | Application access |

Port:

```text
8000
```

was not opened to the network.

That created two separate protections around the backend:

```text
UFW
  +
Loopback Binding
```

or:

```text
Layer 1
Firewall Policy

Layer 2
127.0.0.1 Application Binding
```

Firewall validation:

```bash
sudo ufw status verbose
```

Socket validation:

```bash
sudo ss -lntp
```

---

# 🧪 Application Validation

Main endpoint:

```bash
curl -k https://127.0.0.1/
```

Successful response:

```json
{
  "status": "online",
  "server": "linux-web01",
  "service": "Week 21 Lab 2 Production Web Application"
}
```

Health endpoint:

```bash
curl -k https://127.0.0.1/health
```

Successful response:

```json
{
  "status": "healthy"
}
```

This validated:

```text
HTTPS
   |
   v
Nginx
   |
   v
Reverse Proxy
   |
   v
Uvicorn
   |
   v
FastAPI
```

---

# 📊 Multi-Layer Validation

I validated the application at several levels.

| Layer | Question | Tool |
|---|---|---|
| Process | Is Uvicorn running? | `ps` |
| Service | Is `webapp.service` active? | `systemctl` |
| Network | Is the expected socket listening? | `ss` |
| Proxy | Is Nginx functioning? | `nginx -t`, `curl` |
| Firewall | Is expected traffic allowed? | `ufw` |
| HTTPS | Can I make an encrypted request? | `curl` |
| Application | Is the app healthy? | `/health` |
| Logs | What happened? | `journalctl` |

This was stronger than relying on only:

```bash
systemctl status webapp
```

---

# 💥 Controlled Failure Simulation

The most important reliability test was deliberately terminating the application's main Uvicorn process.

First:

```bash
systemctl show webapp -p MainPID
```

Original process:

```text
MainPID=1092
```

The process was then intentionally terminated inside the lab:

```bash
sudo systemctl kill \
--signal=SIGKILL \
--kill-who=main \
webapp
```

This simulated an unexpected application process failure.

---

# 🔄 Automatic Recovery

Because the service contained:

```ini
Restart=on-failure
RestartSec=5
```

the expected sequence was:

```text
Uvicorn
   |
   v
SIGKILL
   |
   v
Process Terminates
   |
   v
systemd Detects Failure
   |
   v
Failure Logged
   |
   v
Restart Scheduled
   |
   v
5-Second Delay
   |
   v
Replacement Uvicorn Process
   |
   v
FastAPI Starts
   |
   v
Health Check
```

After recovery, systemd reported:

```text
active (running)
```

The replacement process was:

```text
Main PID: 1930
```

Therefore:

```text
BEFORE
PID 1092
   |
   X

AFTER
PID 1930
```

The PID change provided evidence that the original process was actually terminated and replaced.

---

# 🔎 Failure Investigation with `journalctl`

The journal recorded:

```text
Main process exited, code=killed, status=9/KILL
```

followed by:

```text
Failed with result 'signal'.
```

systemd then scheduled a restart.

Afterward, Uvicorn started under a new PID.

The application subsequently recorded:

```text
GET /health
```

with:

```text
200 OK
```

This created an evidence chain:

```text
Healthy Application
       |
       v
SIGKILL
       |
       v
Process Termination
       |
       v
systemd Failure Event
       |
       v
Restart Scheduled
       |
       v
New PID
       |
       v
Application Startup
       |
       v
GET /health
       |
       v
200 OK
```

This was more informative than simply observing that the service was running again.

---

# 🩺 Application-Level Recovery Validation

A service manager can report:

```text
active (running)
```

while an application still has a problem.

Because of that, recovery was not considered complete until:

```bash
curl -k https://127.0.0.1/health
```

returned:

```json
{
  "status": "healthy"
}
```

The test therefore validated:

```text
Process Recovery
       +
Service Recovery
       +
Network Recovery
       +
Application Recovery
```

---

# 📜 Nginx & Application Log Review

Nginx access logs were reviewed to confirm requests such as:

```text
GET /
GET /health
GET /does-not-exist
```

Application/systemd logs provided evidence about:

- Uvicorn startup
- HTTP requests
- Application process state
- Process failure
- systemd restart activity
- Recovery

This connected network requests with backend service behavior.

---

# 🔁 Post-Reboot Persistence

The Ubuntu system was rebooted after the environment was configured.

I did **not** manually start the application or Nginx after reboot.

Instead, I verified:

```bash
systemctl is-active webapp
systemctl is-active nginx

systemctl is-enabled webapp
systemctl is-enabled nginx
```

I then retested:

```bash
curl -k https://127.0.0.1/
curl -k https://127.0.0.1/health
```

and inspected listening sockets again.

This demonstrated:

```text
Works During Current Session
             ≠
Configured for Persistent Operation
```

The services returned automatically after boot.

---

# 🛡️ Lab 2 Security Controls

| Security Control | Implementation |
|---|---|
| Dedicated Service Identity | `webapp` |
| Least Privilege | Application not running as root |
| Backend Isolation | `127.0.0.1:8000` |
| Reverse Proxy | Nginx |
| Firewall | UFW |
| Default Inbound Policy | Deny |
| Lab Restrictions | `192.168.50.0/24` |
| Encryption | HTTPS/TLS |
| HTTP Protection | Redirect to HTTPS |
| Private-Key Protection | Restricted permissions |
| systemd Hardening | `NoNewPrivileges=true` |
| Temporary Directory Isolation | `PrivateTmp=true` |
| Recovery | `Restart=on-failure` |
| Health Monitoring | `/health` |
| Logging | journald + Nginx logs |

---

# 🧭 Troubleshooting Method

One of the most important outcomes of Lab 2 was developing a structured troubleshooting process.

Different tools answer different questions:

| Question | Tool |
|---|---|
| Is the service running? | `systemctl` |
| Is it enabled? | `systemctl is-enabled` |
| Why did it fail? | `journalctl` |
| What process is running? | `ps` |
| What PID does systemd manage? | `systemctl show` |
| What ports are listening? | `ss` |
| Is Nginx valid? | `nginx -t` |
| Does HTTPS work? | `curl` |
| Is the application healthy? | `/health` |
| What does the firewall permit? | `ufw` |
| What certificate is served? | `openssl` |
| Who owns the files/process? | `id`, `ls`, `stat`, `ps` |

The workflow became:

```text
Problem
   |
   v
Check Service State
   |
   v
Review Logs
   |
   v
Inspect Process
   |
   v
Inspect Listening Ports
   |
   v
Test Network/Application
   |
   v
Inspect Proxy
   |
   v
Inspect Firewall
   |
   v
Identify Cause
   |
   v
Make Smallest Required Change
   |
   v
Validate Again
```

---

# 🧪 Lab 2 Validation Matrix

| Control | Intended State | Validation |
|---|---|---|
| FastAPI | Responding | `curl` |
| Health Endpoint | Healthy | `/health` |
| Uvicorn | Running | `ps`, `systemctl` |
| Service Account | `webapp` | `id`, `ps` |
| Backend | Localhost only | `ss` |
| systemd | Active/enabled | `systemctl` |
| Recovery | Restart on failure | Controlled `SIGKILL` |
| Recovery Evidence | Logged | `journalctl` |
| Nginx | Valid/running | `nginx -t`, `systemctl` |
| HTTP Redirect | 301 → HTTPS | `curl -I` |
| HTTPS | Functional | `curl -k` |
| TLS Certificate | Served | `openssl` |
| Firewall | Expected rules | `ufw status verbose` |
| Boot Persistence | Automatic startup | Reboot test |

---

# 📈 Lab 2 Before vs. After

| Area | Before | After |
|---|---|---|
| Application | Basic Python application | Managed FastAPI service |
| Runtime | Manual | Uvicorn |
| Identity | User context | Dedicated `webapp` |
| Backend Exposure | Potentially broad | `127.0.0.1:8000` |
| Service Management | Manual | systemd |
| Web Entry Point | Direct | Nginx |
| HTTP | Basic | Redirected |
| HTTPS | Not configured | TLS |
| Firewall | Initial state | Default-deny UFW |
| Recovery | Manual | Automatic |
| Health | Process-centric | `/health` |
| Failure Testing | None | Controlled `SIGKILL` |
| Investigation | Basic | journald evidence |
| Persistence | Session-based concern | Boot validated |

---

# 🏁 Week 21 Lab 2 Outcome

The completed service architecture was:

```text
                          CLIENT
                             |
                             v
                            UFW
                             |
                             v
                      Nginx :80/:443
                             |
                          HTTPS
                             |
                             v
                       Reverse Proxy
                             |
                             v
                     127.0.0.1:8000
                             |
                             v
                          Uvicorn
                             |
                             v
                          FastAPI
                             |
                             v
                          /health


                    SUPPORTING CONTROLS
                             |
            +----------------+----------------+
            |                |                |
            v                v                v
         systemd          journald       Permissions
            |                |
            v                v
     Auto-Recovery      Failure Evidence
```

The application:

- Ran under a dedicated service account
- Did not require root for normal operation
- Was managed by systemd
- Started automatically
- Automatically recovered from unexpected process failure
- Used Nginx as a reverse proxy
- Redirected HTTP to HTTPS
- Used TLS
- Kept Uvicorn on localhost
- Used UFW for inbound restrictions
- Exposed a health endpoint
- Produced service logs
- Recovered after controlled failure
- Returned after system reboot

---

# ============================================================
# 📊 COMBINED PROJECT ANALYSIS
# ============================================================

# 🔗 How the Two Labs Connect

The two labs were designed around different technical goals, but together they created a much more complete Linux project.

Lab 1 answered questions such as:

```text
How do I administer Linux?

How do I control users?

How do I restrict privileges?

How do I protect files?

How do I secure SSH?

How do I restrict network traffic?

How do I monitor important changes?

How do I automate security checks?
```

Lab 2 extended those concepts:

```text
How do I securely run an application?

What user should the application run as?

What should be exposed to the network?

How should the application be managed?

How should HTTPS be provided?

How do I know the application is healthy?

What happens when its process fails?

How can I investigate the failure?

Can the environment recover automatically?
```

Together:

```text
LINUX ADMINISTRATION
        |
        v
LINUX SECURITY
        |
        v
APPLICATION DEPLOYMENT
        |
        v
SERVICE SECURITY
        |
        v
MONITORING
        |
        v
TROUBLESHOOTING
        |
        v
RECOVERY
```

---

# 💼 Skills Demonstrated

## Linux Administration

- Ubuntu administration
- Kali Linux
- Users and groups
- Service accounts
- Filesystem permissions
- ACLs
- Package management
- Process management
- systemd
- Network interfaces
- Routing
- DNS troubleshooting
- Listening socket analysis
- Cron
- Linux logs

## Security Engineering Foundations

- Least privilege
- Role-based access
- SSH hardening
- Public-key authentication
- Root-login restriction
- Firewall configuration
- Default-deny policy
- Network segmentation
- Reduced service exposure
- Audit logging
- AppArmor
- TLS
- Private-key protection
- Service hardening

## Application & Web Operations

- Python
- FastAPI
- Uvicorn
- Nginx
- Reverse proxying
- HTTP
- HTTPS
- TLS termination
- Health endpoints
- Backend isolation

## Troubleshooting & Incident Analysis

- `systemctl`
- `journalctl`
- `ss`
- `ps`
- `top`
- `pgrep`
- `curl`
- `openssl`
- `ufw`
- `nmap`
- `ausearch`
- `aureport`
- Authentication log review
- PID analysis
- Port analysis
- Failure reconstruction
- Recovery validation

## Automation

- Bash
- Variables
- Pipes
- Text processing
- Automated health checks
- Automated security reports
- Cron scheduling

---

# 🔎 Operational Mindset Developed

The biggest improvement across these projects was not learning one specific command.

It was learning to ask the right technical questions.

Instead of only asking:

```text
"Is the server working?"
```

I now break that into:

```text
What is running?

Who is running it?

What privileges does it have?

What files can it access?

What ports is it listening on?

Which interfaces are those ports bound to?

What does the firewall permit?

Can another host reach it?

How is authentication handled?

Is traffic encrypted?

What logs are generated?

What happens if the service fails?

Does it restart?

Is the application actually healthy afterward?

Will everything return after reboot?

How can I prove each answer?
```

---

# 🧠 Major Lessons Learned

## 1. Configuration Is Not Proof

A configuration file represents intended behavior.

It does not always prove actual behavior.

For example:

```text
sshd_config
     |
     v
sshd -T
     |
     v
Remote SSH Test
```

or:

```text
UFW Rule
   |
   v
ufw status
   |
   v
Nmap
```

or:

```text
systemd Restart Policy
       |
       v
Controlled Failure
       |
       v
Journal Evidence
       |
       v
Replacement PID
       |
       v
Health Check
```

---

## 2. Security Is Layered

No single control secured these environments.

The combined security model included:

```text
Network Segmentation
        +
Firewall
        +
Authentication
        +
Authorization
        +
Least Privilege
        +
Filesystem Permissions
        +
Service Accounts
        +
Application Binding
        +
TLS
        +
Logging
        +
Auditing
        +
Application Restrictions
        +
Validation
```

---

## 3. Running Does Not Mean Reachable

```text
systemctl
```

answers whether a service is running.

```text
ss
```

answers whether something is listening.

```text
ufw
```

helps answer whether traffic is permitted.

```text
nmap
```

shows what another system can reach.

```text
curl
```

shows whether the application responds.

These are different layers of evidence.

---

## 4. Running Does Not Mean Healthy

Lab 2 reinforced another distinction:

```text
Process Running
      ≠
Application Healthy
```

That is why the `/health` endpoint was useful.

---

## 5. Logs Tell the Story

Logs turned failures into timelines.

For the application failure:

```text
Process Running
      |
      v
SIGKILL
      |
      v
Failure Recorded
      |
      v
Restart Scheduled
      |
      v
New Process
      |
      v
Health Check
      |
      v
200 OK
```

That sequence could be reconstructed from evidence instead of guessed.

---

## 6. Least Privilege Applies to Humans and Applications

Lab 1 applied least privilege to human accounts.

Lab 2 applied it to an application service.

```text
Human Administrator
        |
        v
linuxadmin
        |
        v
sudo only when required
```

and:

```text
Application
     |
     v
webapp account
     |
     v
Only required permissions
```

---

## 7. Recovery Should Be Tested

A restart configuration is only a configuration until it is tested.

The controlled failure proved that systemd could:

```text
Detect
  |
  v
Record
  |
  v
Restart
  |
  v
Recover
```

the application.

---

# 📂 Recommended Repository Structure

```text
Linux-Systems-Security-Portfolio/
│
├── README.md
│
├── Week20-Lab1/
│   │
│   ├── Configs/
│   │   ├── ssh-hardening.conf
│   │   └── audit-hardening.rules
│   │
│   ├── Scripts/
│   │   ├── security-health-check.sh
│   │   ├── linux-security-report.sh
│   │   └── scheduled-health-report.sh
│   │
│   ├── Documentation/
│   │   ├── commands-used.md
│   │   └── lessons-learned.md
│   │
│   └── Screenshots/
│       ├── linux-baseline-system-info.png
│       ├── private-management-network-connectivity.png
│       ├── role-based-users-and-groups.png
│       ├── least-privilege-permissions-after.png
│       ├── ssh-hardening-effective-controls.png
│       ├── ufw-host-firewall-hardening.png
│       ├── auditd-sensitive-file-monitoring.png
│       ├── bash-security-health-check.png
│       └── final-hardened-server-security-posture.png
│
├── Week21-Lab2/
│   │
│   ├── app/
│   │   └── app.py
│   │
│   ├── Configs/
│   │   ├── webapp.service
│   │   └── nginx-linux-web01.conf
│   │
│   ├── Documentation/
│   │   ├── technical-analysis.md
│   │   └── recovery-case-study.md
│   │
│   └── Screenshots/
│       ├── systemd-service-configuration.png
│       ├── nginx-reverse-proxy.png
│       ├── https-application-validation.png
│       ├── firewall-and-listening-ports.png
│       ├── service-account-permissions.png
│       ├── tls-certificate-validation.png
│       ├── systemd-failure-recovery.png
│       ├── post-reboot-validation.png
│       └── final-production-validation.png
│
└── LICENSE
```

---

# 📸 Evidence Strategy

The screenshots in this repository are intended to prove major technical outcomes rather than document every command typed.

Strong evidence includes:

| Evidence | Demonstrates |
|---|---|
| Linux baseline | Initial system state |
| Private network | Network configuration |
| User/group configuration | Identity management |
| Permission testing | Least privilege |
| SSH effective configuration | SSH hardening |
| UFW state | Host firewall |
| Nmap | External exposure |
| Auditd events | Security monitoring |
| Bash output | Automation |
| systemd service | Application management |
| Uvicorn process ownership | Dedicated service account |
| Listening sockets | Backend isolation |
| Nginx configuration | Reverse proxy |
| TLS certificate | HTTPS |
| Health endpoint | Application health |
| Failure journal | Failure detection |
| PID change | Process replacement |
| Post-reboot state | Persistence |
| Final validation | Completed environment |

---

# 🔐 Repository Security

Sensitive material should never be committed.

This repository should not contain:

```text
SSH private keys
TLS private keys
Passwords
API keys
Access tokens
Authentication secrets
Production credentials
Sensitive private configurations
```

For example, the ED25519 private key remains on the administration workstation.

The TLS private key remains under the protected Linux certificate directory.

Only safe examples, scripts, screenshots, application code, and sanitized configurations should be published.

---

# 🚧 Limitations

These projects were completed in a controlled virtual home lab.

Several design decisions reflect that environment.

For example:

- TLS used a self-signed certificate.
- IP addressing was private lab addressing.
- Firewall restrictions were designed around the lab network.
- Failure testing was intentionally performed against systems I controlled.
- The architecture did not include external load balancing.
- Monitoring remained primarily local.
- There was no production certificate authority.
- The application was not intended as an Internet-facing production service.

These limitations are important because the project demonstrates the **technical concepts and validation process**, not a claim that the lab is equivalent to a full enterprise production environment.

---

# 🔮 Future Improvements

Potential extensions include:

- Centralized log collection
- SIEM integration
- Splunk or Elastic ingestion
- Security alerting
- Application metrics
- Infrastructure monitoring
- Resource monitoring
- Automated service-failure notifications
- Nginx rate limiting
- Additional HTTP security headers
- Automated certificate management
- Publicly trusted TLS certificates in an appropriate environment
- Configuration management
- Ansible automation
- Infrastructure as Code
- Automated deployment
- Backup automation
- Restore testing
- Dependency monitoring
- Vulnerability scanning
- File integrity monitoring
- Additional systemd hardening
- Application-level structured logging
- Automated health monitoring
- External availability checks

A future security-monitoring extension could follow:

```text
Linux Server / Application
           |
           v
        Logs
           |
           v
Centralized Collection
           |
           v
      SIEM Platform
           |
           v
 Detection / Correlation
           |
           v
         Alert
           |
           v
     Investigation
           |
           v
       Response
```

---

# 🎯 Relevance to Security Operations

Although these projects focus heavily on Linux administration, they also build skills that transfer directly into security operations.

Examples include:

```text
Authentication Logs
        |
        v
Failed Login Investigation
```

```text
Auditd Event
     |
     v
Sensitive File Change
     |
     v
Investigation
```

```text
Listening Port
      |
      v
Process Identification
      |
      v
Exposure Analysis
```

```text
Service Failure
      |
      v
journalctl
      |
      v
Timeline Reconstruction
      |
      v
Recovery Validation
```

These are useful foundations for work involving:

- SOC analysis
- Incident response
- Detection engineering
- Linux security
- Security engineering
- Cloud security
- Infrastructure security

---

# 🏆 Final Combined Outcome

Across these labs, I moved from administering an operating system to securely operating an application on top of it.

The full progression was:

```text
                    LINUX SERVER
                         |
                         v
                 System Baseline
                         |
                         v
                    Networking
                         |
                         v
                 Identity & Access
                         |
                         v
                  Least Privilege
                         |
                         v
                Filesystem Security
                         |
                         v
               Secure Remote Access
                         |
                         v
                 Firewall Security
                         |
                         v
                Logging & Auditing
                         |
                         v
                    Automation
                         |
                         v
               External Validation
                         |
                         v
               APPLICATION SERVER
                         |
                         v
                      FastAPI
                         |
                         v
                      Uvicorn
                         |
                         v
                     systemd
                         |
                         v
                       Nginx
                         |
                         v
                     HTTPS/TLS
                         |
                         v
                  Backend Isolation
                         |
                         v
                   Health Checking
                         |
                         v
                  Failure Testing
                         |
                         v
                 Log Investigation
                         |
                         v
                Automatic Recovery
                         |
                         v
                Final Validation
```

The most important lesson from the entire project was:

> **A system is not secure, reliable, or healthy simply because it appears to be running. Its identity, privileges, network exposure, configuration, logs, failure behavior, and recovery should also be understood and validated.**

---

# ✅ Overall Project Status

## Week 21 — Lab 1

- [x] Linux baseline
- [x] Dedicated hostname
- [x] Dual-interface networking
- [x] Private management network
- [x] Role-based identities
- [x] Least-privilege sudo
- [x] Filesystem hardening
- [x] setgid directories
- [x] ACL testing
- [x] Package administration
- [x] systemd administration
- [x] OpenSSH
- [x] ED25519 authentication
- [x] Password SSH disabled
- [x] Root SSH disabled
- [x] SSH group restrictions
- [x] UFW
- [x] Default-deny inbound policy
- [x] Nmap validation
- [x] Automatic update review
- [x] Auditd
- [x] Audit investigation
- [x] Authentication log review
- [x] AppArmor
- [x] Process administration
- [x] Network administration
- [x] Bash security automation
- [x] Automated reporting
- [x] Cron scheduling
- [x] Final attack-surface assessment

## Week 21 — Lab 2

- [x] FastAPI application
- [x] Python virtual environment
- [x] Uvicorn
- [x] Dedicated `webapp` service account
- [x] Application permissions
- [x] systemd service
- [x] Boot enablement
- [x] Automatic restart
- [x] `NoNewPrivileges`
- [x] `PrivateTmp`
- [x] Localhost-only backend
- [x] Nginx reverse proxy
- [x] Nginx validation
- [x] HTTP-to-HTTPS redirect
- [x] HTTPS/TLS
- [x] Certificate inspection
- [x] Private-key permission validation
- [x] UFW
- [x] Backend port isolation
- [x] `/health` endpoint
- [x] HTTPS validation
- [x] Listening-port validation
- [x] Service log investigation
- [x] Controlled `SIGKILL`
- [x] Failure detection
- [x] Automatic process replacement
- [x] Recovery validation
- [x] Post-reboot validation
- [x] Final production-style validation

---

# ⚠️ Disclaimer

All configuration, administration, network testing, failure simulation, and security validation documented in this repository were performed inside a controlled virtual lab using systems I owned and configured for educational and portfolio purposes.

The configurations are designed to demonstrate technical concepts in this environment. Production systems should be configured according to organizational requirements, approved security policies, architecture standards, change-management procedures, and risk requirements.

---

# 📬 Project Focus

**Linux Administration | Linux Security | Security Operations | Infrastructure Security | Application Security Foundations | Troubleshooting | Automation | Reliability**

---

**Week 21 — Lab 1: Complete** ✅  
**Week 21 — Lab 2: Complete** ✅
