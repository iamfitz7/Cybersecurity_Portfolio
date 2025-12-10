Week 2 — Network, Broadcast & Host Address Lab
Objective

The goal of this lab was to practice identifying network, broadcast, and host addresses by creating a small LAN in Packet Tracer. I assigned IP addresses to a router and three PCs, then tested connectivity to make sure everything was configured correctly. This helped reinforce how devices communicate inside the same subnet and what role the default gateway plays.

1. Topology Overview

The network includes:

1 Router (Router0)

1 Switch (Switch0)

3 PCs (PC0, PC1, PC2)

All devices are connected using Copper Straight-Through cables.

Subnet Used: 192.168.50.0/24
Subnet Mask: 255.255.255.0

2. Subnet Breakdown
Item	Address
Network Address	192.168.50.0
Broadcast Address	192.168.50.255
Usable Host Range	192.168.50.1 – 192.168.50.254
Why these matter

The network address identifies the whole subnet.

The broadcast address is used to send traffic to all devices in the subnet.

Only the addresses between those two are valid for hosts like PCs, routers, and servers.

3. IP Address Assignments
Device	IP Address	Subnet Mask	Default Gateway
Router0 (G0/0)	192.168.50.1	255.255.255.0	—
PC0	192.168.50.10	255.255.255.0	192.168.50.1
PC1	192.168.50.20	255.255.255.0	192.168.50.1
PC2	192.168.50.30	255.255.255.0	192.168.50.1

These are all valid host addresses. The router’s address (192.168.50.1) is used as the default gateway so devices know where to send traffic outside their local network.

4. Router Interface Status

Interface used: GigabitEthernet0/0

IP Address: 192.168.50.1
Subnet Mask: 255.255.255.0
Status: Up/Up


This confirms the router interface is enabled and ready to route traffic if needed.

5. Connectivity Tests

All pings from PC0 were successful:

PC0 → PC1
ping 192.168.50.20
Reply from 192.168.50.20

PC0 → PC2
ping 192.168.50.30
Reply from 192.168.50.30

PC0 → Router0 (Gateway)
ping 192.168.50.1
Reply from 192.168.50.1

What this shows

All devices received valid host IP addresses.

The router and PCs are in the same subnet.

The default gateway is reachable.

No device was accidentally assigned the network or broadcast address.

6. Screenshots Included (Folder Files)

Topology.png

PC0_IP_Config.png

PC1_IP_Config.png

Router_Interface_Status.png

(Optional) Ping test screenshot

These provide visual proof of how the lab was set up and configured.

7. Summary / Reflection

This lab was helpful for reviewing how IPv4 addressing works, especially:

Understanding network vs. host vs. broadcast addresses

Assigning IPs correctly within a subnet

Setting up a default gateway

Testing connectivity using ping

These skills are essential for networking and cybersecurity roles because almost every troubleshooting situation involves checking IP addresses, subnet masks, and gateway configuration.