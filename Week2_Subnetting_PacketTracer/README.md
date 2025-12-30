# 📡 Networking Foundations — Subnetting, VLANs & Packet Analysis

This repository documents hands-on labs focused on **advanced subnetting, network segmentation, and packet-level analysis**.

The goal of this work is to move beyond basic connectivity and begin **reasoning about how networks are structured, segmented, and observed at the protocol level**. These labs emphasize understanding *why* traffic behaves the way it does, not just that it works.

This knowledge is foundational for networking, security operations, and incident investigation.

---

## 🎯 Learning Goals

Through these labs, the objectives are to:

- Subnet **larger address spaces** accurately
- Identify **network, broadcast, and usable host addresses**
- Design and explain **VLAN-based segmentation**
- Understand **TCP vs UDP behavior**
- Observe packet flow across **OSI layers**
- Analyze **DNS traffic** at the packet level
- Explain network behavior clearly using captured evidence

---

## 📐 Advanced Subnetting & Address Analysis

These labs focus on subnetting beyond small networks:

- Subnetting larger CIDR ranges (e.g., /16, /22)
- Calculating:
  - Network address
  - Broadcast address
  - Usable host range
- Assigning IPs based on subnet design
- Validating subnet logic through configuration

📸 *Artifacts added:*  
- Subnetting examples  
- IP assignment screenshots  

🧠 **Why this matters:**  
Large networks fail when addressing plans are unclear. Accurate subnetting enables scalability, troubleshooting, and secure segmentation.

---

## 🧭 Network vs Host Addressing

These labs reinforce how addressing boundaries work:

- Distinguishing between:
  - Network addresses
  - Broadcast addresses
  - Valid host addresses
- Assigning IPs correctly in simulated environments
- Observing what happens when invalid addresses are used

📸 *Artifacts added:*  
- Device IP configurations  
- Connectivity validation  

🧠 **Why this matters:**  
Misunderstanding address boundaries leads to silent failures that are difficult to diagnose.

---

## 🧩 VLANs & Network Segmentation

These labs introduce VLAN-based segmentation:

- Creating multiple VLANs
- Assigning devices to VLANs
- Understanding VLAN IDs and logical separation
- Visualizing segmentation using diagrams
- Testing connectivity within and across VLANs

📸 *Artifacts added:*  
- VLAN configuration screenshots  
- Network diagrams  

🧠 **Why this matters:**  
VLANs separate traffic logically without additional hardware. They are fundamental to security, performance, and access control.

---

## 🔁 TCP vs UDP Traffic Analysis

These labs analyze transport-layer behavior using packet capture:

- Capturing TCP and UDP traffic
- Filtering packets by protocol
- Observing reliability vs speed trade-offs
- Comparing connection-oriented and connectionless communication

📸 *Artifacts added:*  
- Wireshark captures filtered by TCP and UDP  

🧠 **Why this matters:**  
Different applications require different transport behaviors. Understanding this distinction helps explain latency, loss, and performance issues.

---

## 📶 Packet Flow Across OSI Layers

These labs focus on how packets move through the stack:

- Observing the TCP three-way handshake
- Identifying SYN, SYN-ACK, and ACK packets
- Mapping packet behavior to OSI layers
- Understanding session establishment at a protocol level

📸 *Artifacts added:*  
- TCP handshake screenshots  
- Annotated packet views  

🧠 **Why this matters:**  
Many network and security incidents occur during session setup. Recognizing normal handshake behavior is critical for identifying anomalies.

---

## 🌐 DNS Query Analysis

These labs examine DNS communication:

- Capturing DNS queries and responses
- Observing name resolution in real time
- Understanding query/response mechanics
- Identifying request types and responses

📸 *Artifacts added:*  
- DNS packet captures  
- Query/response analysis  

🧠 **Why this matters:**  
DNS is often targeted in attacks and outages. Analysts must understand normal DNS behavior before they can detect abuse.

---

## 🛠️ Tools & Technologies

- **Cisco Packet Tracer** — Network simulation and validation  
- **Wireshark** — Packet capture and protocol analysis  
- **IPv4 & CIDR notation**  
- **VLAN segmentation concepts**  

---
