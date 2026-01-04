# 🖥️ Active Directory — Log Auditing & Secure Access Integration

This section focuses on **monitoring Active Directory activity** and **integrating identity with secure remote access**.

The goal of these labs is to understand how authentication activity is logged, how failed logins appear in system logs, and how Active Directory works together with secure access solutions to strengthen security.

---

## 🎯 Learning Goals

Through these labs, the objectives are to:

- Review Active Directory authentication logs
- Identify failed login attempts in Windows Event Viewer
- Understand why login auditing is important
- Integrate Active Directory with secure remote access
- Explain the security benefit of centralized identity control

---

## 📊 Auditing Active Directory Logs

These labs focus on reviewing authentication-related logs in **Windows Event Viewer**:

- Locating security logs related to logins
- Identifying failed login attempts
- Reviewing event details such as:
  - Username
  - Timestamp
  - Failure reason
- Capturing evidence of authentication activity

📸 *Artifacts added:*  
- Screenshots showing failed login attempts in Event Viewer  

🧠 **Why this matters:**  
Failed login attempts are often an early sign of misconfiguration or suspicious behavior. Being able to find and understand these logs is important for security monitoring and investigations.

---

## 🔗 Active Directory & Secure Access Integration

These labs combine identity management with secure remote access:

- Integrating Active Directory authentication with a secure connection
- Observing how users authenticate before gaining network access
- Verifying that identity checks happen before access is allowed

📸 *Artifacts added:*  
- Screenshot showing Active Directory working with secure access  

🧠 **Why this matters:**  
Combining centralized identity management with secure access helps ensure that only authorized users can connect to internal resources, even when connecting remotely.

---

## 🛠️ Tools & Technologies

- Windows Server (Active Directory)
- Windows Event Viewer
- Secure remote access solution
- Virtual machine lab environment

---

## 📁 Repository Structure

```text
/
├── ad-logs/
│   └── failed-logins/
├── integration/
│   └── ad-secure-access/
└── README.md
