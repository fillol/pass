# 🔑 Advanced GPG Key Configuration

**Page Objective:** This section provides an overview and navigation for setting up and managing your GPG keys, which are crucial for encrypting your passwords with `pass`.

Understanding and correctly configuring GPG is the foundation of this password management system. We will cover generating robust keys, backing them up securely, and integrating GPG with your desktop environment.

## Topics Covered:

1.  **[Generating GPG Keys (Master Key + Subkeys)](./3.1_Generating_Keys.md)**:
    Learn to create a strong GPG keypair, separating the Master Key (for certification, ideally kept offline) from Subkeys (for daily signing, encryption, and authentication). This setup enhances security.

2.  **[Secure GPG Key Backup & Revocation Certificate](./3.2_Backup_and_Revocation.md)**:
    Essential steps for securely backing up your private GPG keys and generating a revocation certificate. These are critical for disaster recovery and security in case your key is compromised.

3.  **[Configuring `gpg-agent` on Arch Linux with KDE](./3.3_GPG_Agent_KDE.md)**:
    Instructions on setting up the GPG agent (`gpg-agent`) on your Arch Linux system with KDE Plasma for convenient passphrase caching and integration with KWallet.

---
➡️ **Next: [Generating GPG Keys (Master Key + Subkeys)](./3.1_Generating_Keys.md)**

🏠 **[Back to Main Index](../README.md)**
