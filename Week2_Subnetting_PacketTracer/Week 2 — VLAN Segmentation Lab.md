Week 2 — VLAN Segmentation Lab
1. Overview

This lab focuses on understanding how Virtual Local Area Networks (VLANs) segment a network at Layer 2. I created two VLANs on a Cisco switch, assigned ports to each VLAN, configured IP addresses on PCs, and tested communication to verify segmentation behavior.

This lab reinforces core networking concepts used in real environments, such as broadcast domain reduction, traffic separation, and switch port configuration.

2. Topology

Devices Used

1 × Cisco 2960 Switch

4 × PCs

Copper Straight-Through cables

VLAN Assignments

VLAN 10 (SALES): PC0, PC1

VLAN 20 (HR): PC2, PC3

Screenshot:

Topology_VLAN_Lab.png

3. VLAN Creation

Two VLANs were created using Cisco CLI commands:

vlan 10
name SALES

vlan 20
name HR


These VLANs represent separate logical LANs inside a single physical switch.

Screenshot:

Switch_VLAN_Creation.png

4. Port Assignments

Ports were mapped to their respective VLANs:

VLAN 10 (SALES)

Ports: Fa0/1, Fa0/2

VLAN 20 (HR)

Ports: Fa0/3, Fa0/4

Configuration:

interface range fastEthernet0/1 - 2
 switchport mode access
 switchport access vlan 10
exit

interface range fastEthernet0/3 - 4
 switchport mode access
 switchport access vlan 20
exit


Screenshot:

Switch_VLAN_Assignment.png

5. VLAN Verification

To confirm VLANs and port mappings, the following command was used:

show vlan brief


This verified that ports were correctly assigned to VLAN 10 and VLAN 20.

Screenshot:

VLAN_Show_Brief.png

6. IP Addressing

Each VLAN used a separate subnet to simulate real-world segmentation:

VLAN 10 (192.168.10.0/24)

PC0 → 192.168.10.10

PC1 → 192.168.10.11

VLAN 20 (192.168.20.0/24)

PC2 → 192.168.20.10

PC3 → 192.168.20.11

Subnet Mask (all PCs): 255.255.255.0
Default Gateway: Not used in this lab

Screenshots:

PC0_IP_Config.png

PC2_IP_Config.png

7. Ping Test Results
VLAN 10 Communication

PC0 → PC1 = Successful

PC0 → VLAN 20 devices = Blocked

VLAN 20 Communication

PC2 → PC3 = Successful

PC2 → VLAN 10 devices = Blocked

Screenshot:

Ping_Test_Results.png

8. What This Lab Demonstrates

VLANs separate devices into different broadcast domains

Devices inside the same VLAN can communicate normally

Devices in different VLANs cannot communicate without a router

VLANs provide better structure, security, and traffic control

Layer 2 segmentation is a foundation for enterprise network design

9. Skills Demonstrated

VLAN creation and naming

Switch port configuration using access mode

Mapping switch interfaces to VLANs

Subnetting for segmented networks

Basic troubleshooting using pings

Using CLI commands like show vlan brief

10. Files Included

Week2_VLAN_Segmentation_Lab.pkt

Topology_VLAN_Lab.png

Switch_VLAN_Creation.png

Switch_VLAN_Assignment.png

VLAN_Show_Brief.png

PC0_IP_Config.png

PC2_IP_Config.png

Ping_Test_Results.png