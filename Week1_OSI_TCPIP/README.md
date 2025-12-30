# 🌐 Networking Fundamentals — OSI, TCP/IP, IP Addressing & Subnetting

This repository documents hands-on labs focused on **core networking fundamentals** that underpin all modern networking and security work.

The emphasis of these labs is not memorization alone, but **understanding how data moves through networks**, how addressing and subnetting shape communication, and how foundational concepts appear in real troubleshooting scenarios.

These skills form the baseline for networking, security operations, and infrastructure roles.

---

## 🎯 Learning Goals

Through these labs, the objectives are to:

- Understand the **OSI and TCP/IP models** and how they relate
- Identify **protocols and responsibilities at each layer**
- Assign and validate **IPv4 addressing**
- Distinguish between public, private, and special-use IP ranges
- Perform **subnetting without calculators**
- Use basic tools like **ping and traceroute** to validate connectivity
- Explain observed network behavior clearly and accurately

---

## 🧠 OSI Model & Layered Communication

These labs focus on the **7-layer OSI model** and how it structures network communication:

- Identifying each OSI layer and its function
- Mapping common protocols to the correct layers
- Understanding how data flows top-down and bottom-up
- Recognizing where issues occur when troubleshooting

🧠 **Why this matters:**  
The OSI model provides a shared language for diagnosing problems. Knowing *where* something breaks is often more important than knowing *what* breaks.

---

## 🔁 OSI vs TCP/IP Model

These labs examine how the **TCP/IP model** relates to OSI:

- Comparing OSI’s 7 layers to TCP/IP’s condensed structure
- Mapping OSI layers to their TCP/IP equivalents
- Understanding why TCP/IP is used in practice
- Learning how both models are referenced in real environments

🧠 **Why this matters:**  
Engineers and analysts move between both models constantly. Understanding the mapping prevents confusion during troubleshooting and documentation.

---

## 🖧 IP Addressing Fundamentals

These labs focus on IPv4 addressing concepts:

- Understanding IP classes
- Distinguishing between public and private address ranges
- Identifying special-use addresses (e.g., APIPA)
- Assigning IP addresses to hosts within the same subnet
- Verifying connectivity between devices

📸 *Artifacts added:*  
- IP configuration screenshots  
- Successful ping validation  

🧠 **Why this matters:**  
Incorrect IP addressing is one of the most common causes of network failure. Clear addressing prevents outages and simplifies troubleshooting.

---

## 📐 Subnetting & Network Segmentation

These labs build subnetting skills incrementally:

- Subnetting common CIDR ranges (/24, /25, /26, /28)
- Calculating network IDs, usable hosts, and broadcast addresses
- Practicing subnetting **without calculators**
- Applying subnetting to practical network scenarios
- Using VLAN-based labs to reinforce segmentation concepts

📸 *Artifacts added:*  
- Subnetting examples  
- IP configuration and validation screenshots  
- Ping tests across defined segments  

🧠 **Why this matters:**  
Subnetting controls scale, performance, and security. Engineers must be able to reason about address space quickly and accurately.

---

## 📡 ICMP, Ping & Traceroute

These labs use basic diagnostic tools to observe network behavior:

- Understanding ICMP’s role in connectivity testing
- Using `ping` to verify reachability
- Using `traceroute` to observe packet paths
- Interpreting latency, timeouts, and unreachable responses
- Testing connectivity across VLANs

📸 *Artifacts added:*  
- Ping and traceroute outputs  
- VLAN connectivity validation  

🧠 **Why this matters:**  
Ping and traceroute are often the **first tools used in incident response**. Correct interpretation is essential to avoid false conclusions.

---

## 🛠️ Tools & Technologies

- **Cisco Packet Tracer** — Network simulation and validation  
- **ICMP utilities** — Ping and traceroute  
- **IPv4 addressing & CIDR notation**

---
