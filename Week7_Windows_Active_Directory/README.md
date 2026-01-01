# 🖥️ Active Directory Basics — Users, Groups & Policy Control

This repository documents hands-on labs focused on **Microsoft Active Directory (AD)** and its role in managing users, access, and security in an enterprise environment.

The goal of this work is to understand how identity is structured, how policies are enforced, and how activity is logged and reviewed. Active Directory is a core system in most organizations and is often involved in both security monitoring and investigations.

---

## 🎯 Learning Goals

Through these labs, the objectives are to:

- Understand how Active Directory is structured
- Manage users and groups in a domain environment
- Organize objects using Organizational Units (OUs)
- Create and apply Group Policy Objects (GPOs)
- Enforce basic security policies
- Review authentication activity through system logs
- Understand how AD integrates with other security controls

---

## 🧠 Active Directory Structure & Concepts

These labs focus on the basic structure of Active Directory:

- Domains and forests
- Organizational Units (OUs)
- Users and security groups
- How objects are organized and managed

🧠 **Why this matters:**  
Active Directory controls who can access systems and resources. Understanding its structure is critical for both system administration and security monitoring.

---

## 👥 User & Group Management

These labs involve creating and managing users and groups:

- Creating domain user accounts
- Creating security groups
- Placing users and groups into OUs
- Understanding how group membership affects access

📸 *Artifacts added:*  
- Screenshots of user and group creation  
- OU structure examples  

🧠 **Why this matters:**  
Incorrect user or group setup can lead to access issues or security risks. Clear organization helps reduce mistakes and improve visibility.

---

## 📋 Group Policy Objects (GPOs)

These labs introduce Group Policy Objects:

- Creating GPOs
- Linking policies to specific OUs
- Applying system and security settings
- Verifying that policies are enforced

📸 *Artifacts added:*  
- Screenshots showing GPOs applied  
- Evidence of policy enforcement  

🧠 **Why this matters:**  
GPOs allow administrators to enforce consistent rules across many systems, which is critical for maintaining security standards.

---

## 🔐 Active Directory Hardening

These labs focus on basic AD security hardening:

- Enforcing password policies through GPO
- Applying stronger authentication rules
- Reviewing the impact of policies on users

📸 *Artifacts added:*  
- Updated policy screenshots  
- Notes explaining security benefits  

🧠 **Why this matters:**  
Weak policies make accounts easy targets. Basic hardening helps reduce common attack paths.

---

## 📊 Auditing & Log Review

These labs focus on reviewing authentication activity:

- Using Windows Event Viewer
- Identifying failed login attempts
- Understanding where AD-related events are logged
- Capturing evidence of authentication activity

📸 *Artifacts added:*  
- Screenshots of failed login events  

🧠 **Why this matters:**  
Login activity is often the first sign of a security issue. Being able to read these logs is important for investigations.

---

## 🔗 Integrating Active Directory with Other Controls

These labs combine AD with other systems:

- Connecting domain access with secure remote access
- Observing how identity ties into network security
- Understanding the security benefit of centralized authentication

📸 *Artifacts added:*  
- Screenshots showing integrated setup  

🧠 **Why this matters:**  
Centralized identity management strengthens security and makes monitoring easier across systems.

---

## 🛠️ Tools & Technologies

- Windows Server (Active Directory Domain Services)
- Windows client systems
- Group Policy Management
- Windows Event Viewer
- Virtual machine lab environment

---

## 📁 Repository Structure

```text
/
├── ad-basics/
│   ├── users-groups/
│   └── ou-structure/
├── gpo/
│   ├── policies/
│   └── enforcement/
├── logs/
│   └── authentication-events/
├── integration/
│   └── ad-network/
└── README.md
