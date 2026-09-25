# Week 21, Lab 3: Linux Compromise — Incident Case Study

**Analyst:** Fitzgerald Afari-Minta  
**Case type:** Authorized lab simulation  
**Ubuntu host:** `linux-srv01` (`192.168.56.10`)  
**Simulation host:** Kali `kali-admin01` (`192.168.56.20`)  
**Time zone for listed events:** UTC

## Case summary

I created a controlled incident on my Ubuntu virtual machine and investigated it from the Linux host. The scenario began with SSH authentication activity and access to a lab account. It then included a suspicious Python process, cron and systemd persistence, an SSH public key, privilege indicators, a connection to the Kali virtual machine, and modified files. I used Linux logs and commands to understand the sequence, then contained and cleaned up the activity.

This was my own VirtualBox lab. The activity was simulated and authorized. The Kali machine represented the source of suspicious behavior; it was not an unknown attacker on the internet.

## Initial access

I reviewed the Ubuntu SSH journal and authentication log to find failed and accepted sign-in events. The records showed failures at 18:42:22 and 18:57:36 UTC, followed by an accepted authentication event at 19:05:31 UTC for the lab account `irguest21`. I compared this with account information and session commands such as `last`, `who`, and `w`.

**What this means:** The accepted event confirms that SSH authentication worked. The failed events provide context, but the available evidence does not prove that every failed attempt came from a separate person. Because I generated this activity in my lab, I know it was part of the scenario.

## Persistence

I inspected scheduled jobs, systemd units, and the account's SSH `authorized\_keys`. The simulated activity included a cron entry, an `ir-lab21.service` systemd unit, and a public key for the lab account. The unit started a Python program stored under `/opt/ir-lab21`.

**Why it matters:** A cron entry or systemd unit can restart a program, and an authorized SSH key can preserve a way to sign in. I connected the unit to the program by looking at its configuration, service records, and the running activity.

## Privilege activity

I checked the account's groups and sudo logs for evidence of elevated access. The lab account's privileges and the sudo records were relevant to the case. I treated membership as a possible ability to run privileged commands and used the logs to look for what was actually attempted or run. I did not describe a command as successful unless the log evidence supported it.

## Network behavior

The lab Python service connected from Ubuntu to the Kali listener at `192.168.56.20:8000`. I checked socket and process information with `ss` and `lsof` and observed the receiving side on Kali. This showed a connection between the program and the lab destination. It did not show real-world exfiltration.

## Affected artifacts

The investigation centered on the `irguest21` account and its SSH key, the cron entry, `/etc/systemd/system/ir-lab21.service`, `/opt/ir-lab21/agent.py`, `/opt/ir-lab21/change-marker.txt`, and temporary lab files. I also reviewed the UFW rule that allowed SSH from the Kali lab address. These were artifacts I created for the scenario; their names are useful for explaining this case but are not universal malware indicators.

## Evidence-based timeline

|UTC time|Event|Evidence or limitation|
|-|-|-|
|18:42:22|SSH authentication failure|Authentication records.|
|18:57:36|Additional SSH authentication failure|Authentication records.|
|19:05:31|SSH authentication accepted for `irguest21`|SSH log or journal.|
|19:16:49|Change marker shows a modification time|File metadata; it does not identify who changed it.|
|19:33:26|Service unit shows a recorded file time|File metadata for the systemd artifact.|
|About 19:49:34|Lab Python program has a later observed file time|Approximate timestamp from the lab.|
|19:53:23|Service restart observed|Lab notes and service investigation.|
|Exact time not established|Service connected to Kali port `8000`|Host network inspection and Kali listener output.|
|Exact time not established|Containment and cleanup completed|Service, account, file, and firewall checks.|

## Scope and impact

I investigated one Ubuntu server and used one Kali machine to simulate and observe the activity. The case involved access to the lab account, a persistence mechanism, privilege indicators, a lab network connection, and local file changes. I cannot claim that another VM was affected or that information was stolen. This was a practice scenario, so the impact was limited to the virtual machines and changes I made for the exercise.

## Containment

I stopped and disabled the `ir-lab21` service, removed the scheduled job and lab SSH key, and locked the lab account so it could not keep being used during the investigation. I checked the process and connection state afterward to see whether the behavior continued.

## Remediation and verification

After gathering the evidence needed for the portfolio, I removed the service unit, Python program, marker, temporary files, lab account, and home directory. I reloaded systemd after removing the unit. I also reviewed and removed the temporary UFW SSH rule for the Kali host. I verified that the lab unit, account, and directory were gone and checked for remaining activity. These checks helped show that cleanup took effect.

## Lessons learned

The strongest conclusion came from putting several kinds of evidence together. The SSH log showed access; account and sudo records added privilege context; cron, systemd, and SSH keys showed ways activity could continue; process and socket checks tied the Python program to its network connection; and file timestamps helped order the events. I also learned to state the limits of the evidence. A timestamp does not identify an actor by itself, and a network connection alone does not prove data theft.

## 

