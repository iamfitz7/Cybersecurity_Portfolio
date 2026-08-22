# 🐧 Week 20 — Lab 1: Enterprise Linux Server Administration, Hardening & Secure Remote Access

![Linux](https://img.shields.io/badge/Linux-Ubuntu-E95420?logo=ubuntu&logoColor=white)
![Bash](https://img.shields.io/badge/Scripting-Bash-4EAA25?logo=gnubash&logoColor=white)
![Security](https://img.shields.io/badge/Focus-System%20Hardening-blue)
![SSH](https://img.shields.io/badge/Remote%20Access-OpenSSH-black)
![Firewall](https://img.shields.io/badge/Firewall-UFW-red)
![VirtualBox](https://img.shields.io/badge/Virtualization-VirtualBox-183A61?logo=virtualbox&logoColor=white)

## 📌 Overview

This lab focused on building, administering, securing, and validating an Ubuntu Linux server in a controlled virtual enterprise environment.

I created a two-system Linux lab consisting of an **Ubuntu server (`linux-srv01`)** and a **Kali Linux administration workstation (`kali-admin01`)** connected through a dedicated VirtualBox internal management network.

Rather than treating Linux administration as a collection of isolated commands, this project focused on the complete lifecycle of administering and hardening a Linux server. I established a system baseline, configured networking, implemented role-based identity management, enforced least privilege, secured remote administration, configured firewall controls, implemented auditing and logging, reviewed mandatory access controls, automated security checks with Bash, scheduled recurring reports with cron, and performed a final security assessment.

The project combines skills relevant to **Linux administration, security engineering, SOC operations, incident response, infrastructure security, and security automation**.

---

## 🎯 Objectives

The objectives of this lab were to:

- Establish a Linux system baseline before making configuration changes
- Configure a dedicated Linux management network
- Administer Linux users and groups
- Implement role-based access controls
- Configure least-privilege sudo administration
- Secure files and directories using Linux permissions
- Implement granular authorization with ACLs
- Manage packages with APT and DPKG
- Administer services using systemd
- Configure secure SSH remote administration
- Implement ED25519 public-key authentication
- Disable SSH password authentication
- Prevent direct root SSH access
- Restrict SSH access to authorized administrators
- Configure a deny-by-default UFW firewall
- Validate externally exposed services using Nmap
- Review automatic security-update mechanisms
- Configure Auditd monitoring for sensitive resources
- Investigate SSH authentication activity
- Review privileged sudo activity
- Validate AppArmor mandatory access controls
- Practice Linux process and network administration
- Develop Bash security and administration scripts
- Automate recurring reporting with cron
- Perform a final hardened-server security assessment

---

# 🏗️ Lab Environment

| System | Role | Management IP |
|---|---|---|
| Ubuntu Linux | Enterprise Linux Server | `192.168.50.10` |
| Kali Linux | Administration / Security Validation Workstation | `192.168.50.20` |
| Oracle VirtualBox | Virtualization Platform | N/A |

Each virtual machine used two network interfaces:

| Interface | Purpose |
|---|---|
| NAT | Internet connectivity, updates, and package installation |
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
              +-------------+-------------+
                            |
                         LinuxLab
                     Internal Network
                      192.168.50.0/24

                 Ubuntu: 192.168.50.10
                 Kali:   192.168.50.20
