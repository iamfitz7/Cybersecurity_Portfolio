Validating Network Intrusion Detection Visibility Using Suricata on pfSense


1. Context: What Area This Lab Focused On and Why It Matters

This lab focused on network traffic visibility and intrusion detection, specifically how an IDS (Suricata) observes, analyzes, and generates alerts based on network behavior when deployed on a firewall.

In real environments, intrusion detection systems are often assumed to be “working” simply because they are enabled. However, without validating where traffic flows and what traffic is actually inspected, teams can develop blind spots without realizing it. This lab highlighted the importance of confirming that security controls are not just configured, but actually seeing the traffic they are meant to monitor.

Understanding this matters because an IDS that does not observe relevant traffic provides a false sense of security, especially in virtualized or segmented networks.

2. Realistic Scenario: Why Someone Would Need to Investigate This

A realistic scenario for this setup would be an environment where a firewall-based IDS is deployed, but security teams notice that expected alerts are not appearing, even during known scanning or testing activity.

The system is reachable, traffic flows correctly, and services respond as expected — yet the IDS remains silent. This raises an important question:
Is the IDS misconfigured, or is the traffic simply not passing through the inspection point?

This situation does not represent an active breach, but it does represent an uncertainty that needs to be resolved before trusting alerts (or lack of alerts) in production.

3. Thinking Through the Problem (Not Just Following Steps)

At the beginning of the lab, my assumption was straightforward:
If Suricata was enabled on the LAN interface and I ran Nmap scans from a Kali VM, alerts should appear.

When no alerts showed up, my first thought was that something was misconfigured. I checked rule updates, interface status, logging settings, and verified that Suricata was running. All of those checks came back normal.

At that point, I realized the issue might not be configuration, but visibility. I started thinking about how traffic actually moves in a virtualized network. The key realization was that LAN-to-LAN traffic does not necessarily pass through the firewall routing path in this environment. That meant Suricata could be running correctly and still see nothing.

This shifted my focus from “what settings are wrong” to “what traffic is actually crossing the firewall.”

4. Signals That Actually Mattered

Several signals helped clarify what was happening:

Nmap scan results showed successful scans, confirming traffic was occurring

Suricata logs and alerts remained empty, even after multiple scan types

DHCP leases confirmed both Kali and Windows VMs were active

Firewall routing showed LAN traffic was not traversing WAN

The most important signal was not a log entry, but the absence of alerts despite known activity. That absence pointed to a traffic visibility issue rather than a broken IDS.

Once Suricata was attached to the WAN interface, and outbound scanning traffic was generated, alerts immediately appeared. This confirmed that Suricata was functioning properly once traffic passed through the correct inspection point.

5. Decision Made Based on Evidence

Based on the observed behavior, the reasonable decision was to:

Deploy Suricata on the WAN interface

Validate detection using WAN-bound reconnaissance traffic

Treat LAN-only scans as insufficient for IDS validation in this environment

This decision matched the evidence and avoided unnecessary changes to otherwise working configurations. It also aligned with how IDS systems are commonly validated in real networks.

6. Risks, Limitations, and Trade-Offs

One major risk highlighted by this lab is assuming visibility without validation. An IDS may appear healthy while silently missing traffic due to placement decisions.

There is also a trade-off between visibility and noise. Monitoring WAN traffic increases alert volume and requires tuning to avoid false positives. However, missing alerts entirely is a much greater risk than handling extra noise.

Additionally, virtualized lab environments can behave differently than physical networks, which makes understanding traffic flow even more important.

7. Common Beginner Mistake This Lab Exposed

A common beginner mistake is assuming that any network traffic automatically passes through the firewall. This lab showed that internal traffic can bypass inspection depending on topology.

It’s tempting to keep adjusting settings when alerts don’t appear, but sometimes the correct answer is to step back and question assumptions about traffic flow instead of configuration.

8. One Practical Improvement

A practical improvement would be to document IDS inspection points clearly as part of firewall deployment. This ensures future testing and alert expectations are aligned with where traffic is actually inspected.

Clear documentation helps avoid wasted time troubleshooting systems that are technically functioning as designed.

9. Final Summary

This lab reinforced the importance of validating security controls through observation, not assumption. By working through missing alerts and understanding traffic flow in a virtualized environment, I confirmed how IDS placement directly affects visibility. The experience emphasized reasoning, signal recognition, and decision-making over blindly following configuration steps. Most importantly, it demonstrated that effective security depends on understanding how systems behave in practice, not just how they are configured on paper.