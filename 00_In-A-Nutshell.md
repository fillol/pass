# 🚀 In a Nutshell / Quick Start

This page provides the essential commands for setting up `pass` with a self-hosted Git server, GPG (using subkeys), an Arch Linux client, and an Android client. 
**This is a rapid reference; consult the detailed guide for full explanations and troubleshooting.**

**Replace all placeholders (e.g., `YOUR_GPG_MASTER_KEY_ID_OR_EMAIL`, `YOUR_GPG_ENCRYPTION_SUBKEY_ID`, `YOUR_SERVER_GIT_USER`, `YOUR_SERVER_HOSTNAME_OR_IP`, `YOUR_GIT_REPO_PATH_ON_SERVER`) with your actual values.**

## 1. GPG Key Setup (on Desktop Client)

*   **Generate GPG Keypair (Master + Subkeys):**
    ```bash
    gpg --expert --full-generate-key # Choose ECC or RSA, set validity
    # (Within gpg --edit-key YOUR_GPG_MASTER_KEY_ID_OR_EMAIL)
    # gpg> addkey # Add Encryption subkey [E], Signing subkey [S], Auth subkey [A]
    # gpg> save
    ```
*   **Identify Encryption Subkey ID:**
    ```bash
    gpg --list-secret-keys --keyid-format long YOUR_GPG_MASTER_KEY_ID_OR_EMAIL
    # Note the long ID of the subkey with [E] capability. This is YOUR_GPG_ENCRYPTION_SUBKEY_ID.
    ```
*   **Backup GPG Keys (CRITICAL - Store Offline & Securely):**
    ```bash
    gpg --export-secret-keys --armor YOUR_GPG_MASTER_KEY_ID_OR_EMAIL > ~/gpg_secret_keys_backup.asc
    gpg --export --armor YOUR_GPG_MASTER_KEY_ID_OR_EMAIL > ~/gpg_public_key_backup.asc
    gpg --output ~/gpg_revocation_cert.asc --gen-revoke YOUR_GPG_MASTER_KEY_ID_OR_EMAIL
    ```

## 2. Self-Hosted Git Server Setup

*   **On Server (as root/sudo):**
    ```bash
    # Install Git (e.g., sudo apt install git OR sudo pacman -S git)
    sudo adduser YOUR_SERVER_GIT_USER # Set password, or plan for key-only SSH
    # (Optional, recommended: sudo usermod -s /usr/bin/git-shell YOUR_SERVER_GIT_USER)
    ```
*   **On Server (as `YOUR_SERVER_GIT_USER`):**
    ```bash
    # Example path: /srv/git/password-store.git (adjust YOUR_GIT_REPO_PATH_ON_SERVER accordingly)
    mkdir -p YOUR_GIT_REPO_PATH_ON_SERVER
    cd YOUR_GIT_REPO_PATH_ON_SERVER
    git init --bare
    # Ensure YOUR_SERVER_GIT_USER owns this directory and its contents.
    ```
*   **Configure SSH Access (Server):**
    For *each client device*, add its public SSH key to `~/.ssh/authorized_keys` for `YOUR_SERVER_GIT_USER`.
    ```bash
    # On server, as YOUR_SERVER_GIT_USER:
    mkdir -p ~/.ssh && chmod 700 ~/.ssh
    touch ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys
    # echo "CLIENT_PUBLIC_SSH_KEY_STRING" >> ~/.ssh/authorized_keys
    ```

## 3. Desktop Client Setup (Arch Linux)

*   **Install Packages:**
    ```bash
    sudo pacman -S pass git qtpass wl-clipboard pass-otp oathtool
    ```
*   **Initialize `pass` & Local Git Repo:**
    ```bash
    pass init YOUR_GPG_ENCRYPTION_SUBKEY_ID
    cd ~/.password-store
    git init && git branch -M main # Or your preferred default branch name
    git remote add origin YOUR_SERVER_GIT_USER@YOUR_SERVER_HOSTNAME_OR_IP:YOUR_GIT_REPO_PATH_ON_SERVER
    echo -e "*~\n*.swp\n.DS_Store" > .gitignore # Basic .gitignore
    git add .gpg-id .gitignore
    git commit -m "Initial: GPG ID & gitignore"
    pass insert test/desktop-init # Add a test password
    git push -u origin main
    ```
*   **Configure `gpg-agent` with KDE (see detailed guide [Section 3.3](03_GPG_Setup/3.3_GPG_Agent_KDE.md)).**

## 4. Android Client Setup (APS & OpenKeychain)

1.  **Install Apps:** `Android Password Store (APS)` & `OpenKeychain: Easy PGP` (from F-Droid/Play Store).
2.  **Export GPG Encryption Subkey (from Desktop):**
    ```bash
    # The "!" is crucial for exporting only the subkey's secret material
    gpg --export-secret-keys --armor YOUR_GPG_ENCRYPTION_SUBKEY_ID! > ~/encryption_subkey_for_android.asc
    ```
3.  **Transfer & Import:** Securely transfer `encryption_subkey_for_android.asc` to Android and import into OpenKeychain.
4.  **Configure SSH Key in OpenKeychain:** Generate/select an SSH key in OpenKeychain. Copy its **public SSH key string**.
5.  **Add Android's SSH Key to Server:** Add this public key to `YOUR_SERVER_GIT_USER`'s `~/.ssh/authorized_keys` (see step 2).
6.  **Configure APS:**
    *   Clone repo: `ssh://YOUR_SERVER_GIT_USER@YOUR_SERVER_HOSTNAME_OR_IP/YOUR_GIT_REPO_PATH_ON_SERVER` (Note: path may need `//` if absolute, e.g., `ssh://user@host//srv/repo.git`).
    *   Select imported GPG key & SSH key (via OpenKeychain).
    *   Set Git branch, user name, email.
    *   Sync.

## 5. Basic `pass` Usage (Desktop CLI)

```bash
pass                                      # List passwords
pass insert path/to/entry                 # Add new password
pass generate path/to/entry <length>      # Generate new password
pass show path/to/entry                   # Show & copy password to clipboard
pass -c path/to/entry                     # Copy password to clipboard (no show)
pass edit path/to/entry                   # Edit entry
pass rm path/to/entry                     # Remove entry
pass git pull (--rebase)                  # Sync: Pull changes (rebase recommended)
pass git push                             # Sync: Push changes
pass otp path/to/otp_entry                # Show OTP code (if pass-otp used)
pass otp -c path/to/otp_entry             # Copy OTP code (if pass-otp used)
```

## 6. Adding a New Client (Desktop)

1.  **Install `pass`, `git`, `gpg`**.
2.  **Import GPG Secret Key & Public Key (from backup):**
    ```bash
    gpg --import ~/gpg_public_key_backup.asc
    gpg --import ~/gpg_secret_keys_backup.asc # Or the subkey-only backup if using that model
    gpg --edit-key YOUR_GPG_MASTER_KEY_ID_OR_EMAIL # Then 'trust' -> '5' (ultimate) -> 'save'
    ```
3.  **Setup SSH Key:** Generate/add client's SSH public key to server's `authorized_keys`.
4.  **Clone Repo:**
    ```bash
    git clone YOUR_SERVER_GIT_USER@YOUR_SERVER_HOSTNAME_OR_IP:YOUR_GIT_REPO_PATH_ON_SERVER ~/.password-store
    ```
    `pass` should now work. The `.gpg-id` file is cloned with the repo.

---

➡️ [Back to Main Index](README.md)
