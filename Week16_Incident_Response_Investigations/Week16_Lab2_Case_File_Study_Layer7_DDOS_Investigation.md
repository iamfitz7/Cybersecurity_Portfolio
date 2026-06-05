Layer 7 HTTP DDOS Investigation Case File:

Case Summary

A potential Layer 7 DDoS attack was identified after abnormal web traffic activity was observed within Nginx web server logs. The objective of the investigation was to determine whether the traffic represented normal user behavior or a deliberate denial-of-service attempt targeting application resources.

Initial Findings

A review of requests-per-minute data identified a significant traffic spike that greatly exceeded the established baseline. The increase warranted further investigation to determine the source and nature of the activity.

Source Analysis

Traffic analysis revealed multiple external source IP addresses generating elevated request volumes. No single host was responsible for the majority of traffic. The distributed nature of the requests increased confidence that the activity was consistent with a DDoS-style event rather than a traditional single-source DoS attack.

Target Analysis

The majority of requests were directed toward:

/
/login
/api/v1/data

These endpoints require application processing and are commonly targeted during Layer 7 HTTP flood attacks.

Impact Assessment

Review of HTTP status codes identified elevated server-side errors including 5xx responses. Increased error rates suggested the application experienced operational stress while processing the traffic.

Automation Analysis

User-Agent analysis identified indicators associated with automated tools, including:

curl
python-requests

These indicators increased confidence that portions of the traffic were generated through automation rather than legitimate user browsing activity.

Investigation Conclusion

Evidence supported a Layer 7 HTTP flood attack based on:

Significant traffic anomaly
Multiple source IP addresses
Common targeted endpoints
Increased server error activity
Automated User-Agent indicators
Recommended Actions
Continue monitoring affected web resources
Block confirmed malicious IP addresses where appropriate
Implement rate limiting controls
Review WAF protections
Monitor API endpoint utilization
Improve anomaly detection thresholds
Document findings for future investigations
Final Disposition
Confirmed Layer 7 HTTP Flood Activity
Severity: Medium
Status: Closed – Investigation Complete