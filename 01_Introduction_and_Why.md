# 🤔 Introduction: What is `pass` and Why This Setup?

**Page Objective:** This page introduces `pass` (The Standard Unix Password Manager), GPG, Git, and explains the benefits of the self-hosted, GPG-encrypted setup described in this guide.

## What is `pass`?
`pass` is a password manager that fully adheres to the Unix philosophy: "do one thing and do it well." Instead of using a complex database format or a heavy graphical interface, `pass` organizes passwords in a simple hierarchy of folders and files, just like the files on your computer. Each password is a single text file, individually encrypted with GPG. This minimalist approach makes it incredibly robust, transparent, and easy to script and integrate with other command-line tools like Git.

## What is GPG (GNU Privacy Guard)?
GPG (GNU Privacy Guard) is the open-source implementation of the OpenPGP encryption standard. It is based on public-key cryptography, a system that uses a key pair: a **public key** (which you can distribute freely, like an open padlock) and a **private key** (which you must keep absolutely secret, as it is your personal key). Data encrypted with your public key can *only* be decrypted with your private key. In our setup, `pass` uses GPG to "lock" each password file with your public key's padlock, ensuring that only you, the holder of the private key, can open it.

## What is Git?
Git is a distributed version control system, the de facto standard for software development. Think of Git as a time machine for your files: it tracks every single change, allows you to see who changed what and when, and lets you revert to a previous version if needed. For `pass`, Git serves two fundamental roles:
1.  **Synchronization:** It allows you to "push" and "pull" changes between your various devices (desktop, smartphone) and the central server, keeping everything aligned.
2.  **History & Auditing:** It provides a complete and verifiable history of every password that is added, modified, or removed.

## How the Pieces Fit Together
This diagram shows how the main components interact. Your devices never directly expose unencrypted passwords to the server; they only synchronize the encrypted files using Git over a secure SSH connection.

```text
  +------------------+               +--------------------------+             +---------------------+
  |   ARCH LINUX     |               |    SELF-HOSTED SERVER    |             |       ANDROID       |
  | (pass, qtpass)   |               |      (Linux + Git)       |             | (APS, OpenKeychain) |
  +--------+---------+               +------------+-------------+             +----------+----------+
           |                                      |                                      |
           |                                      |                                      |
           |            [GPG Encryption/Decryption Happens Locally on Device]            |
           |                                      |                                      |
           +<=== [SSH Connection] ===>+  Bare Git Repository  +<=== [SSH Connection] ===>+
                                       (Encrypted .gpg files)
```

## Why This Specific Configuration?
*   **Full Control & Ownership:** Your passwords on your server.
*   **Security:** Strong GPG encryption.
*   **Open Source & Standards-Based:** Transparent, auditable, no vendor lock-in.
*   **Flexibility & Extensibility:** CLI power, GUI options, browser integration, OTPs.
*   **Offline Access & Synchronization:** Passwords available locally, synced when online.
*   **Version History:** Git tracks every change.

## Who is This Guide For?
This guide is for users who aren't afraid to get their hands dirty and who prioritize control and security over the packaged convenience of commercial solutions. It's ideal for you if you are:
*   Familiar with the Linux command line.
*   Comfortable managing your own server (even a small Raspberry Pi at home is perfect).
*   A proponent of open-source, transparent, and standards-based tools.
*   Someone who wants absolute certainty that their password data is never stored on a third-party server.

---
⬅️ **Previous: [In a Nutshell / Quick Start](./00_In-A-Nutshell.md)**

🏠 **Main Index: [Ultimate Pass Guide](./README.md)**

➡️ **Next: [Prerequisites](./02_Prerequisites.md)**
