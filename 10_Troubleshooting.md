# 🆘 Common Issues & Troubleshooting

**Page Objective:** This page provides solutions and guidance for common problems encountered during the setup and use of `pass` with a self-hosted Git server, GPG, and clients on Arch Linux and Android.

When things go wrong, a systematic approach to troubleshooting can help identify the root cause.

## 1. GPG Issues (Decryption/Encryption Failed)

*   **Symptom (Desktop):** `gpg: decryption failed: No secret key`, `gpg: encryption failed: Unusable public key`, errors from `pass` about GPG.
*   **Symptom (Android/APS):** Passwords don't decrypt, "No GPG key found" errors in APS, OpenKeychain prompts repeatedly or fails.

    **Troubleshooting Steps:**
    1.  **Verify Secret Key Presence (Desktop):**
        ```bash
        gpg --list-secret-keys YOUR_GPG_MASTER_KEY_ID_OR_EMAIL
        ```
        Ensure the master key and, crucially, the **encryption subkey `[E]`** are listed.

    2.  **Check `~/.password-store/.gpg-id` (Desktop & Server Repo):**
        This file (in the root of your `pass` store) *must* contain the correct GPG ID (preferably the long ID or fingerprint of the encryption subkey) for which passwords should be encrypted. If sharing, ensure all relevant IDs are present, one per line.
        ```bash
        cat ~/.password-store/.gpg-id
        ```

    3.  **GPG Key Trust (Desktop):**
        Your GPG key needs to be trusted, at least "marginally" or "fully," by your GPG instance. Usually, your own key has "ultimate" trust.
        ```bash
        gpg --edit-key YOUR_GPG_MASTER_KEY_ID_OR_EMAIL
        # At gpg> prompt:
        # gpg> trust
        # Choose appropriate level (e.g., 5 for ultimate for your own key)
        # gpg> save
        ```

    4.  **`gpg-agent` and `pinentry` (Desktop):**
        *   Ensure `gpg-agent` is running. `ps aux | grep gpg-agent`.
        *   Ensure a `pinentry` program (like `pinentry-qt` for KDE) is installed and configured in `~/.gnupg/gpg-agent.conf`.
        *   Test by manually encrypting/decrypting a file with GPG to see if passphrase prompt appears correctly.

    5.  **Correct GPG Key in OpenKeychain (Android):**
        *   In OpenKeychain, verify that the correct GPG key (containing the encryption subkey) was imported.
        *   In APS settings, ensure this GPG key is selected for decryption.

    6.  **File Permissions for GPG (Less Common):**
        Ensure your `~/.gnupg` directory and its contents have correct, restrictive permissions (usually `drwx------` for the directory, and `rw-------` for files within).

## 2. SSH Connection / Git Sync Issues

*   **Symptom (Desktop):** `pass git push/pull` fails with "Permission denied (publickey)", "Connection timed out", "Host key verification failed", "Repository not found".
*   **Symptom (Android/APS):** APS fails to clone or sync, showing connection or authentication errors.

    **Troubleshooting Steps:**
    1.  **Verbose SSH Output (Desktop):**
        From your client, try connecting with maximum verbosity:
        ```bash
        ssh -vvv YOUR_SERVER_GIT_USER@YOUR_SERVER_HOSTNAME_OR_IP
        # If using a specific key:
        # ssh -vvv -i YOUR_CLIENT_SSH_KEY_PATH YOUR_SERVER_GIT_USER@YOUR_SERVER_HOSTNAME_OR_IP
        ```
        Analyze the output for clues (e.g., which keys are being offered, server response).

    2.  **Server-Side SSH Logs:**
        Check `/var/log/auth.log` (Debian/Ubuntu) or `journalctl -u sshd` (systemd systems) on the **server** for messages related to the failed login attempt.

    3.  **Public Key on Server (`authorized_keys`):**
        *   **Correct Key:** Ensure the *exact* public SSH key from the client is present in `~/.ssh/authorized_keys` on the server, under the `YOUR_SERVER_GIT_USER` account. No extra spaces, no missing characters.
        *   **One Key Per Line:** Each public key must be on its own line.

    4.  **Permissions on Server (CRITICAL):**
        For `YOUR_SERVER_GIT_USER` on the server:
        *   `~/.ssh` directory: `chmod 700 ~/.ssh` (drwx------)
        *   `~/.ssh/authorized_keys` file: `chmod 600 ~/.ssh/authorized_keys` (-rw-------)
        *   Home directory itself (e.g., `/home/YOUR_SERVER_GIT_USER`): Should not be world-writable.

    5.  **SSH Configuration on Server (`/etc/ssh/sshd_config`):**
        *   `PubkeyAuthentication yes` (usually default)
        *   `AuthorizedKeysFile .ssh/authorized_keys` (usually default)
        *   If you made changes, restart the SSH service: `sudo systemctl restart sshd`.

    6.  **Git Remote URL (Client):**
        In `~/.password-store/.git/config` on the client, verify the `[remote "origin"] url = ...` is correct.
        *   Pay attention to the path part, especially for absolute paths on the server (e.g., `user@host:/path/to/repo.git` vs. `user@host:relative/repo.git`).
        *   For APS, the `ssh://user@host//absolute/path/to/repo.git` (double slash) is often needed for absolute paths not in the user's home.

    7.  **Firewall (Server):**
        Ensure your server's firewall is allowing incoming SSH connections on the port SSHD is listening on (default 22).

    8.  **SSH Key Passphrase (Client):**
        If your private SSH key is passphrase-protected, ensure `ssh-agent` is running and has the key added (`ssh-add YOUR_CLIENT_SSH_KEY_PATH`), or you're being prompted for it. For APS, OpenKeychain handles this.

    9.  **Host Key Verification Failed (Client):**
        If you've reinstalled your server or its SSH host keys changed, your client might refuse to connect. Remove the old host key from `~/.ssh/known_hosts` on the client for `YOUR_SERVER_HOSTNAME_OR_IP`. The next connection will prompt you to accept the new host key.

