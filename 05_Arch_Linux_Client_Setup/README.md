# 💻 Arch Linux Client Setup (KDE/Wayland)

**Page Objective:** This section details how to set up `pass` (The Standard Unix Password Manager) on your Arch Linux desktop running KDE Plasma with Wayland.

We will cover installing `pass` and all necessary companion tools, initializing your local password store, connecting it to your self-hosted Git server, and ensuring clipboard integration works correctly under Wayland.

## Topics Covered:

1.  **[Installing `pass` and Related Tools](./5.1_Installing_Pass.md)**:
    Instructions for installing `pass`, `git`, `qtpass` (a Qt-based GUI for `pass`), `wl-clipboard` (for Wayland clipboard support), and other useful utilities like `pass-otp`.

2.  **[Initializing `pass` with Your GPG Key](./5.2_Initializing_Pass_GPG.md)**:
    How to initialize your password store using the GPG encryption subkey you generated earlier, ensuring all your passwords will be encrypted with your key.

3.  **[Initializing the Git Repository for `pass`](./5.3_Initializing_Git_Repo.md)**:
    Setting up the local Git repository within your password store directory and connecting it to the remote bare repository on your self-hosted server.

4.  **[First Commit and Push & `.gitignore`](./5.4_First_Commit_Push.md)**:
    Making your first password entry, committing the initial configuration (like the `.gpg-id` file), adding a `.gitignore` file, and pushing everything to your server to verify the synchronization setup.

---
⬅️ **Previous: [Configuring SSH Access for Clients](../04_Server_Git_Setup/4.4_Configuring_SSH_Access.md)**

🏠 **Main Index: [Ultimate Pass Guide](../README.md)**

➡️ **Next: [Installing `pass` and Related Tools](./5.1_Installing_Pass.md)**
