# Week 21: Linux Administration, Secure Application Operations, and Incident Investigation

Three connected, hands-on labs completed in an authorized Oracle VirtualBox environment.

I built and hardened an Ubuntu server, deployed a web application on Linux, tested how that application recovered from a process failure, and investigated a simulated compromise using evidence from the Linux host. The project covers the full path from **building a system** to **operating it** to **investigating suspicious activity on it**.

| Lab | Focus | Main result |
| --- | --- | --- |
| [Lab 1](#lab-1-enterprise-linux-server-administration-and-hardening) | Linux administration and hardening | Built and tested accounts, permissions, SSH, firewall rules, auditing, and scheduled reports. |
| [Lab 2](#lab-2-secure-fastapi-deployment-and-service-recovery) | Application operations and recovery | Deployed a FastAPI service behind Nginx and verified recovery after killing its main process. |
| [Lab 3](#lab-3-linux-compromise-investigation) | Host-based incident investigation | Reconstructed simulated SSH, persistence, privilege, process, network, and file activity; then contained and cleaned up the lab. |

> **Authorization and scope:** All systems were virtual machines I owned and controlled. Security tests, process failure tests, and the simulated compromise took place inside my lab. No third-party system was targeted.

## What this project demonstrates

- Configuring and troubleshooting Ubuntu systems.
- Managing users, groups, sudo access, file permissions, and ACLs.
- Hardening SSH and testing access from a separate machine.
- Applying UFW rules and checking actual network exposure.
- Reviewing Linux authentication, service, audit, and application logs.
- Writing Bash checks and scheduling reports with cron.
- Running a FastAPI application through Uvicorn, systemd, and Nginx.
- Limiting a backend service to localhost and serving the application over lab HTTPS.
- Testing service failure, automatic restart, application health, and reboot behavior.
- Investigating a simulated Linux compromise directly on the host.
- Building an evidence-based timeline and verifying containment and cleanup.

**My approach throughout the project:** I treated a configuration file as a statement of intent. I used commands, access tests, process information, network checks, logs, and screenshots to find out whether the system actually behaved as intended.

## Navigation

- [Lab 1: Enterprise Linux Server Administration and Hardening](#lab-1-enterprise-linux-server-administration-and-hardening)
- [Lab 2: Secure FastAPI Deployment and Service Recovery](#lab-2-secure-fastapi-deployment-and-service-recovery)
- [Lab 3: Linux Compromise Investigation](#lab-3-linux-compromise-investigation)
- [How the labs connect](#how-the-labs-connect)
- [Evidence and documentation](#evidence-and-documentation)
- [Project limits and next improvements](#project-limits-and-next-improvements)
- [References](#references)

---

# Lab 1: Enterprise Linux Server Administration and Hardening

## Goal

The first lab focused on building an Ubuntu server that could be managed over a private network and then applying security controls to its accounts, files, remote access, firewall, auditing, and routine checks.

I used a separate Kali Linux machine to validate the server from another host. That mattered because a service can appear correctly configured on the server while access from the network behaves differently.

## Environment

| System | Purpose | Private lab address |
| --- | --- | --- |
| `linux-srv01` | Ubuntu server | `192.168.50.10` |
| `kali-admin01` | Administration and validation machine | `192.168.50.20` |

The machines communicated over a VirtualBox internal network, `192.168.50.0/24`. Separate NAT interfaces provided internet access for updates and package installation.

### Network design

```text
Package access and updates
          |
     VirtualBox NAT
          |
   Ubuntu and Kali VMs

Private administration and testing
          |
  192.168.50.0/24
          |
Kali 192.168.50.20 ↔ Ubuntu 192.168.50.10
```

I recorded the server baseline and checked interface names, IP addresses, and routes. During setup, I worked through routing issues so the private lab interface did not create an unwanted second default route. I then verified that the machines could communicate before tightening SSH and firewall access.

## Identity and privilege management

I created accounts and groups to represent different responsibilities:

| Account | Lab responsibility |
| --- | --- |
| `linuxadmin` | Server administration |
| `securityanalyst` | Security information access |
| `webadmin` | Web administration |
| `backupsvc` | Backup operations |

| Group | Intended purpose |
| --- | --- |
| `linux-admins` | Administrative access |
| `security-team` | Security-related access |
| `web-team` | Web-related access |
| `backup-team` | Backup-related access |

I verified accounts and memberships with commands such as `id` and `getent`. For sudo access, I placed a rule for `linux-admins` in `/etc/sudoers.d/linux-admins`, set restrictive file permissions, checked the configuration with `visudo -c`, and tested access from an authorized account.

**Why this matters:** A normal user account can perform routine work without operating as root all the time. An account should receive the permissions needed for its responsibility, and the resulting access should be tested.

## File permissions and ACLs

I practiced securing lab files and shared directories with ownership, groups, standard Linux permissions, the setgid bit, and access control lists.

I tested access from different user accounts. Looking at `ls -l` or `getfacl` helped me inspect the settings, but an actual allowed or denied file operation showed how the system enforced them.

This work helped me understand that file access can depend on several things at once:

- The account accessing the file.
- Its group memberships.
- Permissions on the file.
- Permissions on every directory in the path.
- Any ACL entries that apply.

## SSH access and hardening

I first established a working SSH connection from Kali to Ubuntu. Then I created an ED25519 key pair on Kali and installed the **public key** for the administrator account on Ubuntu.

I tested key-based access **before** disabling password authentication. I also kept an existing SSH session open while testing the new settings from another session. That gave me a way back into the server if the new configuration blocked access.

The lab used a separate OpenSSH configuration file:

```text
/etc/ssh/sshd_config.d/99-lab-hardening.conf
```

The settings documented in the lab included:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
MaxAuthTries 3
LoginGraceTime 30
AllowGroups linux-admins
X11Forwarding no
```

I checked the configuration syntax with `sshd -t` and reviewed effective settings with `sshd -T`. I then tested access from Kali, including an access attempt that should be denied.

| Check | What it helped establish |
| --- | --- |
| `sshd -t` | The SSH configuration had valid syntax. |
| `sshd -T` | The settings SSH would use after processing its configuration. |
| Authorized remote login | The intended administrator could connect. |
| Unauthorized remote test | The access restriction worked as expected in the lab. |

The private SSH key was not added to the repository.

## Host firewall and network exposure

I configured UFW with a default-deny incoming policy and permitted required management traffic from the private lab network.

I checked UFW's reported rules, inspected listening sockets with `ss`, and scanned the Ubuntu machine from Kali with Nmap. These checks answered different questions:

| Evidence | Question |
| --- | --- |
| UFW status | Which traffic did the host firewall allow? |
| `ss` on Ubuntu | Which processes were listening, and on which addresses? |
| Nmap from Kali | Which services appeared reachable from another lab machine? |

For example, seeing a listening service on Ubuntu does not mean that Kali can reach it. The service's binding address, UFW rules, and network path affect the result.

## Logging, auditing, and scheduled checks

I reviewed Linux authentication and privileged activity, practiced Auditd monitoring, and checked AppArmor status. These controls gave me different views of server activity and protection.

I also wrote Bash scripts for repeatable checks. The health-check script gathered information such as:

- Host and operating-system details.
- Disk and memory use.
- IP addresses and listening sockets.
- UFW status.
- Failed systemd units.
- SSH and other relevant service state.
- Recent authentication activity.

I checked the script's syntax before running it. A second script wrote timestamped security reports. I scheduled reports through cron at a **15-minute interval for lab testing** and inspected the generated output to confirm that scheduling worked. The short schedule helped me observe results during the lab; it was not a claim that every real server should generate reports every 15 minutes.

## Validation summary

| Area | What I configured or examined | How I checked it |
| --- | --- | --- |
| Network | Private management network and routing | IP, route, and connectivity checks |
| Accounts | Role-based users and groups | `id`, `getent`, account tests |
| Sudo | Controlled administrative access | `visudo -c`, authorized-user test |
| File access | Permissions and ACLs | Permission review and actual user tests |
| SSH | Keys and restricted remote access | `sshd -t`, `sshd -T`, remote tests |
| Firewall | Default-deny incoming policy | UFW status and tests from Kali |
| Exposure | Listening services | `ss` compared with Nmap |
| Auditing | Auditd activity | Audit rule and event review |
| Application restrictions | AppArmor status | Profile status check |
| Automation | Bash and cron | Syntax, execution, and generated reports |

## Lab 1 result

I finished with an Ubuntu server that had controlled accounts and administration, restricted SSH, a host firewall, auditing practice, and repeatable health reporting. The most useful lesson was to compare the **intended setting**, the **effective setting**, and an **actual access test**.

The setup also gave me troubleshooting practice. I worked through interface names, default routes, SSH authorization, firewall behavior, user permissions, script paths, cron output, and service state.

---

# Lab 2: Secure FastAPI Deployment and Service Recovery

## Goal

The second lab used the Linux foundations from Lab 1 to run a small FastAPI application as a managed service. I configured a dedicated service account, placed Nginx in front of the application, enabled HTTPS for the lab, and tested what happened when the application's main process was unexpectedly terminated.

This was a **production-style design exercise inside a lab**. It was not a public production deployment: the certificate was self-signed and the application was tested in the virtual environment.

## Environment and request path

| Component | Lab setup |
| --- | --- |
| Ubuntu host | `linux-web01` |
| Application | FastAPI |
| Application server | Uvicorn |
| Application directory | `/opt/webapp` |
| Service account | `webapp` |
| Service manager | systemd |
| Reverse proxy | Nginx |
| Backend listener | `127.0.0.1:8000` |
| Client-facing ports | HTTP `80`, HTTPS `443` |
| Private lab network | `192.168.50.0/24` |
| TLS certificate | Self-signed lab certificate |

```text
Lab client
   |
   | HTTP :80 → HTTPS redirect
   | HTTPS :443
   v
 Nginx
   |
   | Local request
   v
 Uvicorn at 127.0.0.1:8000
   |
   v
 FastAPI application
```

Nginx was the web entry point. Uvicorn listened on the loopback address instead of a network-facing address. I checked the listening sockets and firewall rules to confirm the backend was not directly exposed to another lab machine.

## Application identity and service configuration

I created a dedicated `webapp` account and ran the application as that user rather than as root or my personal account. I checked account and process information to confirm the service used the intended identity.

The systemd service started Uvicorn from the application directory. Its documented settings included:

```ini
[Service]
User=webapp
Group=webapp
WorkingDirectory=/opt/webapp
ExecStart=/opt/webapp/venv/bin/uvicorn app:app --host 127.0.0.1 --port 8000
Restart=on-failure
RestartSec=5
NoNewPrivileges=true
PrivateTmp=true
```

The settings served different purposes:

| Setting | Purpose in this lab |
| --- | --- |
| `User` and `Group` | Run the application under a dedicated account. |
| `WorkingDirectory` and `ExecStart` | Define where and how the application starts. |
| `--host 127.0.0.1` | Keep the backend listener on the local host. |
| `Restart=on-failure` | Ask systemd to restart the service after qualifying failures. |
| `RestartSec=5` | Wait before a restart attempt. |
| `NoNewPrivileges=true` | Add a restriction against gaining new privileges through execution. |
| `PrivateTmp=true` | Give the service a private temporary-file view. |

I checked the service state, process ownership, and listening address after applying the configuration. The configuration text showed what I requested; those checks showed what was running.

## Nginx, TLS, and firewall controls

I configured Nginx as the reverse proxy, redirected HTTP requests to HTTPS, and used a self-signed certificate for the controlled lab. I checked the served certificate with OpenSSL and restricted the certificate private key's file access.

Because the certificate was self-signed, I used `curl -k` for some lab tests. In that command, `-k` skips certificate trust verification. It does **not** mean the certificate would be trusted by a public browser or suitable as-is for a public production site.

UFW used a default-deny incoming approach and permitted the required lab access. SSH, HTTP, and HTTPS had defined purposes. Port `8000` was not opened as a network-facing application port, and Uvicorn bound to `127.0.0.1`.

I tested each layer separately:

1. Nginx configuration validity.
2. Nginx and application service state.
3. Listening addresses and ports.
4. UFW rules.
5. HTTP-to-HTTPS redirection.
6. HTTPS application response.
7. The application's `/health` response.

A healthy process is useful evidence, but an application request is needed to show that the application actually responds.

## Controlled process failure

I recorded the main PID of `webapp.service` and then deliberately killed its main process through systemd with `SIGKILL`. This was a controlled test of the restart behavior I had configured.

The journal showed the process ending after a kill signal and the service recording a failure. systemd then scheduled a restart. I observed a replacement process with a different PID.

The evidence chain was:

| Step | Evidence I checked |
| --- | --- |
| Application running before the test | Service state and original main PID |
| Main process terminated | Kill action and journal failure event |
| Restart attempted | systemd journal entries |
| Replacement process started | New main PID and active service state |
| Application recovered | Successful request to `/health` |

The PID change mattered because it showed the original main process was gone and another process had started. The successful health request mattered because it showed that the application responded after recovery.

## Reboot check

After configuring the environment, I rebooted the Ubuntu host and checked that Nginx and the application service returned without starting them manually. I reviewed active and enabled service states, tested HTTPS and `/health`, and checked listening sockets.

That test answered a separate question from the process-failure test: **Would the application come back after a fresh boot?**

## Lab 2 result

I deployed an application with a dedicated service account, a localhost-only backend, a reverse proxy, lab HTTPS, firewall rules, and systemd service management. I then demonstrated failure, investigated its journal events, verified a new process started, and confirmed the application responded again.

The key lesson was to test recovery at both the **service layer** and the **application layer**. An active service state alone does not prove a web application is answering requests.

---

# Lab 3: Linux Compromise Investigation

## Goal and scope

The third lab was an **authorized attack simulation and host investigation**. I generated suspicious activity against an Ubuntu VM that I controlled, then investigated it using Linux logs, process and network commands, account records, cron, systemd, SSH keys, and file metadata.

The scenario covered SSH authentication, a lab account, a Python process, persistence, privilege indicators, a connection to Kali, and file changes. I investigated directly on Linux rather than relying on a SIEM or endpoint security console.

| System | Role in Lab 3 | Lab address |
| --- | --- | --- |
| `linux-srv01` | Ubuntu host under investigation | `192.168.56.10` |
| `kali-admin01` | Simulation and observation host | `192.168.56.20` |

**Network note:** Lab 3 used `192.168.56.0/24`. Labs 1 and 2 documented `192.168.50.0/24`. These addresses represent the lab setups used for the separate exercises.

## Investigation questions

I investigated the activity in this order:

1. **Initial access:** Which SSH attempts failed, and which login was accepted?
2. **Account context:** Which account was involved, and what groups did it belong to?
3. **Privilege activity:** What did sudo records show?
4. **Execution:** What program ran, and which service launched it?
5. **Persistence:** Could cron, systemd, or an SSH key bring the activity back?
6. **Network behavior:** Which process connected to Kali?
7. **File activity:** Which artifacts existed, and what did their timestamps show?
8. **Response:** Did containment stop the activity, and did cleanup remove the lab artifacts?

## Evidence sources

| Source | Tools or location | What I used it to check |
| --- | --- | --- |
| SSH and authentication records | `journalctl`, `/var/log/auth.log`, `grep`, `awk` | Failed and accepted authentication events |
| Session records | `last`, `lastb`, `who`, `w` | Historical and current login context |
| Accounts and groups | User and group records | Identity and potential access |
| Privileged activity | Sudo records | Recorded attempts or commands |
| Processes | `ps`, `pstree`, `systemctl` | Running programs and service context |
| Persistence | Cron locations, systemd units, `authorized_keys` | Ways access or execution could continue |
| Network state | `ss`, `lsof`, Kali listener | Connection, process, and destination |
| Files | `find`, `stat`, file inspection | Related artifacts and recorded file times |

These sources have different strengths. For example, an accepted SSH event proves authentication succeeded, while a file modification time shows a recorded change but does not identify who made it.

## Initial access and account activity

The Ubuntu authentication evidence showed failed SSH events followed by an accepted login for the lab account `irguest21`. I reviewed the journal and authentication log, then checked session and account information for context.

The accepted event established that SSH access succeeded. I did not treat it as automatic proof that the same session caused every later change. I looked for supporting evidence in the account records, processes, service activity, network connection, and files.

The account and its access activity were created as part of my authorized simulation.

## Persistence findings

I checked three paths that could maintain access or execution:

| Method | Lab artifact | Why it mattered |
| --- | --- | --- |
| Scheduled execution | Cron entry | Could run the lab command again on a schedule. |
| Service execution | `ir-lab21.service` | Could start the Python program as a systemd service. |
| Remote access | SSH `authorized_keys` entry | Could allow another key-based login. |

The systemd unit was associated with a Python program under `/opt/ir-lab21`. I compared the unit with service information, process information, and the observed network behavior. Finding a unit file was useful, but service and process evidence helped establish that the lab program ran.

## Privilege indicators

I reviewed the account's groups and sudo records. Group membership showed what access might be available. The sudo records helped investigate specific privilege-related activity.

I kept **possible access** separate from **observed successful action**. A group membership or a sudo attempt does not, by itself, prove that every privileged command succeeded.

## Process, network, and file activity

The lab Python service connected from Ubuntu to the Kali listener at `192.168.56.20:8000`. I checked sockets and process information on Ubuntu and observed the receiving side on Kali.

This supports an observed connection between the lab process and the Kali destination. It does not show real-world data theft.

I also examined the systemd unit, Python program, change marker, and temporary setup files. File timestamps helped order the artifacts, but I did not use a timestamp alone to name an actor.

### Artifacts in scope

| Artifact | Role in this simulation |
| --- | --- |
| `irguest21` | Lab account |
| Account SSH `authorized_keys` entry | Key-based access |
| Lab cron entry | Scheduled persistence |
| `/etc/systemd/system/ir-lab21.service` | Service persistence |
| `/opt/ir-lab21/agent.py` | Python program |
| `/opt/ir-lab21/change-marker.txt` | File modification marker |
| Temporary `ir-lab21` files under `/tmp` | Setup artifacts |
| Temporary UFW allowance for Kali SSH | Lab access rule reviewed during cleanup |

These names describe my lab artifacts. The same filename or account name on another system would require its own investigation and context.

## Evidence-based timeline

The times below are recorded in **UTC**. I have marked observations without a verified exact time rather than guessing.

| UTC time | Observation | What the evidence supports |
| --- | --- | --- |
| 18:42:22 | Failed SSH authentication | An authentication attempt failed. |
| 18:57:36 | Another failed SSH authentication | Additional failed authentication activity was recorded. |
| 19:05:31 | SSH authentication accepted for `irguest21` | Access to the lab account succeeded. |
| 19:16:49 | Change marker modification time observed | A file had this recorded modification time; the timestamp alone does not identify the actor. |
| 19:33:26 | systemd unit file time observed | The service artifact had this recorded file time. |
| Approximately 19:49:34 | Later Python program file time observed | The program changed during lab setup. |
| 19:53:23 | Service restart observed in lab notes | The service started again during the simulation. |
| Exact time not established | Connection to Kali port `8000` observed | Process and listener evidence showed the lab connection. |
| Exact time not established | Containment and cleanup | Lab access and persistence artifacts were addressed. |

The timeline combines authentication records, file metadata, service observations, and notes from the exercise. The events are presented in order, but a file timestamp alone does not prove that the earlier SSH session caused the change.

## Containment and cleanup

I stopped and disabled the lab service, removed the cron entry and lab SSH key, and locked the lab account during containment. After recording the evidence needed for the investigation, I removed the lab service unit, Python program, change marker, temporary files, and account. I reloaded systemd after removing the unit.

I reviewed the temporary UFW rule used for Kali SSH access as part of cleanup. My verification checked for remaining lab account, service, directory, process, and network activity.

In an actual incident, I would follow the organization's response process and preserve needed evidence before removing artifacts. I would also review other access paths and related systems. In this controlled exercise, I could remove the activity I had created after documenting it.

## Findings and limits

**What the evidence supports:** The simulated case included SSH access, persistence, privilege indicators, a Python process, a connection to Kali, and local file changes.

**What the evidence does not establish:** It does not prove that my other virtual machines were compromised or that real-world information was stolen. I have not assigned exact times to events where I could not verify them.

## Lab 3 result

I reconstructed the simulated incident using Linux host evidence, built a timeline, contained the activity, removed the lab artifacts, and checked the result. The main skill I practiced was connecting separate records into a careful explanation while stating what each record could and could not prove.

---

# How the labs connect

| Lab | What I learned | How I applied it later |
| --- | --- | --- |
| **Lab 1: Build and harden** | Accounts, SSH, permissions, firewall rules, logs, and service checks | Established how authorized access and the server's controls should work. |
| **Lab 2: Deploy and operate** | Service identity, localhost binding, reverse proxying, systemd, logs, and recovery | Developed a method for checking processes, ports, application state, and failure events. |
| **Lab 3: Investigate and respond** | Authentication, privilege, persistence, process, network, and file evidence | Used Linux administration knowledge to explain and respond to simulated suspicious activity. |

The same habit helped in each lab: **check the result at the layer that can actually prove it**.

- A valid SSH configuration needs an access test.
- An allowed firewall rule needs a network test.
- A running process needs an application health test.
- A restart policy needs a failure and recovery test.
- A cleanup command needs a final state check.
- An incident claim needs evidence from the relevant logs, processes, files, or connections.

---

# Evidence and documentation

I saved screenshots and written reports from the labs. I selected evidence that shows a configuration or command **together with its result**, rather than keeping a screenshot of every step.

| Lab | Strong evidence to review |
| --- | --- |
| **Lab 1** | Effective SSH settings, authorized and denied access tests, UFW rules, remote exposure tests, audit results, and generated cron reports |
| **Lab 2** | Service account and socket state, HTTPS and health responses, journal failure entries, new PID, recovery response, and reboot checks |
| **Lab 3** | SSH authentication, account and sudo context, cron and systemd artifacts, SSH key, process and connection, UTC timeline, containment, and cleanup checks |

The Lab 3 technical analysis and incident case study provide more detail than this overview. Screenshot and report links should be added using the **actual filenames and paths committed to this repository** so that GitHub can render them correctly.

I do not publish SSH private keys, passwords, or other secrets as evidence.

---

# Project limits and next improvements

These were controlled learning labs, so the results should be read within that scope:

- The network addresses were private VirtualBox lab addresses.
- Lab 2 used a self-signed TLS certificate, not a publicly trusted certificate.
- Lab 2 tested recovery from a specific killed-process failure. That test does not prove recovery from every possible outage.
- Lab 3 was a simulation I created. It does not establish an unknown real-world attacker or data exfiltration.
- Some Lab 3 events did not have a reliable exact timestamp.
- The README describes the documented exercises; it is not a claim that the setup meets every production requirement.

Useful next improvements would be to add verified links to the saved screenshots and reports, repeat the application tests with automated checks, and practice incident evidence preservation before cleanup in a separate lab.

---

# References

These official resources helped inform the Linux configuration, application design, incident-response approach, and README presentation:

- [GitHub: About repository README files](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-readmes)
- [Ubuntu Server: OpenSSH server](https://ubuntu.com/server/docs/how-to/security/openssh-server/)
- [Ubuntu Server: Firewalls](https://ubuntu.com/server/docs/how-to/security/firewalls/)
- [Ubuntu Server: AppArmor](https://ubuntu.com/server/docs/how-to/security/apparmor/)
- [FastAPI: Deployment concepts](https://fastapi.tiangolo.com/deployment/concepts/)
- [FastAPI: About HTTPS](https://fastapi.tiangolo.com/deployment/https/)
- [systemd: Service unit documentation](https://www.freedesktop.org/software/systemd/man/systemd.service.html)
- [NIST SP 800-61 Rev. 3: Incident Response Recommendations and Considerations](https://csrc.nist.gov/pubs/sp/800/61/r3/final)

---

**Status:** Week 21 Labs 1, 2, and 3 completed in an authorized VirtualBox environment.
