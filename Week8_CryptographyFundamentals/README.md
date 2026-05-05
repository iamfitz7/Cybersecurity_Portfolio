# 🔐 Cryptography Fundamentals — Encryption, Hashing & TLS

This repository documents hands-on labs focused on **basic cryptography concepts** and how they are used to protect data in real systems.

The goal of this work is to understand how data is protected at rest and in transit, how trust is established using certificates, and how encryption works together with other security controls.

---

## 🎯 Learning Goals

Through these labs, the objectives are to:

- Understand symmetric and asymmetric encryption
- Create and compare cryptographic hashes
- Learn the basics of public key infrastructure (PKI)
- Generate and use digital certificates
- Set up and verify TLS encryption
- Observe encrypted traffic during communication
- Understand how cryptography supports secure systems end to end

---

## 🔑 Symmetric & Asymmetric Encryption

These labs focus on the two main types of encryption:

- Symmetric encryption (same key for encrypt and decrypt)
- Asymmetric encryption (public and private key pairs)
- Common examples such as AES and RSA
- When each type is used in real systems

🧠 **Why this matters:**  
Encryption protects sensitive data. Understanding how different encryption types work helps explain how secure connections and authentication systems are built.

---

## 🧾 Hashing Fundamentals

These labs focus on hashing and data integrity:

- Creating hashes using algorithms such as MD5 and SHA256
- Comparing hash outputs
- Understanding why hashes cannot be reversed
- Learning common use cases such as file integrity and password storage

📸 *Artifacts added:*  
- Hash creation examples  
- Documented hash values  

🧠 **Why this matters:**  
Hashes help verify that data has not been changed. They are commonly used in security checks and investigations.

---

## 🔐 Public Key Infrastructure (PKI)

These labs introduce PKI concepts:

- Generating a self-signed certificate
- Understanding public and private key pairs
- Learning how certificates are used to establish trust

📸 *Artifacts added:*  
- Screenshot of certificate creation  

🧠 **Why this matters:**  
PKI allows systems to verify identity and encrypt communication securely, especially on the web.

---

## 🌐 TLS Setup & Secure Communication

These labs focus on securing network communication:

- Setting up TLS on a local web server
- Verifying that connections are encrypted
- Observing how certificates are used during connection setup

📸 *Artifacts added:*  
- Screenshot of secure web server  
- Evidence of encrypted connection  

🧠 **Why this matters:**  
TLS protects data in transit and prevents attackers from reading or modifying traffic.

---

## 🔍 TLS Verification & Handshake Observation

These labs validate encryption:

- Using a browser to verify TLS certificates
- Capturing encrypted traffic with Wireshark
- Observing the TLS handshake process
- Confirming that application data is encrypted

📸 *Artifacts added:*  
- Wireshark captures showing TLS traffic  

🧠 **Why this matters:**  
Security should be verified, not assumed. Packet captures provide proof that encryption is working as expected.

---

## 🧩 End-to-End Security Integration

These labs combine cryptography with other controls:

- Using encryption alongside identity management
- Securing access through encrypted tunnels
- Understanding how encryption supports authentication and access control

📸 *Artifacts added:*  
- Screenshot showing integrated secure setup  

🧠 **Why this matters:**  
Strong security comes from layering controls. Cryptography plays a key role in protecting data across systems.

---

## 🛠️ Tools & Technologies

- Cryptographic hashing tools  
- Certificate generation utilities  
- Local web server  
- Wireshark  
- Virtual machine lab environment
  
---

## 🔐 GPG Encryption & Integrity Validation (Week 8 Lab)

This lab focused on using public key cryptography to encrypt files and verify software integrity in a controlled environment.

The main goal was not just to encrypt data, but to understand how encryption, key management, and integrity validation work together in real systems.

---

### 🧪 What Was Done

- Generated a public/private key pair using GPG (Kleopatra)
- Created a plaintext file and encrypted it using my key
- Verified that encrypted output was unreadable without decryption
- Exported a public key for sharing and trust establishment
- Validated the integrity of a downloaded file using SHA256 hashing in PowerShell
- Executed a script that generated an additional encrypted output file

📸 *Artifacts added:*  
- Checksum validation screenshot  
- PowerShell command and output  
- Encrypted `.gpg` file  
- Exported public key (`.asc`)  
- Script-generated encrypted output  

---

### 🧠 Key Observations

