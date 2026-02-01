Week 9: Lab #2 - Splunk Index & Log Source Discovery

1. Context: What Area This Touches & Why It Matters

Security investigations depend on knowing where data actually lives.
In SIEM environments, logs are spread across multiple indexes, each representing different systems, vendors, or telemetry types. If an analyst does not understand index structure, they risk searching the wrong place, missing evidence, or drawing incorrect conclusions.

This work focuses on log source discovery and validation, which is a foundational skill for effective SOC investigations.

2. Realistic Scenario: When This Knowledge Is Actually Used

An alert fires related to suspicious process execution, but it’s unclear which log source produced the data. The analyst needs to quickly determine:

Which index contains endpoint telemetry

Whether the data comes from an EDR platform, Windows logs, or another source

Whether the index is reliable for follow-up investigation

This situation doesn’t start with an incident—it starts with uncertainty about where to look.

3. Thinking Process: How I Approached the Problem

Before running advanced searches, I wanted to understand the structure of the environment. I started by identifying all available indexes and their event volume to avoid guessing.

Once I confirmed the cs-soc index existed and contained data, I explored a small sample of events instead of immediately filtering. This helped me understand how the logs were formatted, what fields were consistently present, and whether the data was usable for investigations.

As I expanded events, I shifted focus from raw text to parsed fields. Seeing fields like host, CommandLine, and file paths confirmed that this index contained endpoint-level telemetry, not just summary logs.

4. Signals That Actually Mattered (Evidence Over Noise)

Two signals stood out:

Consistent parsed fields such as host, CommandLine, and source

Command-line values showing executable paths, downloads, and network activity

These signals mattered because they showed the data was:

Structured

Searchable

Directly useful for process execution and malware investigations

I ignored less relevant metadata and focused on fields that would matter during triage.

5. Decision: What Action Made Sense Based on the Evidence

Based on the structure and content of the logs, the appropriate action was to:

Treat cs-soc as a primary investigation index for endpoint activity

Use it confidently for follow-on searches involving process execution, suspicious binaries, and command-line analysis

If this were production, I would document this index as a trusted data source for endpoint-related alerts.

6. Risks, Trade-Offs, and Limitations

This lab focuses on discovery, not detection accuracy. While the data is useful, it does not validate whether alerts are high fidelity or noisy. Additionally, CSV-based ingestion depends heavily on correct parsing—misconfigured field extractions could reduce reliability.

Understanding these limits prevents overconfidence.

7. Common Beginner Mistake (Subtle but Powerful)

A common beginner mistake is searching across index=* for every investigation.
This creates noise, wastes time, and can hide important signals.

By validating indexes first, searches become faster, more accurate, and easier to explain.

8. One Practical Improvement

A realistic improvement would be to document index ownership and purpose internally (for example: “cs-soc = endpoint telemetry”).
This would reduce ramp-up time for new analysts and improve consistency across investigations.

9. Summary

In this lab, I performed structured SIEM index discovery to identify which data sources were available and usable for investigations. By validating event volume, inspecting parsed fields, and confirming log context, I established which indexes could reliably support SOC analysis. This approach avoids guesswork, reduces noise, and ensures investigations start from the correct data source. The process reinforced the importance of understanding log context before writing detections or responding to alerts.