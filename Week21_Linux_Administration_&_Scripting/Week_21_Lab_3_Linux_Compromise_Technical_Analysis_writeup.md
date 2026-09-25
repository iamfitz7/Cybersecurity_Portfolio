# Week 21, Lab 3: Linux Compromise Investigation — Technical Analysis

**Analyst:** Fitzgerald Afari-Minta  
**Lab type:** Authorized simulation in an isolated VirtualBox environment  
**Investigated host:** Ubuntu `linux-srv01` (`192.168.56.10`)  
**Simulation host:** Kali `kali-admin01` (`192.168.56.20`)  
**Evidence time zone:** UTC where shown by the host; see the screenshots for the original timestamps.

## Purpose

I investigated a simulated Linux compromise using the operating system's own evidence. I followed activity from SSH authentication through account access, persistence, privilege indicators, an outbound connection, file changes, and cleanup. This project focused on host investigation with Linux commands rather than a SIEM or endpoint security console.

## Lab boundaries

All activity was created by me for this lab on virtual machines I control. The accounts, scripts, network traffic, and service described below were part of the simulation. Kali represented the source of suspicious access, and Ubuntu was the host I investigated. The private `192.168.56.0/24` addresses were used inside the lab. I did not use this activity against a third-party system.

## Investigation method

I first checked access and account history, then looked for programs that were running or set to run again. After that I checked network connections, file timestamps, and signs of elevated privileges. I compared the results to build a timeline before removing the lab changes.

|Question|Linux evidence and commands|What the evidence showed|
|-|-|-|
|Who tried to sign in?|`journalctl`, `/var/log/auth.log`, `last`, `lastb`, `who`, `w`, `grep`, `awk`|SSH failures and later access to the lab account. `lastb` depends on whether failed-login accounting is available.|
|Which account was involved?|Account and group records, `id`, sudo logs|The lab account `irguest21` and its group and privilege context.|
|What was running?|`ps`, `pstree`, `systemctl`, `journalctl`|A lab Python program was associated with the persistence activity.|
|How could it start again?|Cron entries, systemd unit files, SSH `authorized\_keys`|The investigation covered scheduled startup, a service unit, and SSH key access.|
|Where did it connect?|`ss`, `lsof`, service logs, Kali listener output|The lab service made a connection toward the Kali host on port `8000`.|
|What changed on disk?|`find`, `stat`, file inspection|Files under `/opt/ir-lab21` and the systemd unit were part of the simulated activity.|

The same sign-in can appear in more than one source. For example, `last` summarizes sessions, while the SSH journal and `auth.log` provide more detail. A single command or a single log line is not enough to tell the entire story.

## Findings

### Initial access

The simulated sequence began with SSH authentication activity from the Kali side of the lab. The Ubuntu authentication records showed failed SSH attempts and a later accepted login for `irguest21`. The accepted event is evidence that authentication succeeded; it does not, by itself, prove what the user did after logging in. I used account records and later host activity to investigate the next steps.

The lab account was a deliberate part of the simulation. An SSH public key was also part of the access path. During an actual incident, an unfamiliar account or authorized key would require checking when it was added, who approved it, and which sessions used it.

### Persistence

I checked both cron and systemd because either can run a command again after a user signs out. The simulated persistence involved a cron entry and an `ir-lab21.service` systemd unit. The unit pointed to a Python program in `/opt/ir-lab21`. I also checked the account's SSH `authorized\_keys`, because a key can allow another login later even if a password is changed.

Finding a service name alone would not establish that the program ran. I compared the unit configuration with service state, journal entries, processes, and the connection seen from Kali. Together, these observations showed how the lab program was launched and what it did.

### Privilege activity

I reviewed sudo records and group membership to determine whether the lab account had a path to elevated privileges. The lab account's membership and the recorded sudo activity were privilege indicators worth investigating. Group membership shows what an account may be allowed to do; a sudo log records a specific attempt or command. I did not assume every sudo event succeeded without checking its result in the log.

### Network behavior

