Week 8 Lab #6 File Encryption Validation Writeup:

1. Context: What Area This Touches & Why It Matters

Cryptography is used to protect data when it is stored or shared. In real environments, this connects to confidentiality, data integrity, and identity verification. If encryption is not handled properly, sensitive data can be exposed even if everything else in the system is working correctly.

2. Realistic Scenario

A file containing user information needs to be shared with another team. The file transfers successfully, but there is uncertainty about whether the data is protected during the transfer or if it could be read by someone else in transit.

3. Thinking Process

At first, I expected that encrypting a file would be a simple one-step process. But as I worked through it, I realized that there are multiple decisions involved, like choosing the right key and understanding who can decrypt the file.

I checked whether the plaintext file was readable first, then compared it to the encrypted version to confirm that the content was no longer visible. I also had to think about whether the encryption was being done for myself or another user.

One point of confusion was making sure the correct keys were selected. I had to slow down and confirm which key was mine and which belonged to the instructor.

4. Signals That Actually Mattered

The most important signal was the difference between the plaintext file and the .gpg file. The plaintext file was readable, while the encrypted file was not.

Another important signal was the successful SHA256 checksum match. This confirmed that the downloaded software was not modified or corrupted.

5. Decision

Based on the results, the correct action was to proceed with encryption using the appropriate keys and verify that the output files were properly generated.

If this were a real environment, I would also document which keys were used and confirm that only intended recipients could decrypt the file.

6. Risks, Trade-Offs, and Limitations

If encryption is done incorrectly, files may either remain readable or become unrecoverable. There is also a trade-off between usability and security, since encryption adds extra steps to normal workflows.

This lab does not fully cover key management at scale, which becomes much more complex in real systems.

7. Common Beginner Mistake

A common mistake is thinking encryption is automatic once a tool is installed. In reality, selecting the wrong key or configuration can make encryption useless or insecure.

8. One Practical Improvement

One improvement would be clearer labeling of keys and recipients to reduce confusion during encryption.

9. Summary

This work focused on understanding how encryption actually behaves in practice. Instead of just running tools, I had to verify outputs and confirm that the data was truly protected. This helped me better understand how encryption decisions impact real systems. It also showed how small mistakes in configuration can lead to security gaps.