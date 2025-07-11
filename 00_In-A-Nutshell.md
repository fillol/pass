# 🚀 In a Nutshell / Quick Start

This is a high-speed reference for GNU `pass`, structured for the most common scenarios. It assumes you are already familiar with GPG, Git, and SSH concepts.

**Always replace placeholders** (e.g., `YOUR_GPG_EMAIL`, `YOUR_SERVER_IP`, `YOUR_GIT_USER`) with your actual values.

---

## Adding a New Device to an Existing Setup

This is the most common task. Use these steps to configure a new client to use your existing password store.

### **Adding a New Desktop Client**

1.  **Install Tools:** Install `pass`, `git`, and `gpg` on the new machine.

2.  **Import Your GPG Keys (from your offline backup):**
    ```bash
    gpg --import gpg_public_key_backup.asc
    gpg --import gpg_secret_keys_backup.asc
    # Trust your own master key to make it fully usable
    gpg --edit-key YOUR_GPG_EMAIL # then type: trust -> 5 -> save
    ```

3.  **Add the New Client's SSH Key to the Server:**
    Generate a new, unique SSH key on this client (`ssh-keygen -t ed25519`). Copy the content of the `.pub` file and add it as a new line to `YOUR_GIT_USER`'s `~/.ssh/authorized_keys` file on your server.

4.  **Clone the Repository:**
    ```bash
    git clone YOUR_GIT_USER@YOUR_SERVER_IP:password-store.git ~/.password-store
    # 'pass' will now work. The crucial .gpg-id file is cloned with the repo.
    ```

### **Adding a New Android Client**

1.  **Install the App:** Install [**Password Store**](https://github.com/agrahn/Android-Password-Store) from [F-Droid](https://f-droid.org/en/packages/app.passwordstore.agrahn). This is the actively maintained client for Android.

2.  **Export *Only* the Encryption Subkey from a Desktop:**
    ```bash
    # The '!' suffix is CRITICAL for exporting only the subkey's secret material
    gpg --export-secret-keys --armor YOUR_ENCRYPTION_SUBKEY_ID! > subkey_for_android.asc
    ```

3.  **Transfer & Import:** Securely copy `subkey_for_android.asc` to your phone. Inside the Password Store app, go to `Settings` -> `Cryptographic Key Settings` and import the file.

4.  **Configure the SSH Key:**
    *   In the app's `Settings` -> `SSH Key Settings`, generate a new key.
    *   Copy the **public SSH key** displayed by the app.
    *   Add this public key to the `authorized_keys` file on your server.

5.  **Clone the Repository in the App:** From the app's main screen, initiate the clone process using your repository URL:
    `ssh://YOUR_GIT_USER@YOUR_SERVER_IP/password-store.git`

---

## Full System Setup (First Time or Server Rebuild)

Follow these steps only when creating the GPG keys and server from scratch.

### **1. GPG: Create and Back Up Your Keys (Desktop)**

1.  **Generate Master Key + Subkeys:**
    ```bash
    gpg --expert --full-generate-key # Choose ECC or RSA, set validity
    # Then enter the interactive editor:
    gpg --expert --edit-key YOUR_GPG_EMAIL
    # Use the 'addkey' command to create [E], [S], [A] subkeys, then 'save'.
    ```

2.  **Identify Your Encryption Subkey ID:**
    ```bash
    gpg --list-keys     # List all public keys
    # Note the 16-char ID of the 'ssb' key with the [E] flag
    gpg --list-secret-keys --keyid-format long YOUR_GPG_EMAIL
    ```

3.  **Critical Backup (Store Offline & Securely):** This is your disaster recovery plan.
    ```bash
    gpg --export-secret-keys --armor YOUR_GPG_EMAIL > gpg_secret_keys_backup.asc
    gpg --export --armor YOUR_GPG_EMAIL > gpg_public_key_backup.asc
    gpg --gen-revoke --output gpg_revocation_cert.asc YOUR_GPG_EMAIL
    ```

### **2. Server: Set Up a Bare Git Repository**

1.  **Create a Dedicated, Restricted Git User (as root/sudo):**
    ```bash
    # Install git (e.g., sudo apt install git or sudo pacman -S git)
    sudo adduser YOUR_GIT_USER
    sudo usermod -s /usr/bin/git-shell YOUR_GIT_USER # Restrict shell for security
    ```

2.  **Initialize the Bare Repository (as `YOUR_GIT_USER`):**
    ```bash
    sudo su - YOUR_GIT_USER
    git init --bare password-store.git # Creates repo in the user's home
    exit
    ```
    > **Note for Rebuilding a Server:** If your server breaks, follow these same steps to create a new one. Your clients hold the data. After the new server is ready, update the git remote on your clients (`git remote set-url origin NEW_URL`) and `pass git push` to restore the repository.

### **3. First Desktop Client: Initialize and Push**

1.  **Install Packages (Arch Linux Example):**
    ```bash
    sudo pacman -S pass git qtpass wl-clipboard pass-otp oathtool
    ```

2.  **Authorize Client and Initialize `pass`:**
    *   First, add your desktop client's SSH public key to the server's `authorized_keys` file (see "Adding a New Desktop Client", step 3).
    *   Then, initialize the password store on your client:
    ```bash
    # Use the ENCRYPTION SUBKEY ID from step 1.2
    pass init YOUR_ENCRYPTION_SUBKEY_ID

    cd ~/.password-store
    git init && git branch -M main
    git remote add origin YOUR_GIT_USER@YOUR_SERVER_IP:password-store.git
    
    # Add a test password to create the first commit
    pass insert Test/Desktop-Init
    # Push the store (including the crucial .gpg-id file) to the server
    git push -u origin main
    ```

---

## **Daily Usage Cheatsheet (Desktop CLI)**

```bash
pass                           # List passwords
pass show path/to/entry        # Show & copy password to clipboard
pass -c path/to/entry          # Copy password to clipboard (no show)
pass insert path/to/entry      # Add a new password
pass generate path/to/entry 20 # Generate a new password
pass edit path/to/entry        # Edit an entry
pass rm path/to/entry          # Remove an entry
pass git pull --rebase         # Sync: Pull changes from server
pass git push                  # Sync: Push changes to server
pass otp -c path/to/otp_entry  # Copy OTP code (if using pass-otp)
```