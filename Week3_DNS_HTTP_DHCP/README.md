# 🌐 Network Services & Traffic Analysis — DNS, HTTP, and DHCP

This repository documents hands-on labs focused on **core network services** and how they appear during real packet-level analysis.

The emphasis of this work is understanding **how foundational services operate on the wire**, how they depend on one another, and how their behavior is validated through packet capture. These services are present in nearly every network interaction and frequently appear in troubleshooting and security investigations.

---

## 🎯 Learning Goals

Through these labs, the objectives are to:

- Understand **DNS resolution mechanics**
- Analyze **HTTP and HTTPS traffic**
- Observe the **DHCP lease process**
- Capture and filter traffic effectively in Wireshark
- Correlate multiple protocols in a single network flow
- Map observed traffic to **OSI and TCP/IP models**
- Explain packet behavior clearly using captured evidence

---

## 🌍 DNS Resolution & Name Lookup

These labs focus on how DNS resolves domain names to IP addresses:

- Understanding recursive vs iterative resolution
- Capturing DNS queries and responses
- Observing query types and response codes
- Tracing resolution steps through packet analysis

📸 *Artifacts added:*  
- DNS query and response captures  
- Annotated packet views  

🧠 **Why this matters:**  
DNS is a dependency for most network activity and a common attack vector. Analysts must understand normal resolution behavior before identifying misuse or failure.

---

## 🌐 HTTP & HTTPS Traffic Analysis

These labs examine application-layer web traffic:

- Capturing browser-based HTTP and HTTPS requests
- Identifying GET and POST requests
- Observing request and response headers
- Interpreting HTTP status codes
- Understanding the visibility difference between HTTP and HTTPS

📸 *Artifacts added:*  
- HTTP request/response captures  
- Header and status code examples  

🧠 **Why this matters:**  
Web traffic dominates modern networks. Understanding how requests are formed and responses are delivered is essential for troubleshooting and security monitoring.

---

## 📡 DHCP Address Assignment

These labs focus on dynamic IP assignment using DHCP:

- Observing the DHCP lease process
- Identifying discover, offer, request, and acknowledgment messages
- Assigning IP addresses to hosts dynamically
- Verifying successful lease assignment

📸 *Artifacts added:*  
- DHCP IP assignment screenshots  
- Lease exchange validation  

🧠 **Why this matters:**  
Without DHCP, most networks fail silently. Understanding lease behavior helps diagnose connectivity issues and detect rogue devices.

---

## 🔗 Correlating DNS, HTTP & Transport Traffic

These labs combine multiple protocols into a single analysis:

- Capturing DNS followed by HTTP requests
- Observing protocol dependencies
- Following packet flow from name resolution to web request
- Annotating important packet fields

📸 *Artifacts added:*  
- Combined DNS + HTTP captures  
- Annotated packet walkthroughs  

🧠 **Why this matters:**  
Real investigations rarely involve one protocol. Analysts must correlate multiple layers to understand what actually happened.

---

## 🔍 Traffic Filtering & Analysis Techniques

These labs emphasize efficient packet analysis:

- Filtering traffic by IP address
- Filtering by protocol (DNS, HTTP, TCP)
- Reducing noise to isolate relevant packets
- Building repeatable filtering workflows

📸 *Artifacts added:*  
- Filtered Wireshark captures  

🧠 **Why this matters:**  
Large captures are unusable without filtering. Effective analysis depends on isolating what matters quickly.

---

## 🧠 End-to-End Packet Flow Review

These labs reinforce full-stack understanding:

- Observing TCP session establishment
- Mapping DNS resolution to application requests
- Tracing HTTP traffic over established connections
- Mapping observed packets to OSI and TCP/IP layers

🧠 **Why this matters:**  
Security and networking professionals must reason across layers, not just protocols in isolation.

---

## 🛠️ Tools & Technologies

- **Wireshark** — Packet capture and protocol analysis  
- **Cisco Packet Tracer** — DHCP simulation and validation  
- **DNS, HTTP/HTTPS, DHCP protocols**  
- **TCP/IP & OSI models**

---

## 📁 Repository Structure

```text
/
├── dns/
│   ├── captures/
│   └── analysis/
├── http/
│   ├── requests/
│   └── responses/
├── dhcp/
│   ├── leases/
│   └── screenshots/
├── combined-analysis/
│   ├── dns-http/
│   └── annotations/
└── README.md
