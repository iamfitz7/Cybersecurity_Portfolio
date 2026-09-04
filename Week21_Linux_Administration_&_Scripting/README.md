````markdown
# 🐧 Week 20 — Lab 1: Enterprise Linux Server Administration, Hardening & Secure Remote Access

![Linux](https://img.shields.io/badge/Linux-Ubuntu-E95420?logo=ubuntu&logoColor=white)
![Bash](https://img.shields.io/badge/Scripting-Bash-4EAA25?logo=gnubash&logoColor=white)
![Security](https://img.shields.io/badge/Focus-System%20Hardening-blue)
![SSH](https://img.shields.io/badge/Remote%20Access-OpenSSH-black)
![Firewall](https://img.shields.io/badge/Firewall-UFW-red)
![Auditd](https://img.shields.io/badge/Auditing-Auditd-purple)
![AppArmor](https://img.shields.io/badge/Access%20Control-AppArmor-orange)
![VirtualBox](https://img.shields.io/badge/Virtualization-VirtualBox-183A61?logo=virtualbox&logoColor=white)

---

## 📌 Project Overview

This project focused on building, administering, hardening, monitoring, automating, and validating an Ubuntu Linux server inside a controlled virtual environment.

I created a two-system Linux lab consisting of:

- **Ubuntu Server — `linux-srv01`**
- **Kali Linux Administration Workstation — `kali-admin01`**

Both systems were connected through a dedicated VirtualBox internal management network while keeping separate NAT interfaces for Internet access, updates, and package installation.

The goal was not simply to practice Linux commands. I wanted to build a Linux environment that demonstrated how system administration, access control, network security, secure remote administration, logging, auditing, automation, and security validation work together.

During the project, I:

- Established and documented a Linux system baseline
- Configured a dedicated private management network
- Created role-based users and groups
- Implemented least-privilege sudo administration
- Secured sensitive files and shared directories
- Tested Linux permissions and ACL-based access
- Managed packages and services
- Configured and hardened OpenSSH
- Implemented ED25519 public-key authentication
- Disabled SSH password authentication
- Disabled direct root SSH login
- Restricted SSH access to an authorized administrative group
- Configured a default-deny UFW firewall
- Validated exposed services from Kali Linux using Nmap
- Reviewed automatic security-update mechanisms
- Configured Auditd monitoring for sensitive resources
- Investigated authentication and privileged activity
- Reviewed AppArmor mandatory access controls
- Built Bash scripts for security health checks and reporting
- Scheduled recurring reports with cron
- Performed a final security and attack-surface assessment

A major part of this project was **validation**. I did not assume a security control worked just because its configuration file looked correct. Controls were tested through effective configuration checks, access attempts, service inspection, log review, audit events, and external validation from Kali Linux.

---

## 🎯 Project Goals

The main goals of this project were to:

1. Build a structured Linux server environment
2. Apply secure Linux administration practices
3. Implement role-based access and least privilege
4. Secure remote administration with SSH keys
5. Reduce unnecessary network exposure
6. Improve visibility through logging and auditing
7. Automate repeatable security checks
8. Validate security controls from both local and remote perspectives
9. Document the final server security posture
10. Better understand how Linux administration creates useful security evidence

---

## 🏗️ Lab Environment

| System | Purpose | Management IP |
|---|---|---|
| Ubuntu Linux | Server Administration & Hardening | `192.168.50.10` |
| Kali Linux | Administration & Security Validation | `192.168.50.20` |
| Oracle VirtualBox | Virtualization Platform | N/A |

Each virtual machine used two network adapters:

| Interface | Purpose |
|---|---|
| NAT | Internet access, updates, and package installation |
| `LinuxLab` Internal Network | Private administration and security testing |

---

## 🌐 Network Architecture

```text
                              INTERNET
                                 |
                        +--------+--------+
                        |  VirtualBox NAT |
                        +--------+--------+
                                 |
                   +-------------+-------------+
                   |                           |
             Ubuntu Server                 Kali Linux
              linux-srv01                 kali-admin01
                   |                           |
          192.168.50.10                192.168.50.20
                   |                           |
                   +-------------+-------------+
                                 |
                         LinuxLab Network
                          192.168.50.0/24
```

The two-interface design separated Internet connectivity from private management traffic.

The NAT interfaces were used for:

- Operating system updates
- Package installation
- Internet connectivity

The `LinuxLab` interfaces were used for:

- SSH administration
- Internal connectivity testing
- Firewall validation
- Nmap scanning
- Security testing

This allowed administrative traffic to remain on a dedicated private network.

---

## 🧰 Technologies & Tools

| Technology | Purpose |
|---|---|
| Ubuntu Linux | Server administration and hardening |
| Kali Linux | Remote administration and validation |
| Oracle VirtualBox | Virtualized lab infrastructure |
| Bash | Administration and security automation |
| OpenSSH | Secure remote administration |
| ED25519 | Public-key SSH authentication |
| UFW | Host-based firewall |
| Auditd | Security auditing |
| AppArmor | Mandatory access control |
| systemd | Service management |
| journalctl | System and service log review |
| Netplan | Ubuntu network configuration |
| NetworkManager | Kali network configuration |
| APT / DPKG | Package management and inventory |
| Nginx | Service administration and exposure testing |
| Nmap | External service validation |
| ACL | Granular filesystem permissions |
| Cron | Scheduled automation |
| `ss` | Network socket inspection |
| `ps` / `top` / `pgrep` | Process administration |
| `grep` / `awk` / `cut` / `sed` | Log and text processing |

---

## 🛡️ Security Controls Implemented

The final server used multiple security layers instead of depending on one control.

```text
                  Private Management Network
                             |
                             v
                       UFW Firewall
                             |
                             v
                       OpenSSH Server
                             |
                             v
                 ED25519 Authentication
                             |
                             v
                    AllowGroups Control
                             |
                             v
                  Role-Based Linux Users
                             |
                             v
                  Least-Privilege sudo
                             |
                             v
              File Permissions + ACLs
                             |
                             v
                         AppArmor
                             |
                             v
                 Auditd + System Logging
                             |
                             v
                  Bash Security Checks
```

This project reinforced that server security is strongest when network, identity, privilege, filesystem, application, logging, and monitoring controls support each other.

---

## 1️⃣ System Baseline & Host Configuration

Before making changes, I collected a baseline of the Ubuntu server.

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

The baseline documented:

- Current user
- Hostname
- Operating system
- Kernel
- Network interfaces
- Routing
- Storage
- Filesystem usage
- Memory

The server was then assigned a clear hostname:

```bash
sudo hostnamectl set-hostname linux-srv01
```

Final hostname:

```text
linux-srv01
```

This established an important habit that I followed throughout the project:

> **Observe first, change second, and verify afterward.**

---

## 2️⃣ Private Management Network

A dedicated internal network named `LinuxLab` was configured in VirtualBox.

```text
Network: 192.168.50.0/24

Ubuntu: 192.168.50.10
Kali:   192.168.50.20
```

Ubuntu maintained its NAT interface for Internet connectivity while the internal interface handled private administrative traffic.

The internal interface was configured without another default gateway so Internet traffic continued using NAT.

Connectivity was tested in both directions.

From Kali:

```bash
ping -c 4 192.168.50.10
```

From Ubuntu:

```bash
ping -c 4 192.168.50.20
```

Successful communication confirmed that the private management network was functioning before additional controls were applied.

---

## 3️⃣ Role-Based Linux Identity Administration

I created separate users and groups to represent different responsibilities instead of giving every account the same level of access.

### Groups

```text
linux-admins
security-team
web-team
backup-team
```

### Users

| User | Group | Purpose |
|---|---|---|
| `linuxadmin` | `linux-admins` | Server administration |
| `securityanalyst` | `security-team` | Security data and log access |
| `webadmin` | `web-team` | Web service administration |
| `backupsvc` | `backup-team` | Backup-related access |

Example commands included:

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

Group membership was verified using:

```bash
id linuxadmin
id securityanalyst
id webadmin
id backupsvc
```

This created a clearer access model where permissions could be assigned according to responsibility.

---

## 4️⃣ Least-Privilege Sudo Administration

Administrative elevation was limited to the `linux-admins` group.

A dedicated sudoers configuration was created:

```text
/etc/sudoers.d/linux-admins
```

with:

```text
%linux-admins ALL=(ALL:ALL) ALL
```

The file was protected:

```bash
sudo chmod 440 /etc/sudoers.d/linux-admins
```

Sudo configuration was validated before use:

```bash
sudo visudo -c
```

The `linuxadmin` account was then tested:

```bash
sudo whoami
```

Expected result:

```text
root
```

This allowed administration to occur through an individual user account while requiring privilege elevation only when necessary.

This provides better accountability than routinely working through a shared or directly logged-in root account.

---

## 5️⃣ Filesystem Permission Hardening

I created a simulated organizational directory structure:

```text
/srv/company/
├── security/
├── web/
└── backups/
```

Ownership was assigned according to the appropriate group.

```bash
sudo chown root:security-team /srv/company/security
sudo chown root:web-team /srv/company/web
sudo chown root:backup-team /srv/company/backups
```

The directories were configured with:

```bash
sudo chmod 2770 /srv/company/security
sudo chmod 2770 /srv/company/web
sudo chmod 2770 /srv/company/backups
```

The `2` enabled the **setgid** bit so new files created inside a shared directory could inherit the directory's group ownership.

### 🚨 Insecure Permission Before Hardening

A sensitive test file was intentionally given insecure permissions:

```text
security-investigation.txt
```

Before remediation:

```text
777
rwxrwxrwx
```

This meant every user could read, modify, or execute the file.

### 🔒 Hardened Permission

The file was remediated using:

```bash
sudo chown root:security-team /srv/company/security/security-investigation.txt
sudo chmod 640 /srv/company/security/security-investigation.txt
```

Final permissions:

```text
640
rw-r-----
```

| Identity | Permission |
|---|---|
| Owner | Read + Write |
| `security-team` | Read |
| Everyone Else | No Access |

---

## 6️⃣ Permission Validation

I tested the permissions using the actual user accounts.

`securityanalyst` could read the security file because the account belonged to `security-team`.

A modification attempt was denied because the group had read-only access.

`webadmin` was initially unable to read the file because it did not belong to `security-team`.

This demonstrated an important difference between:

```text
Configuration says access should be denied
```

and:

```text
Testing proves access is denied
```

The second provides stronger evidence that the control actually works.

---

## 7️⃣ Granular Access with ACLs

I used Access Control Lists to demonstrate more specific authorization.

The existing ACL was reviewed:

```bash
getfacl /srv/company/security/security-investigation.txt
```

Temporary read-only access was granted to `webadmin`:

```bash
sudo setfacl -m u:webadmin:r /srv/company/security/security-investigation.txt
```

The permission was tested and verified.

After validation, the temporary exception was removed:

```bash
sudo setfacl -x u:webadmin /srv/company/security/security-investigation.txt
```

This demonstrated how an individual account can receive a specific permission without permanently changing the main group structure.

---

## 8️⃣ Package & Software Administration

APT and DPKG were used to manage and inspect software.

Examples included:

```bash
sudo apt update
sudo apt upgrade
apt search nginx
sudo apt install nginx
apt show nginx
dpkg -l | grep nginx
```

At the end of the project, security-related packages were reviewed with:

```bash
dpkg -l | grep -E 'nginx|openssh|audit|apparmor|ufw'
```

Knowing which software and versions are installed is important when reviewing system exposure, updates, and vulnerabilities.

---

## 9️⃣ Service Management with systemd

Nginx was used to practice service administration.

```bash
systemctl status nginx
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
systemctl is-enabled nginx
```

Logs were reviewed with:

```bash
journalctl -u nginx
```

This reinforced the difference between:

- A service being installed
- A service being running
- A service being enabled at boot
- A service listening on the network
- A service being reachable by another system

Listening sockets were reviewed using:

```bash
sudo ss -tulpn
```

---

## 🔑 1️⃣0️⃣ Secure SSH Remote Administration

OpenSSH Server was installed on Ubuntu.

An initial remote connection was tested from Kali:

```bash
ssh linuxadmin@192.168.50.10
```

Inside the remote session:

```bash
whoami
hostname
```

Expected results:

```text
linuxadmin
linux-srv01
```

This confirmed that the administration path between Kali and Ubuntu was working before SSH hardening began.

---

## 🔐 1️⃣1️⃣ ED25519 Public-Key Authentication

An ED25519 SSH key pair was generated on Kali:

```bash
ssh-keygen -t ed25519
```

The public key was copied to Ubuntu:

```bash
ssh-copy-id linuxadmin@192.168.50.10
```

Key-based authentication was successfully tested **before** password authentication was disabled.

This order was important because disabling password authentication before validating the SSH key could have caused an administrative lockout.

> **The private SSH key was never included in the project repository.**

---

## 🛡️ 1️⃣2️⃣ OpenSSH Hardening

SSH was hardened using a dedicated configuration snippet:

```text
/etc/ssh/sshd_config.d/99-lab-hardening.conf
```

Key settings included:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
MaxAuthTries 3
LoginGraceTime 30
AllowGroups linux-admins
X11Forwarding no
```

These controls:

- Disabled direct root login
- Required public-key authentication
- Disabled password authentication
- Reduced allowed authentication attempts
- Reduced login grace time
- Restricted SSH access to `linux-admins`
- Disabled unnecessary X11 forwarding

Before applying the configuration, syntax was checked:

```bash
sudo sshd -t
```

The effective configuration was then verified:

```bash
sudo sshd -T | grep -E \
'permitrootlogin|pubkeyauthentication|passwordauthentication|maxauthtries|logingracetime|allowgroups|x11forwarding'
```

I also kept the existing SSH session open while testing a second connection so I had a recovery path if the new configuration caused a problem.

---

## 🚫 1️⃣3️⃣ Unauthorized SSH Validation

The `webadmin` account was used to test the SSH restriction.

Even though `webadmin` was a valid Linux account, it was not a member of:

```text
linux-admins
```

Because SSH was configured with:

```text
AllowGroups linux-admins
```

the account was denied SSH access.

This demonstrated that:

```text
Valid Linux Account
        ≠
Authorized Remote Administrator
```

The final remote administration path became:

```text
Kali Workstation
      |
      v
LinuxLab Network
      |
      v
UFW Firewall
      |
      v
OpenSSH
      |
      v
ED25519 Key Authentication
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

## 🔥 1️⃣4️⃣ UFW Firewall Hardening

UFW was configured with a default-deny inbound policy.

```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

SSH access was restricted to the private management network:

```bash
sudo ufw allow from 192.168.50.0/24 to any port 22 proto tcp
```

The firewall was enabled:

```bash
sudo ufw enable
```

The final policy was reviewed:

```bash
sudo ufw status verbose
```

The firewall strategy followed:

```text
Incoming Traffic
       |
       v
DENY by Default
       |
       +---- Required Management Traffic
       |
       +---- Required Service Traffic
       |
       +---- Everything Else Denied
```

This reduced unnecessary network exposure while keeping required lab services available.

---

## 🔍 1️⃣5️⃣ External Service Validation with Nmap

I validated the server from Kali instead of relying only on Ubuntu's local configuration.

```bash
nmap -sT 192.168.50.10
```

This allowed me to compare:

```text
Ubuntu Local View
        |
        v
ss -tulpn
```

against:

```text
Remote Network View
        |
        v
Nmap from Kali
```

This helped answer an important security question:

> **What can another system actually reach?**

If an unexpected service appeared remotely, I could return to Ubuntu and identify the responsible process or service.

---

## 🔄 1️⃣6️⃣ Automatic Security Update Review

The server's automatic update mechanisms were reviewed.

```bash
dpkg -l | grep unattended-upgrades
cat /etc/apt/apt.conf.d/20auto-upgrades
systemctl status apt-daily.timer
systemctl status apt-daily-upgrade.timer
```

This connected server hardening with ongoing maintenance rather than treating security as a one-time configuration task.

---

## 👁️ 1️⃣7️⃣ Auditd Security Monitoring

Auditd was configured to provide additional visibility into changes involving sensitive resources.

Persistent audit rules included:

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

The rules provided monitoring for changes involving:

- User identities
- Groups
- Credentials
- Privilege configuration
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

## 🕵️ 1️⃣8️⃣ Audit Event Investigation

I generated an authorized test event inside the monitored security directory:

```bash
sudo touch /srv/company/security/audit-test.txt
```

The event was then investigated:

```bash
sudo ausearch -k security_data_changes -i
```

Audit summaries were also reviewed using:

```bash
sudo aureport
```

This demonstrated the monitoring cycle:

```text
Sensitive Resource
       |
       v
Activity Occurs
       |
       v
Auditd Captures Event
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

## 📜 1️⃣9️⃣ Authentication & Privileged Activity Review

SSH activity was reviewed using:

```bash
sudo journalctl -u ssh --since today
```

Authentication logs were also reviewed where available.

A controlled failed SSH attempt was generated from Kali using an account that was not authorized for SSH.

The resulting activity was reviewed for:

- Account name
- Source system
- Timestamp
- Authentication result
- Related SSH activity

Privileged activity was also reviewed through sudo-related logging.

This showed how normal administrative activity creates evidence that can later support troubleshooting, auditing, and security investigation.

---

## 🧱 2️⃣0️⃣ AppArmor Validation

AppArmor status was reviewed using:

```bash
sudo aa-status
```

This confirmed that the AppArmor framework was loaded and that profiles were being enforced.

Traditional Linux permissions answer questions such as:

```text
Can this user access this file?
```

AppArmor adds another layer:

```text
What is this application allowed to access?
```

This demonstrated the difference between standard Linux permissions and mandatory access controls.

---

## ⚙️ 2️⃣1️⃣ Process & Network Administration

Process activity was reviewed using:

```bash
ps aux
ps aux --sort=-%cpu | head
ps aux --sort=-%mem | head
pgrep -a nginx
pstree -p
top
```

Network state was reviewed using:

```bash
ip -br address
ip route
ss -tulpn
resolvectl status
ping
getent hosts
```

Rather than memorizing commands individually, I used them to answer questions such as:

```text
What is running?

Who owns the process?

What is consuming resources?

Which process is listening?

Which interface has this address?

Where will traffic go?

Is DNS working?

Can another host reach the server?
```

---

## 🤖 2️⃣2️⃣ Bash Security Health Check

I built a Bash script to combine multiple security and administration checks into one repeatable workflow.

The script collected information including:

- Hostname and system information
- Filesystem usage
- Memory utilization
- IP addresses
- Listening sockets
- UFW status
- Failed systemd units
- SSH state
- Nginx state
- AppArmor status
- Recent failed authentication activity

The script used:

- Variables
- Command substitution
- Pipes
- `grep`
- `tail`
- Output formatting
- Standard-error redirection
- Linux administration commands

Syntax was checked using:

```bash
bash -n ~/scripts/security-health-check.sh
```

Permissions were restricted:

```bash
chmod 750 ~/scripts/security-health-check.sh
```

The script turned several separate commands into one reusable server health-check process.

---

## 📊 2️⃣3️⃣ Automated Linux Security Reporting

A second Bash script generated timestamped Linux security reports.

The report included:

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

Reports were written using timestamped filenames such as:

```text
linux-security-report-YYYYMMDD-HHMMSS.txt
```

This created repeatable evidence that could be reviewed later instead of requiring every check to be performed manually.

---

## ⏰ 2️⃣4️⃣ Scheduled Reporting with Cron

A report directory was created:

```bash
mkdir -p ~/admin-reports
```

A dedicated scheduled reporting script was used with cron.

The lab schedule was:

```text
*/15 * * * * /home/fitzgerald/scripts/scheduled-health-report.sh >> /home/fitzgerald/admin-reports/cron.log 2>&1
```

For the lab, this generated a report every 15 minutes.

Cron configuration was verified with:

```bash
crontab -l
```

Cron service state was checked:

```bash
systemctl is-active cron
```

Generated report files were then reviewed to confirm that the scheduled automation actually executed.

> **Note:** The 15-minute schedule was used for lab validation. A real environment would use a schedule based on actual operational requirements.

---

## 🔎 2️⃣5️⃣ Final Security Assessment

After completing the configuration, I performed a final review instead of ending the project immediately after the last change.

Key checks included:

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

The final review checked:

- Firewall state
- Effective SSH configuration
- Audit rules
- AppArmor status
- Listening services
- SSH availability
- Nginx availability
- Cron availability
- Failed systemd units

The server was also checked remotely from Kali Linux.

---

## ✅ Final Security Posture

| Security Area | Final State |
|---|---|
| Server Hostname | `linux-srv01` |
| Private Management Network | `192.168.50.0/24` |
| Server Management IP | `192.168.50.10` |
| Administration Workstation | `192.168.50.20` |
| Internet Connectivity | Separate NAT interface |
| Identity Management | Role-based users and groups |
| Privileged Administration | Controlled through sudo |
| Sensitive File Access | Least-privilege permissions |
| Shared Directories | setgid group inheritance |
| Granular Authorization | ACL testing completed |
| SSH Authentication | ED25519 public keys |
| SSH Password Authentication | Disabled |
| Root SSH Login | Disabled |
| SSH Authorization | Restricted to `linux-admins` |
| SSH Authentication Attempts | Limited |
| X11 Forwarding | Disabled |
| Host Firewall | UFW active |
| Inbound Firewall Policy | Deny by default |
| Management Access | Restricted to private network |
| Security Auditing | Auditd active with custom rules |
| Authentication Visibility | SSH and authentication logs reviewed |
| Mandatory Access Control | AppArmor validated |
| Service Management | systemd |
| Security Automation | Bash health checks |
| Security Reporting | Automated Bash reports |
| Scheduled Automation | Cron |
| External Validation | Kali Linux + Nmap |
| Final System Review | Completed |
| Recovery Point | Final VirtualBox snapshot created |

---

## 📈 Before vs. After

| Area | Before | After |
|---|---|---|
| Host Identity | Initial VM configuration | Named `linux-srv01` |
| Network | Basic VM connectivity | Dedicated management network |
| User Structure | Standard accounts | Role-based identities |
| Privileged Access | General administration | Controlled sudo elevation |
| Sensitive File Test | `777` | `640` |
| Shared Directories | Basic permissions | `2770` + setgid |
| Granular Access | Standard permissions | ACL testing |
| SSH | Initial password testing | ED25519 keys |
| Root SSH | Not explicitly hardened | Disabled |
| SSH Passwords | Initially available | Disabled |
| SSH Authorization | General access | `AllowGroups linux-admins` |
| Firewall | Initial state | Default-deny inbound |
| Management Exposure | General | Private network restriction |
| Audit Visibility | Standard logging | Custom Auditd rules |
| Security Checks | Manual | Bash automation |
| Reporting | Manual review | Timestamped reports |
| Scheduling | Manual | Cron automation |
| Exposure Validation | Local checks | External Nmap validation |

---

## 🧪 How I Validated the Controls

One of the most important parts of this project was testing each major control after configuration.

| Control | Configuration | Validation |
|---|---|---|
| Network | Netplan / NetworkManager | `ip`, `ip route`, `ping` |
| Users & Groups | `useradd`, `groupadd`, `usermod` | `id`, `getent` |
| Sudo | `/etc/sudoers.d/` | `visudo -c`, `sudo whoami` |
| File Permissions | `chmod`, `chown` | `ls -l`, user access tests |
| ACLs | `setfacl` | `getfacl`, access tests |
| Services | systemd | `systemctl`, `journalctl` |
| SSH | `sshd_config.d` | `sshd -t`, `sshd -T`, remote login |
| SSH Authorization | `AllowGroups` | Unauthorized user test |
| Firewall | UFW | `ufw status`, Nmap |
| Network Exposure | Local service state | `ss` + remote Nmap |
| Auditd | Audit rules | `auditctl`, `ausearch`, `aureport` |
| Authentication Logging | SSH activity | `journalctl`, authentication logs |
| AppArmor | Existing profiles | `aa-status` |
| Bash Automation | Scripts | `bash -n`, execution results |
| Cron | User crontab | `crontab -l`, generated reports |

The general validation process was:

```text
Configure
    |
    v
Check Syntax
    |
    v
Apply
    |
    v
Inspect Effective State
    |
    v
Test the Control
    |
    v
Generate Activity
    |
    v
Review Evidence
    |
    v
Confirm Final State
```

---

## 🚧 Troubleshooting & Problem Solving

Not every part of the project worked perfectly on the first attempt.

Some of the issues I worked through included:

- Identifying the correct network interface names
- Keeping NAT and private management traffic separated
- Preventing the management interface from creating another default route
- Testing SSH keys before disabling password authentication
- Keeping an existing SSH session open during hardening
- Confirming effective SSH settings after configuration
- Verifying firewall rules from another system
- Testing permissions with actual user accounts
- Checking which process owned a listening port
- Validating Bash syntax before execution
- Correcting script paths and permissions
- Creating required report directories
- Correcting the scheduled reporting workflow before cron testing
- Confirming cron generated actual report files
- Reviewing service state when Ubuntu used SSH socket activation

These troubleshooting steps were valuable because they required me to understand **why** something was not working rather than only repeating commands.

---

## 🧠 What I Learned

This project improved my understanding of Linux far beyond basic command usage.

The biggest lesson was that Linux administration and Linux security are closely connected.

A secure Linux server depends on understanding:

- Users and groups
- Privileges
- File ownership
- Permissions
- Processes
- Services
- Network interfaces
- Routes
- Listening ports
- Authentication
- Logs
- Packages
- Scheduled tasks
- Application restrictions
- Audit records

I also learned that configuration alone is not enough.

For example:

```text
sshd_config
```

shows what I intended to configure.

```bash
sshd -T
```

shows the effective SSH configuration.

And:

```bash
ssh
```

from another system shows whether the control behaves correctly in practice.

The same idea applied to the firewall:

```text
UFW Configuration
       |
       v
Local Firewall Status
       |
       v
Remote Nmap Validation
```

And to auditing:

```text
Audit Rule
    |
    v
Test Activity
    |
    v
Audit Event
    |
    v
Investigation
```

This project helped me think less in terms of isolated commands and more in terms of complete technical workflows.

---

## 💡 Key Takeaways

### Security Is Layered

The server was not protected by one setting. Network controls, authentication, authorization, permissions, firewall rules, auditing, logging, and application controls all worked together.

### Least Privilege Must Be Tested

Creating groups and setting permissions is only part of the process. Testing allowed and denied access provides stronger proof that authorization works.

### Running Does Not Mean Reachable

A service can be running without being accessible remotely.

```text
systemctl → Is it running?

ss → Is it listening?

UFW → Is traffic allowed?

Nmap → Can another host reach it?
```

### Logs Provide Evidence

Authentication events, sudo activity, Auditd records, and systemd logs help explain what happened on a system.

### Automation Improves Repeatability

Bash scripts reduced the need to manually run the same checks repeatedly.

### Hardening Should Be Validated

Security changes can cause outages or lockouts if they are applied without testing.

The workflow I followed was:

```text
Understand → Configure → Validate → Test → Review
```

---

## 📂 Repository Structure

```text
Enterprise-Linux-Server-Hardening/
│
├── README.md
│
├── Screenshots/
│   ├── linux-baseline-system-info.png
│   ├── private-management-network-connectivity.png
│   ├── role-based-users-and-groups.png
│   ├── insecure-permission-before.png
│   ├── least-privilege-permissions-after.png
│   ├── file-acl-granular-access-control.png
│   ├── systemd-service-management.png
│   ├── successful-ssh-remote-administration.png
│   ├── openssh-hardening-configuration.png
│   ├── ssh-hardening-effective-controls.png
│   ├── ufw-host-firewall-hardening.png
│   ├── auditd-sensitive-file-monitoring.png
│   ├── ssh-authentication-log-investigation.png
│   ├── apparmor-status.png
│   ├── bash-security-health-check.png
│   ├── automated-linux-security-report.png
│   ├── cron-security-report-automation.png
│   ├── external-service-exposure-validation.png
│   └── final-hardened-server-security-posture.png
│
├── Scripts/
│   ├── security-health-check.sh
│   ├── linux-security-report.sh
│   └── scheduled-health-report.sh
│
├── Configs/
│   ├── ssh-hardening.conf
│   └── audit-hardening.rules
│
└── Documentation/
    ├── commands-used.md
    └── lessons-learned.md
```

---

## 📸 Project Evidence

| Evidence | What It Shows |
|---|---|
| System Baseline | Original Ubuntu system state |
| Private Management Network | Kali and Ubuntu connectivity |
| Role-Based Accounts | Users, groups, and responsibilities |
| Permission Before Hardening | Insecure `777` test state |
| Permission After Hardening | Least-privilege `640` state |
| ACL Validation | Temporary user-specific access |
| systemd Administration | Service management |
| SSH Connection | Successful remote administration |
| SSH Hardening | Hardened SSH configuration |
| Effective SSH Policy | Loaded SSH security settings |
| UFW Configuration | Host firewall policy |
| External Nmap Scan | Network-visible services |
| Auditd Rules | Sensitive-resource monitoring |
| Audit Event | Recorded sensitive-resource change |
| Authentication Investigation | SSH activity review |
| AppArmor | Mandatory access control status |
| Bash Health Check | Automated security checks |
| Automated Report | Timestamped system report |
| Cron | Scheduled report execution |
| Final Assessment | Hardened server state |

---

## 🔐 Repository Security

Sensitive authentication material was not included in the repository.

The following should never be publicly committed:

```text
SSH private keys
Passwords
API keys
Access tokens
Authentication secrets
Sensitive production configurations
```

For this project, the ED25519 private key remained on the administration workstation:

```text
~/.ssh/id_ed25519
```

Only safe project documentation, screenshots, scripts, and configuration examples should be included.

---

## 🏁 Final Project Outcome

By the end of this project, I had taken an Ubuntu Linux system through a complete administration and hardening workflow:

```text
System Baseline
      |
      v
Network Configuration
      |
      v
Identity Administration
      |
      v
Least Privilege
      |
      v
Filesystem Security
      |
      v
Package Management
      |
      v
Service Administration
      |
      v
Secure Remote Access
      |
      v
SSH Hardening
      |
      v
Firewall Hardening
      |
      v
Logging & Auditing
      |
      v
Mandatory Access Control
      |
      v
Bash Automation
      |
      v
Scheduled Reporting
      |
      v
External Validation
      |
      v
Final Security Assessment
```

The finished environment demonstrated the ability to build and manage a Linux server, secure administrative access, control privileges, protect sensitive resources, reduce network exposure, monitor important system changes, investigate activity through logs, automate repeatable checks, and validate that the final controls worked as intended.

More importantly, this project changed the way I approach Linux systems.

Instead of seeing Linux as a list of commands to memorize, I now approach a server by asking:

```text
What is this system supposed to do?

Who should have access?

What privileges do they actually need?

What services are running?

What is exposed to the network?

How is remote access protected?

What sensitive resources need monitoring?

What evidence will be available if something changes?

What checks can be automated?

How can I prove the controls are working?
```

That mindset was one of the most valuable outcomes of the project.

> **A security control is not complete just because it was configured. It should also be tested, validated, and supported by evidence.**

---

## ⚠️ Disclaimer

This project was completed in a controlled virtual lab using systems that I owned and configured for educational and portfolio purposes.

The configurations shown in this repository were designed for this lab environment. Real systems should be configured according to their specific technical requirements, approved policies, change procedures, and security standards.

---

## ✅ Project Status

**Completed**

- [x] Linux server baseline
- [x] Dedicated server hostname
- [x] Dual-interface networking
- [x] Private management network
- [x] Role-based users and groups
- [x] Password-aging practice
- [x] Least-privilege sudo
- [x] Filesystem permission hardening
- [x] setgid shared directories
- [x] ACL access testing
- [x] Package administration
- [x] systemd service management
- [x] OpenSSH remote administration
- [x] ED25519 authentication
- [x] SSH password authentication disabled
- [x] Direct root SSH disabled
- [x] SSH group restriction
- [x] UFW default-deny firewall
- [x] Management traffic restriction
- [x] External Nmap validation
- [x] Automatic update review
- [x] Auditd monitoring
- [x] Audit event investigation
- [x] Authentication log review
- [x] Privileged activity review
- [x] AppArmor validation
- [x] Process administration
- [x] Network administration
- [x] Bash health-check automation
- [x] Automated security reporting
- [x] Cron scheduling
- [x] Final package review
- [x] Final attack-surface assessment
- [x] Final hardened-server verification
- [x] Final VirtualBox snapshot

---

**Week 20 — Lab 1 Complete** ✅
````
