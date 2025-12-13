# Week 2 – TCP & UDP Analysis Using Wireshark

## Objective
The objective of this lab was to analyze TCP and UDP traffic using Wireshark in a Kali Linux virtual machine. This lab focused on understanding how common network protocols operate at the packet level and how traffic is captured and filtered.

## Tools Used
- Kali Linux (Oracle VirtualBox)
- Wireshark
- curl
- dig

## Lab Overview
In this lab, TCP traffic was generated using HTTP requests, and UDP traffic was generated using DNS queries. Wireshark was used to capture, filter, and analyze the packets to observe protocol behavior, headers, and communication patterns.

## Key Tasks Completed
- Verified network configuration using `ip a`
- Captured TCP traffic using `curl http://example.com`
- Captured UDP traffic using `dig google.com`
- Applied TCP and UDP filters in Wireshark
- Analyzed packet details including source/destination IPs and ports
- Saved packet captures and screenshots
- Transferred files using a shared folder

## Files Included
- `Week2_TCP_UDP_Wireshark.pcapng` – Packet capture file
- `Week2_TCP_UDP_Wireshark_Lab_Report.txt` – Lab write-up
- TCP overview and packet detail screenshots
- UDP packet detail screenshots
- IP configuration verification screenshot
- Shared folder verification screenshots

## Key Takeaways
This lab strengthened my understanding of how TCP and UDP differ in behavior and reliability, how DNS uses UDP, and how Wireshark can be used to inspect real network traffic at a low level.
