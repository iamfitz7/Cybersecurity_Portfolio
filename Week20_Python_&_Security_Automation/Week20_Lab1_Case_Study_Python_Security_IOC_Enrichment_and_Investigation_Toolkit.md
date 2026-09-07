# Week 20 Lab 1: Python Security IOC Enrichment and Investigation Toolkit

## Incident Case Study: High-Priority MD5 Threat Intelligence Finding

---

## Case Information

**Case Type:** Threat Intelligence Enrichment and IOC Investigation  
**Environment:** Kali Linux  
**Tool:** Custom Python Security IOC Enrichment and Investigation Toolkit  
**Threat Intelligence Provider:** VirusTotal  
**Primary IOC Type:** MD5 Hash  
**Classification:** High-Priority Negative Reputation  
**Investigation Priority:** Priority 1  
**Confirmed Endpoint Compromise:** No

---

# 1. Executive Summary

During testing of my Python Security IOC Enrichment and Investigation Toolkit, I performed a bulk investigation using a mixed collection of Indicators of Compromise.

The test data contained IPv4 addresses, domains, a URL, an MD5 hash, duplicate values, malformed data, and unsupported text.

The purpose was to determine whether the application could correctly process realistic mixed IOC data through the following workflow:

```text
IOC Input
    ↓
Validation
    ↓
Type Detection
    ↓
Normalization
    ↓
Deduplication
    ↓
Threat Intelligence Enrichment
    ↓
JSON Processing
    ↓
Evidence Assessment
    ↓
Priority Assignment
    ↓
Investigation Reporting
```

The most important finding was the MD5 hash:

```text
44d88612fea8a8f36de82e1278abb02f
```

The application successfully identified the indicator as a valid MD5 hash and submitted it to VirusTotal.

VirusTotal returned:

```text
63 malicious detections
```

The application's assessment logic converted this evidence into:

```text
Assessment: High-priority negative reputation
Priority: 1
```

This was the highest-priority finding in the bulk investigation.

The result showed that the toolkit could take external threat intelligence evidence and turn it into an organized investigation priority.

However, the result was not treated as proof that a specific endpoint was compromised.

The correct conclusion was that the hash had strong negative external reputation and would require immediate investigation if discovered inside a real environment.

---

# 2. Investigation Background

The case was created while testing a Python security automation application designed to process Indicators of Compromise.

The application supports:

- IPv4 addresses
- Domains
- URLs
- MD5 hashes
- SHA1 hashes
- SHA256 hashes

It also supports:

- Single IOC input
- TXT bulk input
- CSV bulk input
- IOC validation
- IOC normalization
- Deduplication
- Occurrence counting
- VirusTotal enrichment
- AbuseIPDB enrichment for IPv4
- JSON parsing
- Evidence-based assessment
- CSV export
- JSON export
- Analyst reporting
- Operational logging

The investigation was performed inside a Kali Linux virtual machine.

---

# 3. Investigation Objective

The objective of the case was to determine whether the application could correctly:

1. Accept mixed IOC input.

2. Validate each indicator.

3. Identify the IOC type.

4. Reject malformed input.

5. Normalize valid indicators.

6. Remove duplicate investigation targets.

7. Preserve occurrence counts.

8. Route each IOC to the appropriate threat intelligence provider.

9. Process the provider's JSON response.

10. Extract meaningful security evidence.

11. Assign an investigation priority.

12. Generate structured output.

13. Generate a readable analyst report.

14. Avoid making unsupported claims about compromise.

---

# 4. Initial IOC Data

The bulk input contained:

```text
8.8.8.8
EXAMPLE.COM
example.com
https://example.com/
44d88612fea8a8f36de82e1278abb02f
999.999.999.999
not-an-ioc
8.8.8.8
```

This created eight raw input rows.

The data intentionally contained different conditions.

### Valid IPv4

```text
8.8.8.8
```

### Duplicate IPv4

```text
8.8.8.8
```

appeared twice.

### Domain

```text
example.com
```

### Duplicate Domain With Different Capitalization

```text
EXAMPLE.COM
example.com
```

### URL

```text
https://example.com/
```

### MD5 Hash

```text
44d88612fea8a8f36de82e1278abb02f
```

### Malformed IPv4-Like Value

```text
999.999.999.999
```

### Unsupported Input

```text
not-an-ioc
```

The input was intentionally messy to test how the application handled different types of security data during one investigation.

---

# 5. Validation Phase

The first step was validation.