- Encryption alone is not enough — correct key usage determines who can access the data  
- Encrypted output confirms transformation, but not necessarily correct configuration  
- Hash validation plays a critical role in ensuring software has not been altered  
- Small syntax errors (like incorrect hash algorithm formatting) can break validation steps  

---

### ⚠️ Challenges Faced

- PowerShell checksum command initially failed due to incorrect syntax (`SHA 256` vs `SHA256`)
- Multiple encrypted file outputs required careful validation to confirm which file was correct
- Understanding when encryption is properly applied vs. just successfully executed

---

### 🧠 Why This Matters

This lab reflects real-world security practices where:

- Sensitive files must be encrypted before storage or transfer  
- Public keys are shared to enable secure communication  
- Software integrity must be verified before installation  
- Misconfigurations can silently introduce risk even when systems appear to work  

Understanding these concepts helps build a foundation for secure system design and data protection.

---
# 🔐 Week 8 Lab #7: Public Key Infrastructure (PKI) — Certificate Authority Chain Creation

## 1. Context: What Area This Touches & Why It Matters

This work focuses on Public Key Infrastructure (PKI), which is responsible for establishing trust and enabling secure communication between systems. PKI is used in things like HTTPS, VPNs, and authentication systems.

If this process is misunderstood or misconfigured, systems may trust the wrong entities or fail to validate identity correctly, which can lead to serious security risks.

---

## 2. Realistic Scenario: When This Is Actually Used

A realistic situation is when a system is failing to establish a secure connection, or a certificate is being rejected unexpectedly.

At that point, the question becomes:
- Is the certificate valid?
- Is the trust chain correct?
- Is the issuing authority trusted?

This type of issue requires understanding how certificates are created and verified across a full trust chain.

---

## 3. Thinking Process: How I Approached the Problem

Going into this, I expected that creating certificates would be straightforward, but I quickly realized that small mistakes (like file paths, config issues, or naming inconsistencies) could break the entire chain.

I focused on:
- Making sure each step logically built on the previous one
- Verifying outputs instead of assuming they worked
- Watching for errors in OpenSSL commands and correcting them early

At times, things didn’t work immediately, especially with configuration files and signing steps. That forced me to slow down and actually understand what each command was doing instead of just running it.

---

## 4. Signals That Actually Mattered

Two key signals stood out:

- Successful certificate verification (`openssl verify`)
  → Confirmed that the trust chain was correctly built

- Certificate inspection output (`openssl x509 -text`)
  → Showed details like issuer, subject, and key usage, which helped confirm each certificate’s role

Instead of focusing on all output, I paid attention to whether:
- The issuer matched the expected authority
- The chain validated correctly

---

## 5. Decision: What Action Made Sense

Based on the outputs, the correct action was to:
- Confirm the trust chain is valid
- Document the results clearly
- Package the files for submission and future reference

If this were a real environment, the next step would be to deploy the certificates into a system and test real connections.

---

## 6. Risks, Trade-Offs, and Limitations

One major risk is trusting a certificate without verifying its chain. If a root CA is compromised or misused, everything signed by it becomes untrusted.

There’s also a trade-off between:
- Security (strict validation)
- Usability (ease of deployment)

This lab focuses on creation and validation, but not full enterprise-scale certificate management.

---

## 7. Common Beginner Mistake

A common mistake is thinking certificates “just work” after being created.

In reality:
- Trust depends on the full chain
- Every certificate must be validated properly
- Misconfigured files or paths can silently break trust

Understanding the chain is more important than just generating keys.

---

## 8. One Practical Improvement

A practical improvement would be adding structured logging or documentation for certificate creation steps.

In real environments, this helps:
- Track certificate usage
- Prevent misconfiguration
- Support troubleshooting when things fail

---

## 9. Summary

This work helped me understand how trust is actually built between systems using certificates. Instead of just generating keys, I focused on verifying the full chain and understanding how each part connects.

It showed me that small configuration details can have a big impact on whether systems trust each other. This is something I would approach carefully in a real environment, especially when dealing with secure communications.


## 📁 Repository Structure

```text
/
├── encryption/
│   ├── symmetric-asymmetric/
│   └── gpg-encryption/
├── hashing/
│   └── integrity-validation/
├── pki/
│   └── certificates/
├── tls/
│   ├── setup/
│   └── verification/
├── integration/
│   └── secure-access/
└── README.md
