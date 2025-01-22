This guide explains how to configure SSH key-based authentication to connect to a remote server
## Step 1: Check for Existing SSH Keys
Before creating a new SSH key, verify if keys already exist:
```bash
ls ~/.ssh
```
You should see files like `id_ed25519` and `id_ed25519.pub`. If they exist and are already in use, you can add a new key with a different name (e.g., `id_ed25519_atlas248`).
## Step 2: Generate a New SSH Key
Create a new SSH key without overwriting the existing one:
```bash
ssh-keygen -t ed25519 -C "daniellopezcano13@gmail.com"
```
- When prompted, specify a new filename to avoid overwriting:
  ```plaintext
  Enter file in which to save the key (/home/youruser/.ssh/id_ed25519): /home/youruser/.ssh/id_ed25519_id_ed25519_atlas248
  ```
- Optionally, add a passphrase for extra security.
e- **Parameters**:
  - `-t ed25519`: Specifies the type of key (Ed25519 is faster and more secure than RSA).
  - `-C "your.email@example.com"`: Adds an identifier for the key, typically your email.
To copy the content of the public key file, run:
```bash
cat ~/.ssh/id_ed25519_atlas248.pub
```
## Step 3: Configure the `config` File
Create or edit the SSH configuration file at `~/.ssh/config`:
```bash
cd ~/.ssh
touch config # in case it's not already created
```
Add the following lines to the file, specifying the new key:
```plaintext
Host github.com
  AddKeysToAgent yes
  IdentityFile ~/.ssh/id_ed25519_github
```
Another example:
```bash
Host atlas-248.sw.ehu.es
    HostName atlas-248.sw.ehu.es
    User dlopez
    IdentityFile ~/.ssh/id_ed25519_atlas248  # Change name to the specific id_key
    PubkeyAuthentication yes
    PreferredAuthentications publickey
    KexAlgorithms +diffie-hellman-group14-sha1
    LogLevel DEBUG
```
## Step 4: Add the Key to the SSH Agent
Activate the SSH agent and add the new key:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_atlas248
```
## Step 5: Access the Remote Server
Log into the remote server using your usual credentials:
```bash
ssh dlopez@atlas-248.sw.ehu.es
```
Enter your password when prompted.
## Step 6: Set Up the SSH Key on the Remote Server
1. On the remote server, create the `.ssh` directory if it does not exist and set appropriate permissions:
   ```bash
   mkdir -p ~/.ssh
   chmod 700 ~/.ssh
   ```
2. Open (or create) the `~/.ssh/authorized_keys` file:
   ```bash
   nano ~/.ssh/authorized_keys
   ```
3. Paste the public key you copied in **Step 2** at the end of the file. Save and close.
4. Ensure the file has the correct permissions:
   ```bash
   chmod 600 ~/.ssh/authorized_keys
   ```
## Step 7: Test the Connection
From your local machine, test the connection to ensure it works without requiring a password:
```bash
ssh dlopez@atlas-248.sw.ehu.es -v
```
You should be logged in without being prompted for a password.