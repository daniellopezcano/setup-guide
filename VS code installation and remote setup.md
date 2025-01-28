# Installation

1. **Download the `.deb` file**: Go to [Visual Studio Code Download](https://code.visualstudio.com/download) and download the Debian `.deb` package. 
2. **Install the package**      Open a terminal, navigate to the download directory, and install:
   ```bash
   cd ~/Downloads
   sudo dpkg -i code_1.96.3-1736454372_amd64.deb
   sudo apt --fix-broken install # Fix dependencies if needed
     ```
3. **Verify installation** Check the installed version and ensure VS Code runs:
   ```bash
	code --version
	code  # Launch VS Code
    ```
4. Install extensions
	Press `Ctrl+Shift+X` to open the Extensions panel.
	- **Python**: Support for Python, Jupyter Notebooks, and Conda.
	- **Jupyter**: Run and debug notebooks directly in VS Code.
	- **Codeium**: AI Coding Autocomplete  



# Remote Setup Guide for VS Code

## **1. Connect to the Remote Server**

Make sure to create an SSH key pair configured for your remote server: [[SSH Keygen]]
### **Step 1: Create or Update Your SSH Config File**
1. Open your terminal.
2. Edit/check your SSH configuration file (`~/.ssh/config`) with a text editor:
   ```bash
   nano ~/.ssh/config
   ```
3. Find where the following lines are written specifying which IdentityFile should be used for the keygen exchange to your the remote server (in this case dlopez@atlas-248.sw.ehu.es):
   ```plaintext
   Host atlas248
       HostName atlas-248.sw.ehu.es
       User dlopez
       IdentityFile ~/.ssh/id_ed25519_atlas248
   ```
   - **Host:** Alias for the server (e.g., `atlas`). You will use this alias in VS Code.
   - **HostName:** The domain or IP address of the remote server.
   - **User:** Your username on the remote server.
   - **IdentityFile:** Path to your SSH private key (adjust if it differs).
4. Save the file and exit (in nano, press `Ctrl+O`, `Enter`, and `Ctrl+X`).

5. Test your SSH connection using the alias:
   ```bash
   ssh atlas248
   ```
   If you connect successfully without errors, continue to the next step.

---
## **2. Install and Configure the Remote-SSH Extension in VS Code**

1. Open VS Code.
2. Go to the Extensions view by pressing `Ctrl+Shift+X`.
3. Search for **Remote - SSH**.
4. Click **Install** to add the extension.

The Remote-SSH extension automatically uses the SSH config file located at `~/.ssh/config`. You do not need to manually enter the SSH details again.

---
## **3. Connect to the Remote Server**

### **Step 1: Open the Command Palette**
1. Press `Ctrl+Shift+P` to open the Command Palette in VS Code.
2. Type and select:
   ```
   Remote-SSH: Connect to Host
   ```
### **Step 2: Select the Host**
1. From the list of configured hosts, select the alias you created (e.g., `atlas248`).
2. If prompted, choose the platform of the remote server (e.g., Linux).
### **Step 3: Authenticate (if necessary)**
If you configured your SSH key correctly, the connection will be established automatically without a password. If not:
- Ensure your SSH key is added to the SSH agent:
  ```bash
  ssh-add ~/.ssh/id_ed25519_atlas248
  ```
---
## **4. Troubleshooting Common Issues**
### **1. Permission Denied (Public Key)**
- Ensure your public key is added to the remote server's `~/.ssh/authorized_keys` file.
- Verify the permissions of your private key:
  ```bash
  chmod 600 ~/.ssh/id_ed25519_atlas248
  ```
### **2. Remote-SSH Connection Fails**
- Check the Remote-SSH logs:
  1. Go to **View > Output** in VS Code.
  2. Select **Remote - SSH** in the dropdown menu.
  3. Review the logs for specific errors.
### **3. Clear Remote-SSH Cache**
If previous attempts created issues, clear the cached server installation:
```bash
rm -rf ~/.vscode-server
```
Reconnect after clearing the cache.




# Install Extensions on the Remote Server

**Open the Extensions Panel**:
1. Press `Ctrl+Shift+X` to open the Extensions panel.
2. Search for the required extension (e.g., Python).
**Install Extensions Remotely**:
1. In the Extensions view, ensure you are in the remote context (e.g., `SSH: atlas-248.sw.ehu.es`).
2. Click on the **gear icon (⚙️)** next to the extension and select:
   ```
   Install on SSH: atlas-248.sw.ehu.es
   ```
3. Wait for the installation to complete.




# **Set Up Python and virtual envs on the Remote Server**
[[miniconda installation]]
[[Setup Python Virtual Environments]]




# **Configure VS Code to Use the Remote Environment**

### **Step 1: Select the Python Interpreter**
1. Press `Ctrl+Shift+P` and search for:
   ```
   Python: Select Interpreter
   ```
2. Select the path to the Pyt---hon interpreter in your Conda environment:
   ```
   /path/to/miniconda3/envs/my_env/bin/python
   ```




# ** Run Jupyter Notebooks on the Remote Server Using VS Code's Jupyter Integration**

### Step 1: Install the Jupyter Extension in VS Code
1. Open the Extensions tab in VS Code (**Ctrl+Shift+X**).
2. Search for and install **Jupyter** (developed by Microsoft).
3. Make sure the **Python** extension is also installed.
### Step 2: Create or Open a Notebook
1. Create a new file with the `.ipynb` extension or open an existing one.
2. To create a new notebook, press **Ctrl+Shift+P** and search for:
   ```plaintext
   Jupyter: Create New Blank Notebook
   ```
### Step 3: Select the Kernel
1. In the open notebook, click on the top-right corner where it says **"Select Kernel"**.
2. Select the kernel registered as:
   ```plaintext
   Python (VE)
   ```
3. If it doesn’t appear, ensure the `VE` environment is registered correctly with the following command:
   ```bash
   python -m ipykernel install --user --name=VE --display-name "Python (VE)"
   ```




# Other usefull tools
[[git guide]]
[[codeium guide]]