The application checked each input value before performing external threat intelligence enrichment.

The MD5 indicator:

```text
44d88612fea8a8f36de82e1278abb02f
```

matched the expected 32-character hexadecimal format for an MD5 hash.

The application identified:

```text
Valid: True
Indicator Type: md5
```

This allowed the IOC to continue to the enrichment phase.

Validation is important because external threat intelligence services should not receive malformed or unsupported data when the problem can be detected locally first.

---

# 6. Invalid IOC Detection

The same validation process identified two invalid values.

The first was:

```text
999.999.999.999
```

Although it resembles an IPv4 address, it is not a valid IPv4 address.

The second was:

```text
not-an-ioc
```

The application rejected both values.

Their final assessment was:

```text
Assessment: Invalid IOC
Priority: 5
```

The application did not send these values blindly to external threat intelligence services.

This demonstrated that validation was being used as an actual control in the investigation workflow.

---

# 7. Normalization Phase

Valid indicators were normalized before deduplication.

For example:

```text
EXAMPLE.COM
```

was converted to:

```text
example.com
```

The second copy was already:

```text
example.com
```

After normalization, both values represented the same investigation target.

This prevented different capitalization from creating unnecessary duplicate enrichment requests.

---

# 8. Deduplication Phase

After normalization, duplicate indicators were consolidated.

The original input contained:

```text
8.8.8.8
```

twice.

The application processed it as one unique enrichment target while preserving:

```text
occurrence_count = 2
```

The original input also contained:

```text
EXAMPLE.COM
example.com
```

After normalization, both represented:

```text
example.com
```

The eight raw input rows were therefore reduced to:

```text
6 unique investigation targets
```

This was important because external threat intelligence APIs can have request limits.

Deduplication reduces unnecessary API requests while still preserving useful context.

---

# 9. Provider Routing

After validation, normalization, and deduplication, the application determined which threat intelligence provider should receive each IOC.

The provider routing logic was:

```text
IPv4
├── VirusTotal
└── AbuseIPDB

Domain
└── VirusTotal

URL
└── VirusTotal

MD5
└── VirusTotal

SHA1
└── VirusTotal

SHA256
└── VirusTotal
```

Because the main IOC in this case was an MD5 hash, it was routed to:

```text
VirusTotal
```

AbuseIPDB was not used for the hash because the project's AbuseIPDB integration was designed specifically for IPv4 reputation.

---

# 10. Threat Intelligence Enrichment

The application sent the MD5 hash to VirusTotal.

The indicator was:

```text
44d88612fea8a8f36de82e1278abb02f
```

VirusTotal returned a successful response.

The most important evidence extracted from the response was:

```text
Malicious detections: 63
```

The application also processed the provider status and other available analysis information.

Instead of forcing an analyst to review the entire raw JSON response, the application extracted the fields needed for prioritization.

---

# 11. Primary Evidence

The main evidence supporting escalation was:

```text
VirusTotal reported 63 malicious detections
```

This was significantly stronger negative reputation evidence than the other indicators in the test data.

The application therefore needed to distinguish this hash from lower-priority results.

---

# 12. Automated Assessment

The application contained an evidence-based assessment module.

The scoring logic used VirusTotal and AbuseIPDB information to organize findings into priorities.

The priority structure included:

```text
Priority 1
High-priority negative reputation

Priority 2
Suspicious - investigation recommended

Priority 3
No significant negative intelligence observed

Priority 4
Insufficient intelligence

Priority 5
Invalid IOC
```

The application's VirusTotal threshold for Priority 1 included:

```text
5 or more malicious detections
```

The MD5 hash returned:

```text
63 malicious detections
```

This was well above the Priority 1 threshold.

The application therefore returned:

```text
Assessment: High-priority negative reputation
Priority: 1
```

Supporting evidence:

```text
VirusTotal reported 63 malicious detections
```

---

# 13. Comparison With Other Investigation Results

The MD5 finding became more meaningful when compared with the other test indicators.

## MD5 Hash

```text
44d88612fea8a8f36de82e1278abb02f
```

Result:

```text
Valid: True
Type: md5
VirusTotal Status: Success
VirusTotal Malicious Detections: 63
Assessment: High-priority negative reputation
Priority: 1
```

---

## IPv4 Address

```text
8.8.8.8
```

The indicator was valid.

It appeared twice in the original input, so the application preserved:

```text
occurrence_count = 2
```

The IOC successfully received threat intelligence enrichment through both:

