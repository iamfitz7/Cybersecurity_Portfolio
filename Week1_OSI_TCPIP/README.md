# Week 1 – Networking Foundations

This week focused on building a strong foundation in basic IP addressing and inter-VLAN routing using Cisco Packet Tracer. The labs demonstrate both same-network communication and communication across different VLANs using router-on-a-stick.

---

## Lab 1 – IP Addressing Basics Lab

### Objectives
- Assign IP addresses to two PCs in the same /24 subnet  
- Test connectivity using `ping`  
- Understand basic same-network device communication  

### Key Steps
1. Assigned IP addresses to PC0 and PC1.
2. Verified both devices were in the same subnet.
3. Used `ping` to test connectivity between the two PCs.

### Validation
- Successful ping replies confirmed:
  - Correct IP addressing
  - Proper network connectivity
  - Devices communicating within the same subnet

---

## Lab 2 – Inter-VLAN Routing, Ping & Traceroute (Router-on-a-Stick)

### Objectives
- Create VLAN 10 and VLAN 20
- Assign switch ports to correct VLANs
- Configure trunking between switch and router
- Configure router subinterfaces
- Verify inter-VLAN connectivity using `ping` and `traceroute`

### Network Design
- **VLAN 10** → 192.168.10.0/24  
- **VLAN 20** → 192.168.20.0/24  
- One Layer 2 switch  
- One router using router-on-a-stick  
- One trunk link between switch and router  

### Key Configuration Steps

**On the Switch**
- Created VLAN 10 and VLAN 20
- Assigned access ports to the correct VLANs
- Configured the router-facing port as a trunk

**On the Router**
- Created subinterfaces for each VLAN
- Assigned default gateway IPs to each VLAN
- Enabled 802.1Q encapsulation on each subinterface

**On the PCs**
- Assigned IP addresses in the correct subnet
- Configured the correct default gateway

---

### Testing & Verification

- `ping` from VLAN 10 to VLAN 20 was successful  
- `traceroute` showed:
  - Hop 1 → Router subinterface (default gateway)
  - Hop 2 → Destination PC in the other VLAN  

✅ This confirms:
- VLANs are correctly configured  
- The trunk is functioning  
- Subinterfaces are correct  
- Inter-VLAN routing is working  
- ICMP is successfully validating connectivity  

---

## Files Included in This Lab

- `PC0_IP_Config.png` – PC0 IP configuration  
- `PC1_IP_Config.png` – PC1 IP configuration  
- `Switch_VLAN_Creation.png` – VLAN creation on the switch  
- `Switch_VLAN_Assignment.png` – Port assignments  
- `Switch_Trunk_Config.png` – Trunk configuration  
- `Router_Subinterface_Config.png` – Router-on-a-stick configuration  
- `Ping_Test_PC0_to_Router.png` – Ping verification  
- `Ping_Test_PC1_to_Router.png` – Ping verification  
- `Week1_PingTraceroute_Topology.png` – Network topology  
- `Week1_PingTraceroute_VLAN_Lab.pkt` – Packet Tracer lab file  

---

## Skills Demonstrated (Week 1)

- IP addressing (/24)
- VLAN creation & port assignment
- Trunk configuration
- Router-on-a-stick inter-VLAN routing
- ICMP testing (`ping`, `traceroute`)
