# 🔐 Network Security Labs — Firewall, IDS, and VPN

This repository documents hands-on labs focused on **core network security controls** used in real environments: firewalls, intrusion detection systems (IDS), and VPNs.

The purpose of this work is not just to configure tools, but to **understand how security controls behave, interact, and are validated** in practice. Each lab emphasizes reasoning, verification, and clear documentation rather than rote setup.

---

## 🎯 Learning Goals

Through these labs, the objectives are to:

- Design and analyze **firewall rule logic and priority**
- Understand **why IDS tuning is essential** for meaningful detection
- Explain how **VPNs protect data in transit**
- Verify encryption using **packet capture analysis**
- Integrate firewall, VPN, and IDS into a **layered security model**
- Communicate security decisions clearly for documentation and portfolio use

---

## 🔥 Firewall Configuration & Rule Logic

These labs focus on advanced firewall concepts using **pfSense**:

- Creating multiple firewall rules with overlapping conditions
- Understanding rule evaluation order and priority
- Observing how traffic is accepted, rejected, or blocked
- Identifying how misordered rules can weaken security posture

📸 *Artifacts added:*  
- Advanced firewall rule configurations  
- Rule priority and matching behavior  

🧠 **Why this matters:**  
Firewalls enforce the first line of defense. Clear, predictable rule logic is critical to prevent unintended access or outages.

---

## 🧠 Intrusion Detection & Alert Tuning

These labs explore **Suricata IDS** with an emphasis on alert quality:

- Reviewing default alert behavior
- Adjusting thresholds and rule sensitivity
- Reducing false positives
- Improving signal-to-noise ratio for investigations

📸 *Artifacts added:*  
- IDS alert samples  
- Tuned Suricata rule examples  

🧠 **Why this matters:**  
An IDS that generates excessive alerts becomes ignored. Tuning is what turns raw detection into **actionable intelligence**.

---

## 🔒 VPN Concepts & Secure Tunneling

These labs focus on understanding and implementing **VPN-based encryption**:

- Studying how VPNs encrypt traffic over untrusted networks
- Understanding tunneling, authentication, and key exchange
- Implementing an **OpenVPN server and client**
- Establishing a secure encrypted tunnel

📸 *Artifacts added:*  
- OpenVPN configuration and connection status  

🧠 **Why this matters:**  
VPNs protect sensitive data in motion. Security engineers must understand both **how they work** and **how to confirm they are working correctly**.

---

## 🔍 Traffic Analysis & Encryption Verification

These labs use **Wireshark** to validate security controls:

- Capturing VPN traffic
- Applying filters to isolate tunneled packets
- Verifying that payloads are encrypted and unreadable
- Demonstrating encryption through packet inspection

📸 *Artifacts added:*  
- Filtered packet captures  
- Encrypted payload verification  

🧠 **Why this matters:**  
Security should be provable. Packet analysis provides direct evidence that encryption and controls are functioning as intended.

---

## 🧩 Integrated Security Architecture

These labs combine multiple controls into a single defensive system:

- Firewall rules governing VPN traffic
- IDS monitoring encrypted and tunneled traffic
- Observing how controls interact rather than operate in isolation

📸 *Artifacts added:*  
- pfSense firewall rules with VPN interfaces  
- IDS monitoring alongside VPN traffic  
- System overview showing all components active  

🧠 **Why this matters:**  
Real-world security relies on **defense in depth**. Effective protection comes from how systems work together, not from any single tool.

---

## 🛠️ Tools & Technologies

- **pfSense** — Firewall and routing  
- **Suricata** — Intrusion Detection System  
- **OpenVPN** — Secure tunneling  
- **Wireshark** — Packet analysis   

---