```text
VirusTotal
```

and:

```text
AbuseIPDB
```

The available reputation information did not contain strong enough negative evidence to justify the same Priority 1 classification as the MD5 hash.

The application therefore did not falsely label the IP as malicious simply because threat intelligence information existed for it.

---

## Domain

The input contained:

```text
EXAMPLE.COM
example.com
```

Both values normalized to:

```text
example.com
```

The application therefore treated them as one investigation target while preserving the occurrence count.

This showed that the application could recognize the same logical IOC even when the original formatting was different.

---

## URL

The URL:

```text
https://example.com/
```

was successfully recognized as a URL and routed to VirusTotal.

It did not receive the same high-priority negative reputation assessment as the MD5 finding.

---

## Invalid IPv4-Like Input

```text
999.999.999.999
```

Result:

```text
Valid: False
Assessment: Invalid IOC
Priority: 5
```

---

## Unsupported Text

```text
not-an-ioc
```

Result:

```text
Valid: False
Assessment: Invalid IOC
Priority: 5
```

The application therefore separated:

```text
High-Priority Reputation Evidence
        |
        ├── MD5 → Priority 1
        |
Lower Negative Reputation
        |
        ├── Other valid indicators
        |
Invalid Data
        |
        ├── 999.999.999.999
        └── not-an-ioc
```

---

# 14. Analyst Interpretation

The VirusTotal result provided strong evidence that the MD5 hash had a negative external reputation.

A result of:

```text
63 malicious detections
```

would justify immediate attention if the hash were found inside a real environment.

However, the correct interpretation of the evidence is important.

The threat intelligence result shows:

```text
The hash has strong negative reputation.
```

It does not automatically show:

```text
A specific endpoint is compromised.
```

The lab did not contain internal endpoint telemetry proving that this hash existed or executed on a specific machine.

The IOC was provided to the application as investigation input.

Therefore, the correct conclusion was:

```text
The MD5 hash has strong negative threat intelligence and should receive high investigation priority.
```

The evidence was not enough to conclude:

```text
Confirmed endpoint compromise.
```

---

# 15. Why This Distinction Matters

Threat intelligence answers questions about an indicator's reputation.

It does not automatically answer what happened inside an organization.

For example, VirusTotal may tell an analyst that a hash has strong malicious reputation.

It does not automatically tell the analyst:

- Whether the file exists on an internal endpoint.
- Whether the file executed.
- Which user interacted with the file.
- Where the file came from.
- Which process created the file.
- Whether the file created child processes.
- Whether the file contacted external infrastructure.
- Whether persistence was established.
- Whether credentials were accessed.
- Whether lateral movement occurred.
- Whether data was exfiltrated.

Those questions require internal security telemetry.

---

# 16. Recommended Investigation Actions

If this hash appeared during a real security investigation, the next step would be to search internal security systems for the IOC.

Recommended investigation sources would include:

- EDR
- Antivirus telemetry
- SIEM
- File creation events
- Process execution records
- Email security logs
- Browser download history
- PowerShell logs
- DNS logs
- Proxy logs
- Firewall logs
- Network telemetry
- Identity logs
- Cloud security telemetry

The investigation should determine whether the file was actually present or executed.

---

# 17. Endpoint Investigation Questions

If the hash were discovered on an endpoint, I would want to answer questions such as:

```text
Does the file exist on any endpoint?

What is the file path?

When was the file created?

Which user owned or interacted with the file?

What process created the file?

Was the file downloaded?

Did the file execute?

What parent process launched it?

Did it create child processes?

Did it make network connections?

Did it modify the registry?

Did it create scheduled tasks or services?

Did it establish persistence?

Did it access credentials?

Did it communicate with suspicious infrastructure?

Did similar activity occur on other endpoints?

Was data accessed or transferred?
```

These questions would help determine whether the reputation finding was connected to a real security incident.

---

# 18. Possible Investigation Workflow

A real investigation could continue as:

```text
Priority 1 Hash Reputation
            ↓
Search EDR / SIEM
            ↓
Is Hash Present Internally?
        /           \
      No             Yes
      |               |
Document Context   Identify Host
                      ↓
                 Identify User
                      ↓
                 Find File Path
                      ↓
                Review Execution
                      ↓
                Build Process Tree
                      ↓
              Review Network Activity
                      ↓
               Review Persistence
                      ↓
               Review Identity Activity
                      ↓
                Search Other Hosts
                      ↓
              Determine Full Scope
                      ↓
             Final Incident Decision
```

