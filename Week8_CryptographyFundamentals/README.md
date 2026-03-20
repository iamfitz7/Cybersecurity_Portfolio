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
