# The Ultimate `pass` Guide
Welcome to the comprehensive guide for setting up and using `pass` (The Standard Unix Password Manager) securely and synchronized across multiple devices
This guide is designed for easy navigation. If you're an expert user looking for quick commands, jump straight to [**🚀 Quick Start**](./00_In-A-Nutshell.md)

## Main Table of Contents
This guide is divided into the following main sections. Each section may contain multiple pages for easier reading

0.  🚀 **[In a Nutshell / Quick Start for Experts](./00_In-A-Nutshell.md)**
1.  🤔 **[Introduction: What is `pass` and Why This Setup?](./01_Introduction_and_Why.md)**
2.  🛠️ **[Prerequisites](./02_Prerequisites.md)**
3.  🔑 **[Advanced GPG Key Configuration](./03_GPG_Setup/README.md)**
    *   [Generating GPG Keys (Master + Subkeys)](./03_GPG_Setup/3.1_Generating_Keys.md)
    *   [Secure Backup & Revocation Certificate](./03_GPG_Setup/3.2_Backup_and_Revocation.md)
    *   [Configuring `gpg-agent` on Arch Linux with KDE](./03_GPG_Setup/3.3_GPG_Agent_KDE.md)
4.  ☁️ **[Setting Up Your Self-Hosted Git Server](./04_Server_Git_Setup/README.md)**
    *   [Installing Git on the Server](./04_Server_Git_Setup/4.1_Installing_Git.md)
    *   [Creating a Dedicated User](./04_Server_Git_Setup/4.2_Dedicated_User.md)
    *   [Creating the Bare Git Repository](./04_Server_Git_Setup/4.3_Bare_Repository.md)
    *   [Configuring SSH Access](./04_Server_Git_Setup/4.4_SSH_Access.md)
5.  💻 **[Linux Client Setup (KDE)](./05_Arch_Linux_Client_Setup/README.md)**
    *   [Installing `pass` and Related Tools](./05_Arch_Linux_Client_Setup/5.1_Installing_Pass.md)
    *   [Initializing `pass` with Your GPG Key](./05_Arch_Linux_Client_Setup/5.2_Initializing_Pass_GPG.md)
    *   [Initializing the Git Repository for `pass`](./05_Arch_Linux_Client_Setup/5.3_Initializing_Git_Repo.md)
    *   [First Commit and Push](./05_Arch_Linux_Client_Setup/5.4_First_Commit_Push.md)
6.  📱 **[Android Client Setup (APS)](./06_Android_Client_Setup/README.md)**
    *   [Installing Necessary Apps](./06_Android_Client_Setup/6.1_Installing_Apps.md)
    *   [Importing Your Private GPG Key](./06_Android_Client_Setup/6.2_Importing_GPG_Key.md)
    *   [Configuring APS](./06_Android_Client_Setup/6.3_Configuring_APS.md)
7.  ⚙️ **[Daily Usage](./07_Daily_Usage/README.md)**
    *   [CLI Usage on Arch Linux](./07_Daily_Usage/7.1_CLI_Usage_Arch.md)
    *   [Using `qtpass` (KDE GUI)](./07_Daily_Usage/7.2_QtPass_KDE.md)
    *   [Using APS (Android)](./07_Daily_Usage/7.3_APS_Usage_Android.md)
    *   [Synchronization](./07_Daily_Usage/7.4_Synchronization.md)
8.  ✨ **[Advanced Features and Integrations](./08_Advanced_Features/README.md)**
    *   [Managing TOTP Codes with `pass-otp`](./08_Advanced_Features/8.1_Pass_OTP.md)
    *   [Browser Integration](./08_Advanced_Features/8.2_Browser_Integration.md)
    *   [Sharing Passwords](./08_Advanced_Features/8.3_Sharing_Passwords.md)
    *   [Scripting and Useful Aliases](./08_Advanced_Features/8.4_Scripting_Alias.md)
9. 🛡️ **[Maintenance and Security](./09_Maintenance_Security/README.md)**
    *   [Updating GPG Keys (Expiration)](./09_Maintenance_Security/9.1_GPG_Key_Updates.md)
    *   [Password Rotation](./09_Maintenance_Security/9.2_Password_Rotation.md)
    *   [Server Security](./09_Maintenance_Security/9.3_Server_Security.md)
10. 🆘 **[Common Issues & Troubleshooting](./10_Troubleshooting.md)**

---
Let's get started! Begin with [🤔 Introduction: What is `pass` and Why This Setup?](./01_Introduction_and_Why.md) or, if you're in a hurry, the [🚀 In a Nutshell](./00_In-A-Nutshell.md)
