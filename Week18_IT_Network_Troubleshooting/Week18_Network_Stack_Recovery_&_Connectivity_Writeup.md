# Windows Network Troubleshooting Lab Write-Up

## Lab Overview

In this lab, I worked through a Windows network connectivity issue inside a Windows 11 virtual machine running in Oracle VirtualBox. The virtual machine was unable to access the internet, and web pages would not load. Instead of immediately trying random fixes, I used a structured troubleshooting process to identify where the problem was occurring and verify each part of the network configuration.

The purpose of this lab was to practice troubleshooting Windows networking problems using both Command Prompt and PowerShell while learning how different networking components work together to provide connectivity.

---

## Objective

The objective of this lab was to diagnose and restore network connectivity in a Windows 11 virtual machine by verifying network settings, testing connectivity, reviewing Windows networking components, and validating that the system could successfully communicate with external resources after the repairs were completed.

---

## Lab Environment

**Operating System**

* Windows 11 Virtual Machine

**Virtualization Platform**

* Oracle VirtualBox

**Network Configuration**

* NAT Adapter
* Host-Only Adapter

**Tools Used**

* Windows Command Prompt
* Windows PowerShell
* Windows Firewall
* Oracle VirtualBox Network Settings

---

## Problem Description

When I attempted to browse the internet from the virtual machine, Google Chrome displayed a message stating that internet access was blocked. Network tests also failed, including ping requests to Google's public DNS server (8.8.8.8) and DNS lookups for google.com.

Although the network adapters appeared to be connected, the virtual machine was unable to communicate with external systems.

Because several different issues can cause this type of problem, I decided to investigate each possible area one at a time instead of assuming a single cause.

---

## Investigation Process

The first step was verifying the virtual machine's network configuration in Oracle VirtualBox. I confirmed that both the NAT adapter and the Host-Only adapter were configured correctly and that the virtual network itself was operating normally.

After confirming the VirtualBox configuration, I examined the Windows IP configuration using Command Prompt. I reviewed the assigned IPv4 addresses, subnet masks, default gateway information, DHCP configuration, and DNS server settings to verify that Windows had successfully obtained network information.

Next, I reviewed the routing table to make sure Windows had valid routes for network communication. The routing information appeared normal, suggesting that the routing table was not the primary cause of the issue.

I then tested connectivity by sending ping requests to Google's public DNS server (8.8.8.8) and to the VirtualBox Host-Only adapter. Both tests failed with "General failure" messages, confirming that the problem extended beyond normal web browsing.

To continue narrowing down the issue, I performed DNS lookups using nslookup. These requests also timed out, showing that the virtual machine was unable to communicate with its configured DNS server.

Since Windows networking relies on several background services, I checked the Network Location Awareness (NlaSvc) service. I discovered that the service had stopped running, so I started it and verified that it was now active. Although this alone did not restore connectivity, it removed one possible cause from the investigation.

I also verified that Windows detected both network adapters correctly and confirmed that important networking protocols such as IPv4, IPv6, Client for Microsoft Networks, and File and Printer Sharing were properly enabled.

The Windows Firewall configuration was reviewed next. I inspected the Domain, Private, and Public firewall profiles to verify that the firewall configuration appeared normal and was not obviously blocking all communication.

After verifying these individual components, I performed several Windows networking repair procedures. These included resetting the Windows Firewall configuration, resetting the TCP/IP stack, resetting the Winsock catalog, and rebuilding the network adapter configuration. Because these changes require a restart, I rebooted the virtual machine before performing any additional tests.

---

## Validation

After restarting the virtual machine, I repeated the same network tests performed earlier.

This time, the results were successful.

The virtual machine was able to ping Google's public DNS server without packet loss, demonstrating that internet connectivity had been restored.

I also successfully pinged google.com, confirming that DNS resolution was working correctly.

Finally, an nslookup query returned valid DNS records, verifying that the system could now communicate with its configured DNS servers.

By repeating the same tests used during the initial investigation, I was able to confirm that the networking issue had been resolved rather than assuming the repairs were successful.

---

## Challenges

One challenge during this lab was that multiple networking components appeared to be functioning correctly, making it difficult to immediately identify the source of the problem.

The virtual machine had valid IP addresses, both network adapters were detected, and the routing table appeared normal. This reinforced the importance of working through each networking layer individually instead of making assumptions based on only one observation.

Another challenge was understanding that Windows networking depends on several services and components working together. Even when one area appears correct, another component may still prevent successful communication.

---

## What I Learned

This lab helped me better understand how Windows networking is built from multiple layers that all work together.

I learned that troubleshooting network problems is more effective when following a structured process rather than trying random fixes. By validating one component at a time, it became much easier to narrow down possible causes and understand which areas were functioning correctly.

I also gained more experience using both Command Prompt and PowerShell to gather network information and verify system configuration.

Finally, this lab reinforced the importance of validating repairs after making changes. Successfully restoring connectivity is only part of the process—the final step is confirming that the original problem has actually been resolved.

---

## Skills Demonstrated

* Windows network troubleshooting
* VirtualBox network configuration
* TCP/IP troubleshooting
* DHCP validation
* DNS troubleshooting
* Route table analysis
* Windows PowerShell
* Windows Command Prompt
* Windows Firewall review
* Network adapter verification
* Windows service management
* System validation
* Structured troubleshooting
* Technical documentation

---

## Conclusion

This lab gave me practical experience troubleshooting a Windows network connectivity issue in a virtual environment. Rather than relying on trial and error, I followed a structured investigation process that included verifying VirtualBox settings, reviewing Windows network configuration, testing connectivity, checking system services, validating firewall settings, and repairing Windows networking components.

The experience strengthened my understanding of how different networking components work together and reinforced the importance of collecting evidence before making configuration changes. Developing a repeatable troubleshooting methodology like this will be valuable when diagnosing networking and security issues in future system administration and cybersecurity environments.