## 3. `pass` Command Not Found / Tooling Issues

*   **Symptom:** `bash: pass: command not found`.
    **Troubleshooting Steps:**
    1.  **Installation:** Ensure `pass` (and `git`, `gpg`, etc.) are correctly installed (see [Section 5.1](../05_Arch_Linux_Client_Setup/5.1_Installing_Pass.md)).
    2.  **`$PATH` Environment Variable:** Make sure the directory where `pass` is installed (usually `/usr/bin/`) is in your system's `$PATH`. `echo $PATH`.

## 4. Clipboard Issues (Wayland/X11)

*   **Symptom:** Password is not copied to clipboard, or copied but cannot be pasted.
    **Troubleshooting Steps:**
    1.  **Wayland:** Ensure `wl-clipboard` is installed (`wl-copy` command should be available).
    2.  **X11:** Ensure `xclip` or `xsel` is installed.
    3.  **`PASSWORD_STORE_CLIP_TIME`:** This environment variable controls how long the password stays in the clipboard (default 45 seconds).
    4.  **Clipboard Manager:** Some clipboard managers might interfere or have their own history settings. Test with a simple clipboard manager or temporarily disable it.

## 5. Android Specific Issues (APS/OpenKeychain)

*   **APS Cannot Find GPG Key:** Ensure key was imported into OpenKeychain, and APS is configured to use it.
*   **APS Cannot Find SSH Key:** Ensure an SSH identity is configured in OpenKeychain and selected/usable by APS.
*   **OpenKeychain Passphrase Prompts:** If OpenKeychain repeatedly asks for passphrases, check its caching settings. Ensure you're entering the correct passphrase.
*   **Background Sync Failures:** Android's battery optimization (Doze mode) can sometimes interfere with background tasks. Manually syncing when opening/closing APS is more reliable.

## General Troubleshooting Approach

1.  **Isolate the Problem:** Is it GPG, Git, SSH, `pass` itself, or a client-specific issue (APS, `qtpass`)?
2.  **Check Logs:** Client-side (terminal output, browser console for extensions) and server-side (SSH logs, system logs).
3.  **Simplify:** Test the most basic component that seems to be failing (e.g., can you SSH to the server as `YOUR_SERVER_GIT_USER` *at all*? Can you manually decrypt a `.gpg` file with `gpg -d`?).
4.  **Verify Permissions:** Incorrect file/directory permissions are a very common source of GPG and SSH issues.
5.  **Check Configuration Files:** `~/.password-store/.gpg-id`, `~/.password-store/.git/config`, `~/.ssh/config`, `/etc/ssh/sshd_config`, `~/.gnupg/gpg.conf`, `~/.gnupg/gpg-agent.conf`.
6.  **Restart Services/Applications:** Sometimes a simple restart of `gpg-agent`, `sshd`, or the client application can resolve temporary glitches.
    ```bash
    # Restart gpg-agent
    gpg-connect-agent reloadagent /bye
    # Restart sshd on server
    sudo systemctl restart sshd
    ```
7.  **Search Online:** Use specific error messages to search for solutions. The Arch Wiki, Stack Exchange sites, and `pass`/`gpg` mailing lists or forums are good resources.

By methodically checking these common areas, you can usually diagnose and resolve most issues with your `pass` setup.

---
⬅️ **Previous: [Server Security Best Practices](../09_Maintenance_Security/9.3_Server_Security.md)**

🏠 **Main Index: [Ultimate Pass Guide](./README.md)**

➡️ **Next: [Conclusion](../11_Conclusion.md)**