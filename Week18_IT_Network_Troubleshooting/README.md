# Week 18 – Windows Network Troubleshooting

## Overview

Reliable network connectivity is one of the most important requirements for both everyday system administration and cybersecurity operations. Before analysts can investigate alerts, collect logs, perform threat hunting, or access cloud resources, they first need to confirm that the affected system can communicate correctly across the network.

This project documents my process of troubleshooting a Windows 11 virtual machine that unexpectedly lost internet connectivity. Rather than immediately applying random fixes, I worked through the problem methodically by validating network configuration, reviewing Windows networking components, examining system services, checking firewall settings, verifying adapter configuration, and testing connectivity after each change.

The goal of this project was not simply to restore internet access, but to develop a structured troubleshooting process that can be applied to future networking issues.

---

# Objectives

During this project I focused on learning how to:

- Identify common causes of Windows network connectivity problems
- Verify VirtualBox network adapter configurations
- Analyze IP configuration and routing information
- Validate DNS functionality
- Inspect Windows networking services
- Review Windows Firewall configuration
- Reset Windows networking components when appropriate
- Confirm successful network recovery using multiple validation methods

---

# Lab Environment

## Virtualization Platform

- Oracle VirtualBox

## Operating System

- Windows 11 Virtual Machine

## Network Configuration

- NAT Adapter
- Host-Only Adapter
- VirtualBox Host-Only Network

---

# Scenario

While working inside my Windows 11 virtual machine, internet connectivity unexpectedly stopped working.

Google Chrome displayed that internet access had been blocked, websites failed to load, and multiple network tests were unsuccessful. Rather than assuming the problem had a single cause, I approached the issue using a structured troubleshooting methodology to determine exactly where communication was failing.

---

# Investigation Process

## Step 1 – Verify VirtualBox Network Configuration

Before making changes inside Windows, I verified the virtual machine's networking configuration within Oracle VirtualBox.

This included checking:

- NAT Adapter configuration
- Host-Only Adapter configuration
- Cable Connected status
- Host-Only Network settings

### Evidence

- `01_VM_NAT_Adapter.png`
- `02_VM_HostOnly_Adapter.png`
- `03_HostOnly_Network_Settings.png`

---

## Step 2 – Verify Windows Network Configuration

Next, I confirmed that Windows had received valid network settings.

I reviewed:

- IPv4 address
- Subnet mask
- Default gateway
- DHCP information
- DNS server configuration

This helped determine whether Windows had successfully obtained network configuration from the configured adapters.

### Evidence

- `04_IPConfig_All_Part1.png`
- `05_IPConfig_All_Part2.png`
- `06_IPConfig_All_Part3.png`

---

## Step 3 – Examine the Routing Table

After confirming IP configuration, I reviewed the Windows routing table.

I wanted to verify that:

- A default route existed
- Connected routes appeared correctly
- Network interfaces were properly associated with their routes

The routing table appeared reasonable, suggesting the issue was likely located elsewhere.

### Evidence

- `07_Route_Print_Part1.png`
- `08_Route_Print_Part2.png`

---

## Step 4 – Test Network Connectivity

I performed several connectivity tests to determine the scope of the problem.

Tests included:

- Pinging Google's public DNS server (8.8.8.8)
- Pinging the Host-Only adapter
- Performing DNS lookups

All tests failed, confirming that the issue extended beyond normal web browsing.

### Evidence

- `09_Ping_8.8.8.8_Failed.png`
- `10_Ping_HostOnly_Failed.png`
- `11_NSLookup_Failed.png`
- `12_Chrome_No_Internet.png`

---

## Step 5 – Review Windows Network Services

Since Windows networking depends on several background services, I inspected the Network Location Awareness (NlaSvc) service.

The service was initially stopped.

I started the service and verified that it was running correctly.

Although this did not immediately restore connectivity, it eliminated one possible cause of the issue.

### Evidence

- `13_NlaSvc_Stopped.png`
- `14_NlaSvc_Running.png`

---

## Step 6 – Verify Network Adapters

I verified that Windows detected both network adapters correctly.

I confirmed:

- Adapter status
- Interface names
- Operational state

### Evidence

- `15_Get-NetAdapter.png`
- `16_Get-NetIPConfiguration.png`

---

## Step 7 – Review Network Adapter Bindings

Next, I verified the protocols enabled on each adapter.

This included:

- IPv4
- IPv6
- Client for Microsoft Networks
- File and Printer Sharing
- LLDP
- QoS Packet Scheduler

This confirmed that required networking components remained enabled.

### Evidence

- `17_Get-NetAdapterBinding.png`

---

## Step 8 – Review Windows Firewall Configuration

I inspected all Windows Firewall profiles to determine whether firewall settings might be preventing communication.

Profiles reviewed:

- Domain
- Private
- Public

The firewall configuration appeared normal and was not immediately identified as the source of the problem.

### Evidence

- `18_Get-NetFirewallProfile_Part1.png`
- `19_Get-NetFirewallProfile_Part2.png`
- `20_Get-NetFirewallProfile_Part3.png`

---

## Step 9 – Reset Windows Networking Components

After validating the existing configuration, I began repairing Windows networking.

Repairs included:

- Resetting Windows Firewall
- Resetting the TCP/IP stack
- Resetting the Winsock catalog
- Rebuilding network adapter configuration

Several of these repairs required a system restart before taking effect.

### Evidence

- `21_Firewall_Reset.png`
- `22_TCPIP_Reset.png`
- `23_Winsock_Reset.png`
- `24_Network_Adapter_Reset.png`

---

## Step 10 – Validate Connectivity

After restarting the virtual machine, I repeated the same tests performed earlier.

Successful validation included:

- Successful ping to 8.8.8.8
- Successful DNS resolution
- Successful ping to google.com
- Restored internet connectivity

Rather than assuming the repair worked, I confirmed recovery using multiple independent tests.

### Evidence

- `25_IPConfig_After_Reset.png`
- `26_Ping_8.8.8.8_Success.png`
- `27_Ping_Google_Success.png`
- `28_NSLookup_Success.png`

---

# Key Findings

Several important observations stood out during this troubleshooting process.

- A valid IP address alone does not guarantee network connectivity.
- Multiple Windows networking components must work together for successful communication.
- System services, adapter bindings, firewall policies, routing information, and the TCP/IP stack all play important roles.
- Restarting the system after repairing networking components is often required before changes fully take effect.

---

# Skills Demonstrated

- Windows Network Troubleshooting
- Oracle VirtualBox
- NAT Networking
- Host-Only Networking
- TCP/IP Troubleshooting
- DHCP Validation
- DNS Troubleshooting
- Route Analysis
- Windows PowerShell
- Windows Command Prompt
- Windows Firewall
- Winsock Reset
- TCP/IP Stack Repair
- Windows Services
- Network Adapter Configuration
- Technical Documentation

---

# Technologies Used

- Windows 11
- Oracle VirtualBox
- Windows PowerShell
- Command Prompt
- TCP/IP
- DHCP
- DNS
- NAT
- Host-Only Networking

---

# Conclusion

This project strengthened my understanding of Windows network troubleshooting by reinforcing the importance of a structured, evidence-based approach. Rather than relying on trial and error, I worked through the problem by validating configuration, testing connectivity, reviewing Windows services, examining firewall settings, and repairing networking components only after earlier checks had been completed.

Although restoring internet access was the immediate objective, the more valuable outcome was developing a repeatable troubleshooting methodology that can be applied to future networking, system administration, and cybersecurity investigations.
