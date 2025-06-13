# 🛠️ Prerequisites

**Page Objective:** This page lists all the necessary software, accounts, and knowledge you should have before starting this guide to ensure a smooth setup process.

## General Knowledge
*   Basic familiarity with the Linux command line interface (CLI).
*   Understanding of file paths and directory structures.
*   Ability to edit text files using a CLI editor (e.g., `nano`, `vim`).
*   Basic understanding of SSH (Secure Shell) for remote server access.

## On Your Server
*   A Linux server (VPS, dedicated server, Raspberry Pi, home server, etc.) with root or `sudo` access.
*   SSH access to the server.
*   `git` installed on the server (covered in [Section 4.1](./04_Server_Git_Setup/4.1_Installing_Git.md)).

## On Your Arch Linux Desktop Client (KDE/Wayland)
*   A working Arch Linux installation with KDE Plasma and Wayland.
*   Internet access.
*   `sudo` privileges.
*   Software to be installed (covered in the guide):
    *   `gnupg` (usually pre-installed)
    *   `pass`
    *   `git`
    *   `qtpass` (GUI for KDE)
    *   `wl-clipboard` (for Wayland clipboard access)
    *   `pass-otp` and `oathtool` (for 2FA/OTP support)
    *   Browser integration tools (optional, e.g., `browserpass`)

## On Your Android Device
*   Ability to install apps from F-Droid, Google Play Store or download from [GitHub](https://github.com/android-password-store/Android-Password-Store).
*   Apps to be installed (covered in the guide):
    *   `Android Password Store (APS)`
    *   `OpenKeychain: Easy PGP`

## GPG Key
*   You will generate a GPG keypair as part of this guide ([Section 4](./04_Server_Git_Setup/README.md)). If you already have one you wish to use, ensure you have access to its secret key and understand its structure (master vs. subkeys).

## Important Considerations
*   **Backups:** While Git provides versioning, think about how you will back up your GPG keys (critical!) and potentially the Git repository on your server.
*   **Security:** You are responsible for the security of your server and your GPG keys. This guide provides setup instructions, but ongoing security maintenance is up to you.

---
⬅️ **Previous: [Introduction and Why](./01_Introduction_and_Why.md)**

🏠 **Main Index: [Ultimate Pass Guide](./README.md)**

➡️ **Next: [Advanced GPG Key Configuration](./03_GPG_Setup/README.md)**
