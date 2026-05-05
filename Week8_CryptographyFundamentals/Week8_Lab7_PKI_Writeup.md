Week 8 Lab #7 (Write-Up)

Week 8 Cryptography Lab — Public Key Infrastructure (PKI)

This lab focused on understanding how Public Key Infrastructure (PKI) works by creating a full certificate chain. The main goal was to generate a Root Certificate Authority (CA), an Intermediate CA, and a Personal Certificate using OpenSSL, and then verify that they all work together correctly.

I started by setting up my working environment in the UMBC Virtual Desktop and creating a dedicated folder in Google Drive to store all files. From there, I generated a private key for the Root CA and used it to create a self-signed root certificate. This root certificate acts as the main trust anchor in the chain.

Next, I created an Intermediate Certificate Authority. This involved generating another private key and a certificate signing request (CSR). I then used the Root CA to sign the Intermediate CA certificate. This step showed how trust is passed from the root down to other certificates instead of directly trusting everything from one source.

After that, I created a Personal Certificate. I generated a private key and CSR for it, then signed it using the Intermediate CA. This completed the full trust chain: Root CA → Intermediate CA → Personal Certificate.

To make sure everything worked correctly, I verified the certificates using OpenSSL commands. I also inspected each certificate to confirm details like the issuer, subject, and key usage. This helped me understand how certificates are structured and how systems determine whether something is trusted.

One challenge I faced was making sure all file paths and configuration settings were correct. Even small mistakes could cause commands to fail or break the trust chain. This forced me to slow down and carefully check each step instead of just running commands quickly.

This lab helped me understand that certificates are not just files — they are part of a system that builds trust between machines. If the chain is broken or configured incorrectly, secure communication cannot be established.

Overall, this lab gave me a better understanding of how encryption, identity, and trust work together in real systems, especially in things like HTTPS and secure communications.