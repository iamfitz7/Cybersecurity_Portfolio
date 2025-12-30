# 🔥 Network Defense Foundations — Firewalls, NAT & Intrusion Detection

This repository documents hands-on labs focused on **core defensive network controls**, specifically firewalls and intrusion detection systems (IDS).

The purpose of this work is to understand **how traffic is permitted, blocked, translated, and monitored**, and how these controls operate together to reduce risk. The labs emphasize reasoning about traffic behavior, rule logic, and alert generation rather than simple tool configuration.

These concepts form the backbone of perimeter defense and security monitoring.

---

## 🎯 Learning Goals

Through these labs, the objectives are to:

- Understand **stateful vs stateless firewalls**
- Design and explain **packet filtering rules**
- Apply **NAT concepts** within firewall configurations
- Distinguish between **IDS and IPS**
- Understand **signature-based vs anomaly-based detection**
- Generate and analyze IDS alerts
- Combine firewall enforcement with IDS visibility
- Document security behavior clearly using evidence

---

## 🧱 Firewall Fundamentals

These labs focus on firewall behavior and traffic control:

- Understanding stateless packet filtering
- Understanding stateful connection tracking
- Comparing use cases for each approach
- Reasoning about how firewalls evaluate traffic decisions

🧠 **Why this matters:**  
Firewalls are not just “allow or block” devices. Understanding *state* is critical to explaining why traffic is permitted or denied.

---

## 🚦 Packet Filtering & Rule Logic

These labs examine how firewall rules are written and enforced:

- Creating allow and block rules
- Blocking specific traffic (e.g., ICMP echo requests)
- Observing rule order and evaluation logic
- Validating rule behavior through testing

📸 *Artifacts added:*  
- Firewall rule screenshots  
- Traffic validation results  

🧠 **Why this matters:**  
Poorly written rules can unintentionally expose services or block legitimate traffic. Rule clarity is essential for secure operations.

---

## 🔁 Network Address Translation (NAT)

These labs focus on NAT behavior within firewalls:

- Configuring NAT rules
- Understanding address translation mechanics
- Observing how internal traffic appears externally
- Explaining how NAT interacts with firewall rules

📸 *Artifacts added:*  
- NAT rule configuration screenshots  

🧠 **Why this matters:**  
NAT is foundational to modern networks and directly affects visibility, logging, and security monitoring.

---

## 🚨 Intrusion Detection vs Intrusion Prevention

These labs introduce intrusion detection concepts:

- Understanding the difference between IDS and IPS
- Comparing:
  - Signature-based detection
  - Anomaly-based detection
- Exploring alert-based visibility vs traffic blocking

🧠 **Why this matters:**  
Security teams must understand whether a system is **detecting**, **alerting**, or **actively blocking** traffic.

---

## 🧪 IDS Alert Generation & Analysis

These labs focus on generating and interpreting IDS alerts:

- Configuring IDS alert rules
- Simulating suspicious activity
- Triggering alerts through controlled scans
- Documenting alert details and context

📸 *Artifacts added:*  
- IDS alert screenshots  
- Alert metadata and explanation  

🧠 **Why this matters:**  
Alerts without context are noise. Analysts must understand *why* an alert fired before deciding what to do next.

---

## 🧩 Firewall + IDS Integration

These labs combine multiple defensive controls:

- Enforcing traffic policy with firewall rules
- Monitoring traffic using IDS
- Observing how firewall decisions affect IDS visibility
- Capturing evidence of both enforcement and detection

📸 *Artifacts added:*  
- Firewall rule screenshots  
- IDS alerts tied to specific traffic  

🧠 **Why this matters:**  
Defense-in-depth relies on layered controls. Firewalls restrict access, while IDS provides visibility into what is allowed.

---

## 🧠 Review & Concept Reinforcement

This repository represents a consolidated view of defensive fundamentals:

- Clear understanding of firewall behavior
- Practical application of packet filtering and NAT
- Foundational IDS alert analysis
- Integration of enforcement and monitoring

🧠 **Why this matters:**  
Strong defenders understand both **prevention and detection**, and how they complement each other.

---

## 🛠️ Tools & Technologies

- **pfSense** — Firewall and NAT configuration  
- **Suricata** — Intrusion Detection System  
- **ICMP and TCP/IP traffic**  
- **Basic attack simulation for alert generation**

---

## 📁 Repository Structure

```text
/
├── firewall/
│   ├── rules/
│   └── screenshots/
├── nat/
│   ├── configs/
│   └── explanations/
├── ids/
│   ├── alerts/
│   └── analysis/
├── integration/
│   └── firewall-ids/
└── README.md