Threat intelligence therefore acts as an investigation starting point.

---

# 19. Bulk Investigation Results

The original input contained:

```text
8 rows
```

Normalization and deduplication reduced this to:

```text
6 unique indicators
```

The final investigation contained:

- Valid indicators
- Invalid indicators
- Duplicate indicators
- Multiple IOC types
- Live external threat intelligence
- A Priority 1 reputation finding

The MD5 hash was the strongest negative finding in the dataset.

---

# 20. Structured Evidence

The application stored the investigation results in JSON.

The structured result included information such as:

```text
indicator
indicator_type
valid
validation_reason
occurrence_count
timestamp
virustotal
abuseipdb
assessment
priority
reasons
```

This allowed the provider evidence and the application's assessment to remain connected.

The structured format could also be processed by another application in the future.

---

# 21. CSV Evidence

The application also created CSV output.

Important fields included:

```text
indicator
indicator_type
valid
occurrence_count
vt_status
vt_malicious
vt_suspicious
abuse_status
abuse_confidence
abuse_reports
country
assessment
priority
timestamp
```

This created a simple analyst-friendly format that could be opened in a spreadsheet and filtered or sorted.

---

# 22. Analyst Report

The application automatically created a Markdown investigation report.

The report contained:

```text
IOC Threat Intelligence Investigation Report

Investigation Summary

Prioritized Findings

Supporting Evidence

Recommended Next Step

Investigation Limitation
```

The Priority 1 MD5 finding included information such as:

```text
Indicator:
44d88612fea8a8f36de82e1278abb02f

Type:
md5

Valid:
True

Assessment:
High-priority negative reputation

Priority:
1

VirusTotal Status:
success

VirusTotal Malicious Detections:
63

Supporting Evidence:
VirusTotal reported 63 malicious detections
```

This converted the threat intelligence response into a form that an analyst could quickly review.

---

# 23. Operational Logging

The application also created an operational log.

The log recorded events such as:

```text
Started IOC analysis

Processing IOC

Validated IOC

Invalid IOC detected

IOC analysis completed successfully
```

The bulk investigation therefore had both:

```text
Operational Evidence
        ↓
logs/ioc_tool.log
```

and:

```text
Investigation Evidence
        ↓
CSV + JSON + Markdown Report
```

The log described application activity.

The report described investigation findings.

---

# 24. Error Handling During the Investigation

The application was designed so that external provider problems would not automatically stop the entire investigation.

Possible provider states included:

```text
success
unavailable
authentication_error
not_found
rate_limited
provider_error
timeout
network_error
invalid_json
```

This was important because security automation depends on services that may sometimes be unavailable or limited.

A provider failure should be recorded as evidence about the state of the lookup.

It should not automatically destroy the rest of the investigation workflow.

---

# 25. API Rate Limits

The project also had to account for API request limits.

This made normalization and deduplication more important.

Without deduplication:

```text
8.8.8.8
8.8.8.8
```

could create two unnecessary requests.

With deduplication:

```text
8.8.8.8
→ one enrichment request
→ occurrence_count = 2
```

The same principle applied to:

```text
EXAMPLE.COM
example.com
```

The application normalized the values before duplicate checking.

This reduced unnecessary requests while keeping useful investigation context.

---

# 26. Credential Security

The API credentials used during the investigation were protected.

Real keys were stored in:

```text
.env
```

They were not hardcoded into the source code.

The `.env` file was excluded from Git.

A separate:

```text
.env.example
```

file documented the required environment variables without containing the real credentials.

This helped prevent accidental credential exposure.

---

# 27. Application Verification

The completed application was also tested separately from the threat intelligence results.

Python source files passed syntax compilation.

The automated unit tests returned:

```text
Ran 10 tests
OK
```

The generated JSON was also validated and returned:

```text
JSON VALID
```

These checks helped confirm that the investigation output came from a functioning application.

---

# 28. Incident Classification

## Indicator

```text
44d88612fea8a8f36de82e1278abb02f
```

## IOC Type

```text
MD5
```

## Provider

```text
VirusTotal
```

## Provider Status

```text
Success
```

## Malicious Detections

```text
63
```

## Assessment

```text
High-priority negative reputation
```

## Priority

```text
1
```

## Confirmed Internal Execution

```text
Not established
```

## Confirmed Endpoint Infection

```text
Not established
```

## Confirmed Environment Compromise

```text
No
```

## Final Case Classification