The service's Python process connected from Ubuntu toward `192.168.56.20:8000`, where I observed the lab listener on Kali. I used `ss`, `lsof`, service output, and the receiving terminal to connect the socket to the process and destination. This was controlled traffic inside the VirtualBox lab. The evidence supports a connection to the Kali listener; it does not establish that any real-world data was stolen.

### Affected artifacts

|Artifact|Role in the simulation|Investigation or cleanup|
|-|-|-|
|`irguest21` account|Lab account used for access|Checked account/group records; later removed.|
|Account SSH `authorized\_keys`|Public-key access|Inspected; lab key removed during containment.|
|Cron entry|Scheduled persistence|Inspected; lab entry removed.|
|`/etc/systemd/system/ir-lab21.service`|Service persistence|Inspected, stopped, disabled, and removed.|
|`/opt/ir-lab21/agent.py`|Lab Python program|Inspected and removed after recording evidence.|
|`/opt/ir-lab21/change-marker.txt`|File-change marker|Checked with file tools and removed.|
|Temporary `ir-lab21` files under `/tmp`|Lab setup artifacts|Removed during cleanup.|
|UFW rule allowing Kali SSH|Lab access rule|Reviewed and removed when cleanup was verified.|

The names above are lab artifacts, not indicators that these same paths are malicious on another machine.

## Timeline

The times below reflect timestamps observed during the lab and are in UTC. Some events could not be assigned a reliable exact time. The screenshots and saved host output are the source of record if a time needs to be checked again.

|Time (UTC)|Observation|Meaning|
|-|-|-|
|18:42:22|Failed SSH authentication event|An access attempt did not succeed.|
|18:57:36|Another failed SSH authentication event|Authentication failures continued.|
|19:05:31|Accepted SSH authentication event|Access to the lab account succeeded.|
|19:16:49|File modification time observed for the change marker|A lab file changed after access. A file timestamp alone does not identify the actor.|
|19:33:26|Systemd unit file timestamp observed|The persistence artifact was present by this recorded time.|
|Approximately 19:49:34|Python file timestamp observed after correction|The lab program was changed during setup; this is an approximate observed time.|
|19:53:23|Service restart observed in lab notes|The service was started again during the simulation.|
|Exact time not established|Ubuntu-to-Kali connection on port `8000` observed|The process connected to the Kali listener.|
|Exact time not established|Containment and cleanup|The account, key, cron entry, unit, program, and lab firewall rule were addressed.|

These are observations from a simulated case. They should not be read as proof that every event was caused by the earlier SSH login; the correlation is based on the account, service, artifacts, and lab actions together.

## Scope

The investigation covered `linux-srv01` and the Kali host used to generate and observe the lab activity. I examined the simulated account, SSH access, cron and systemd persistence, sudo indicators, processes, network connection, and related files. I did not find evidence in this lab to claim compromise of the other VirtualBox machines. The review was limited to the available host logs, file metadata, commands, and screenshots.

## Containment and remediation

I stopped and disabled the lab service, removed the cron entry and SSH key, and locked the lab account during containment. After recording the needed evidence, I removed the lab service unit, Python program, marker and temporary files, reloaded systemd, removed the lab account and its home directory, and checked that the lab directory and unit were gone. I reviewed the UFW rules and removed the temporary Kali SSH allowance used for the lab. I then checked for remaining service activity and connections.

In a real case, I would preserve appropriate evidence before deleting artifacts, confirm the account owner's identity, review other access paths, rotate affected credentials or keys, and check related systems. Because this was a controlled lab, I could remove my own simulation files after documenting them.

## Lessons learned

1. SSH success is the start of an investigation, not the whole answer. Account activity, sudo records, processes, and changed files provide the follow-up context.
2. Check more than one persistence method. Cron, systemd, and SSH keys can each restore access or execution.
3. Link a connection to its process and service before describing the network behavior.
4. Use timestamps carefully. File modification times do not prove who made a change, and an unrecorded event should not be given a made-up time.
5. Verification matters after containment. I checked the service, account, lab files, connection, and firewall rule rather than relying only on the cleanup commands returning successfully.

## 

