Week 8 Lab #2: PKI Basics — Self-Signed Certificate & Key Pair Validation (homelab.local)

1. Context: What Area This Touches & Why It Matters

Public Key Infrastructure (PKI) is the foundation behind secure communication on networks. It’s how systems prove identity and establish trust using certificates and cryptographic keys. This matters in real environments because services rely on certificates to encrypt traffic, authenticate servers, and prevent impersonation. If certificates are misunderstood or mismanaged, secure systems can break in ways that are hard to diagnose.

2. Realistic Scenario: When This Knowledge Is Actually Used

A common situation is a service that suddenly starts throwing certificate warnings or fails TLS connections altogether. At that point, someone needs to confirm whether the certificate itself is valid, whether it matches the private key, and whether the issue is trust-related or configuration-related. This usually starts with uncertainty rather than a clear error message.

3. Thinking Process: How I Approached the Problem

I expected a self-signed certificate to behave differently from a trusted public certificate, especially around issuer trust. My first focus was on creating a clean key pair and confirming that the certificate truly represented that key. I initially assumed that generating the certificate alone was enough, but I realized that validating the relationship between the private key and certificate is just as important. Seeing how the same identity appears as both the issuer and subject helped reinforce how self-signed certificates work.

4. Signals That Actually Mattered (Evidence Over Noise)

Two signals mattered most:

The Issuer and Subject fields in the certificate both showed homelab.local, confirming it was self-signed.

The matching modulus hashes between the private key and certificate proved they belonged to the same key pair.

These signals mattered far more than the raw command output because they directly answered the trust and identity questions.

5. Decision: What Action Made Sense Based on the Evidence

Based on the evidence, the correct action was to document the certificate as valid for internal lab use but not trusted by default. In a real environment, this would be acceptable for testing or internal services, while production systems would require a trusted CA. The decision fits the evidence and avoids overcomplicating the setup.

6. Risks, Trade-Offs, and Limitations

Self-signed certificates don’t provide automatic trust, which can confuse users and applications. The trade-off is simplicity versus trust: self-signed certificates are easy to create, but they require manual trust management. This lab also doesn’t cover certificate revocation or automated renewal, which are important in larger environments.

7. Common Beginner Mistake 

A common mistake is assuming that if a certificate exists, it must be valid. Beginners often skip verifying that the certificate actually matches the private key. Proper validation avoids wasted troubleshooting time and prevents deploying broken or mismatched certificates.

8. One Practical Improvement

A realistic improvement would be documenting certificate ownership and expiration dates in a central location. This reduces confusion later and helps prevent unexpected service failures due to expired or unknown certificates.

9. Summary

This lab helped me understand how certificates and key pairs work together to establish identity and encryption. I learned how to verify a self-signed certificate, confirm key pair relationships, and recognize where trust boundaries exist. These checks are small but critical steps when troubleshooting secure communication issues. This experience reinforced that security problems often come down to validating assumptions with clear evidence.