```text
High-Priority Threat Intelligence Finding Requiring Internal Correlation
```

---

# 29. Reason for Classification

The MD5 hash received strong negative external reputation evidence from VirusTotal.

The application correctly raised the IOC to Priority 1 because VirusTotal returned 63 malicious detections.

However, the lab did not include endpoint telemetry showing that the hash existed or executed on a specific internal machine.

For this reason, the evidence supported a high-priority IOC classification but did not support a confirmed endpoint compromise classification.

---

# 30. Key Investigation Findings

The main findings from this case were:

1. The MD5 hash was correctly validated.

2. The IOC was correctly identified as an MD5 hash.

3. The application correctly routed the hash to VirusTotal.

4. VirusTotal returned a successful result.

5. VirusTotal reported 63 malicious detections.

6. The application's assessment logic correctly assigned Priority 1.

7. The assessment was labeled `High-priority negative reputation`.

8. Invalid indicators were rejected before API enrichment.

9. Duplicate indicators were consolidated.

10. Occurrence counts were preserved.

11. Eight raw input rows became six unique investigation targets.

12. Live VirusTotal and AbuseIPDB enrichment worked during the broader test.

13. The application produced structured CSV and JSON evidence.

14. The application generated a readable investigation report.

15. The application generated operational logs.

16. The application did not incorrectly claim that the threat intelligence result proved endpoint compromise.

---

# 31. Lessons Learned

## Threat Intelligence Helps Prioritize

The 63 VirusTotal malicious detections gave a strong reason to investigate the MD5 hash before lower-priority indicators.

---

## Reputation Is Not Endpoint Evidence

A malicious reputation result describes what external intelligence providers know about an indicator.

It does not prove that the indicator exists inside an organization's environment.

---

## Internal Correlation Is Required

A high-priority IOC should be searched across internal security telemetry before making a final incident decision.

---

## Validate Before Enrichment

Malformed data should be identified locally before API requests are made.

---

## Normalize Before Deduplication

Indicators should be converted into consistent formats before determining whether they are duplicates.

---

## Deduplication Improves Efficiency

Duplicate API requests waste time and limited provider quota.

Occurrence counting allows duplicates to be removed without losing useful context.

---

## External APIs Must Be Treated as Unreliable Dependencies

Authentication problems, rate limits, timeouts, missing reports, network failures, and provider errors can occur.

Security automation should be designed to handle those conditions.

---

## Structured Evidence Is More Useful Than Raw Output

Raw JSON can contain useful information, but analysts need the important fields organized clearly.

The application converted provider responses into:

```text
Evidence
    ↓
Assessment
    ↓
Priority
    ↓
Analyst Report
```

---

## Automation Supports the Analyst

The application automated repetitive tasks such as:

- Validation
- Type detection
- Normalization
- Deduplication
- API enrichment
- Evidence extraction
- Priority assignment
- Exporting
- Reporting
- Logging

The final security decision still requires analyst judgment.

---

# 32. Final Assessment

The MD5 hash:

```text
44d88612fea8a8f36de82e1278abb02f
```

was the strongest finding in the investigation.

VirusTotal returned:

```text
63 malicious detections
```

The toolkit correctly converted that evidence into:

```text
High-priority negative reputation
Priority 1
```

If this IOC were discovered inside a real organization, it would justify immediate investigation across endpoint, SIEM, network, identity, email, and other available security telemetry.

However, the external reputation result alone would not be enough to declare an endpoint compromised.

The correct response would be:

```text
High-Priority IOC
        ↓
Internal IOC Search
        ↓
Endpoint / Network / Identity Correlation
        ↓
Determine Execution and Scope
        ↓
Final Incident Classification
```

---

# 33. Conclusion

This case demonstrated how Python security automation can turn raw IOC data into structured investigation evidence.

The application accepted a mixed IOC dataset, validated the indicators, normalized values, removed duplicates, preserved occurrence counts, performed live threat intelligence enrichment, processed JSON responses, prioritized findings, and generated analyst-ready output.

The strongest result was the MD5 hash that received 63 VirusTotal malicious detections and was correctly assigned Priority 1.

The case also demonstrated an important security investigation principle.

Threat intelligence should help answer:

```text
What should I investigate first?
```

It should not automatically answer:

```text
Was this environment compromised?
```

That second question requires internal evidence.

Overall, this case showed how automation can reduce repetitive investigation work while still keeping evidence interpretation and final incident decisions with the analyst.