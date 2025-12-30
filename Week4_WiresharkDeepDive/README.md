# 🔍 Packet Analysis Mastery — Wireshark Deep Dive

This repository documents in-depth packet analysis labs focused on **understanding, interpreting, and validating network traffic using Wireshark**.

The purpose of this work is to move beyond simply capturing packets and develop the ability to **explain what normal traffic looks like, how protocols behave, and why certain patterns matter from a security and troubleshooting perspective**.

Packet-level visibility is a core skill for SOC analysts, network engineers, and security practitioners.

---

## 🎯 Learning Goals

Through these labs, the objectives are to:

- Analyze **TCP and UDP traffic** at the packet level
- Interpret **connection-oriented vs connectionless behavior**
- Capture and explain **DNS resolution traffic**
- Observe **HTTP request and response flows**
- Correlate multiple protocols within a single session
- Apply effective **Wireshark filtering techniques**
- Communicate packet analysis findings clearly and professionally

---

## 🔁 TCP Session Analysis

These labs focus on analyzing TCP session establishment:

- Capturing the TCP three-way handshake
- Identifying:
  - SYN
  - SYN-ACK
  - ACK
- Understanding sequence numbers and acknowledgments
- Explaining how reliable connections are established

📸 *Artifacts added:*  
- TCP handshake screenshots  
- Annotated packet views  

🧠 **Why this matters:**  
Many attacks and failures occur during session setup. Understanding normal handshake behavior is essential for detecting anomalies.

---

## 🔄 UDP Packet Analysis

These labs examine UDP traffic behavior:

- Capturing UDP packets
- Observing lack of session establishment
- Comparing reliability and overhead to TCP
- Understanding when UDP is preferred

📸 *Artifacts added:*  
- UDP packet screenshots  

🧠 **Why this matters:**  
UDP is widely used in DNS, streaming, and real-time applications. Analysts must recognize normal UDP behavior to avoid misinterpreting traffic.

---

## 🌍 DNS Traffic Analysis

These labs analyze DNS lookups in detail:

- Capturing DNS queries and responses
- Identifying query types and response codes
- Annotating key packet fields
- Observing timing and resolution patterns

📸 *Artifacts added:*  
- DNS query/response screenshots  
- Annotated packet details  

🧠 **Why this matters:**  
DNS traffic is a frequent indicator of compromise. Knowing what “normal” looks like is critical for detection.

---

## 🌐 HTTP Request & Response Analysis

These labs focus on application-layer traffic:

- Capturing HTTP GET and POST requests
- Observing request headers and payloads
- Analyzing server responses and status codes
- Tracing full request/response flows

📸 *Artifacts added:*  
- HTTP traffic screenshots  
- Request/response walkthroughs  

🧠 **Why this matters:**  
Web traffic analysis helps identify data exfiltration, misconfigurations, and malicious activity.

---

## 🧩 Multi-Protocol Correlation

These labs combine multiple protocols in a single analysis:

- Observing TCP, UDP, DNS, and HTTP together
- Correlating protocol dependencies
- Explaining full traffic flows from resolution to application data
- Highlighting basic security implications

📸 *Artifacts added:*  
- Combined capture screenshots  
- Annotated flow explanations  

🧠 **Why this matters:**  
Real investigations involve **multiple protocols simultaneously**. Correlation skills separate beginners from professionals.

---

## 🎯 Filtering & Analysis Efficiency

These labs emphasize analysis efficiency:

- Filtering by protocol
- Filtering by port number
- Filtering by IP address
- Reducing capture noise
- Building repeatable analysis workflows

📸 *Artifacts added:*  
- Filtered Wireshark captures  

🧠 **Why this matters:**  
SOC analysts rarely have time to review entire captures. Efficient filtering is critical for timely response.

---

## 🧠 Portfolio Consolidation

This repository represents a consolidated view of packet analysis skills:

- Captured and analyzed TCP, UDP, DNS, and HTTP traffic
- Explained protocol behavior and interactions
- Applied filtering techniques to isolate relevant traffic
- Documented observations clearly for portfolio review

🧠 **Why this matters:**  
Clear documentation demonstrates not just technical ability, but **analytical thinking and communication skills**.

---

## 🛠️ Tools & Technologies

- **Wireshark** — Packet capture and analysis  
- **TCP, UDP, DNS, HTTP protocols**  
- **OSI & TCP/IP models**

---

## 📁 Repository Structure

```text
/
├── tcp/
│   ├── handshake/
│   └── analysis/
├── udp/
│   └── captures/
├── dns/
│   ├── queries/
│   └── responses/
├── http/
│   ├── requests/
│   └── responses/
├── combined/
│   └── flows/
└── README.md
