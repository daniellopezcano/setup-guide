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

This guide explains how to set up a remote development environment using VS Code and an SSH connection. Prerequisites:
- A remote server with SSH access (e.g., `dlopez@atlas-248.sw.ehu.es`).
- Installed on your local machine:
  - **VS Code**
  - **Remote - SSH Extension** (developed by Microsoft).
  - An SSH key pair configured for your remote server: [[SSH Keygen]]

## **1. Connect to the Remote Server**

### **Step 1: Add SSH Host**
1. Open VS Code.
2. Press `Ctrl+Shift+P` and search for:
   ```
   Remote-SSH: Add New SSH Host
   ```
3. Enter the SSH connection string:
   ```
   dlopez@atlas-248.sw.ehu.es
   ```
4. Choose the SSH configuration file where the host will be saved (usually `~/.ssh/config`).
### **Step 2: Connect to the Host**
1. Press `Ctrl+Shift+P` and search for:
   ```
   Remote-SSH: Connect to Host
   ```
2. Select `atlas` or your configured hostname.
3. A new VS Code window will open, connected to the remote server.
---
## **2. Install Extensions on the Remote Server**

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
---
## **3. Set Up Python and virtual envs on the Remote Server**
[[miniconda installation]]
[[Setup Python Virtual Environments]]

---
## **4. Configure VS Code to Use the Remote Environment**

### **Step 1: Select the Python Interpreter**
1. Press `Ctrl+Shift+P` and search for:
   ```
   Python: Select Interpreter
   ```
2. Select the path to the Pyt---hon interpreter in your Conda environment:
   ```
   /path/to/miniconda3/envs/my_env/bin/python
   ```
---
## **5. Run Jupyter Notebooks on the Remote Server Using VS Code's Jupyter Integration**

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