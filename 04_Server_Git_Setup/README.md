# ☁️ Setting Up Your Self-Hosted Git Server

**Page Objective:** This section guides you through configuring a secure, private Git server on your own machine, which will host your encrypted password store.

Having your own Git server ensures your password data remains under your control. We will cover installing Git, creating a dedicated user for security, setting up a bare repository for `pass`, and configuring SSH access for your clients.

## Topics Covered:

1.  **[Installing Git on the Server](./4.1_Installing_Git.md)**:
    Instructions for installing the Git version control system on your Linux server, which is essential for managing and synchronizing your password store.

2.  **[Creating a Dedicated User (Recommended)](./4.2_Dedicated_User.md)**:
    Best practices for creating a separate, unprivileged user account on your server specifically for Git operations, enhancing security by isolating `pass` data.

3.  **[Creating the Bare Git Repository](./4.3_Bare_Repository.md)**:
    How to initialize a "bare" Git repository on your server. This special type of repository is used for sharing and does not have a working directory.

4.  **[Configuring SSH Access for Clients](./4.4_SSH_Access.md)**:
    Setting up secure SSH key-based authentication to allow your `pass` clients (desktop, mobile) to connect and synchronize with your private Git server without using passwords.

---
⬅️ **Previous: [Configuring `gpg-agent` on Arch Linux with KDE](../03_GPG_Setup/3.3_GPG_Agent_KDE.md)**

🏠 **Main Index: [Ultimate Pass Guide](../README.md)**

➡️ **Next: [Installing Git on the Server](./4.1_Installing_Git.md)**
