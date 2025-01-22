## Prerequisites
Before starting, ensure you have the following:
- Access to a remote server with Python and Conda installed.
- A terminal application to connect to the remote server via SSH.
---
## 1. Create a Conda Environment and Install Dependencies

### 1.1 Create the Conda Environment
```bash
conda create --name my_env python=3.10 -y
conda activate my_env
```
### 1.2 Install Jupyter and Required Packages
```bash
conda install jupyterlab ipykernel nb_conda_kernels -y
```
---
## 2. Configure Jupyter for the First Time

### 2.1 Generate the Configuration File
```bash
jupyter notebook --generate-config
```
This will create the configuration file at:
```
~/.jupyter/jupyter_notebook_config.py
```
### 2.2 (Optional) Move the Configuration File
To organize your configurations in a custom directory:
```bash
mv ~/.jupyter ~/path/to/config_directory
```
Update the system to recognize the new configuration location:
```bash
export JUPYTER_CONFIG_DIR=~/path/to/config_directory
```
---
## 3. Configure Jupyter for Remote Access
Edit the `jupyter_notebook_config.py` file with the following settings:
```python
# Allow access from all IPs
c.NotebookApp.ip = '0.0.0.0'

# Do not open the browser on the server
c.NotebookApp.open_browser = False

# Use a fixed port for the server (change as needed)
c.NotebookApp.port = 8888

# Disable token authentication (use SSH for security)
c.NotebookApp.token = ''

# Disable password (enable if not using SSH)
c.NotebookApp.password = ''
```

Save the file after making the changes

---
## 4. Launch Jupyter on the Server
Start Jupyter on the server with the specified Conda environment:
```bash
jupyter lab --no-browser --port=8888
```
To check available ports:
```bash
netstat -tulpn | grep LISTEN
```
---

## 5. Access Jupyter from Your Local Machine

### 5.1 Establish an SSH Tunnel
On your local machine, run:
```bash
ssh -L 8888:localhost:8888 user@remote-server
```
- **`-L 8888:localhost:8888`**: Maps local port `8888` to the remote server's port `8888`.
- **`user@remote-server`**: Replace `user` with your username and `remote-server` with the server address.
### 5.2 Open Jupyter in Your Local Browser
Once the SSH tunnel is active, open your browser and go to:
```
http://localhost:8888
```
---
## 6. Troubleshooting

### 6.1 Error: Port Already in Use
Choose a different port:
1. Update the Jupyter configuration file:
   ```bash
   c.NotebookApp.port = 8889
   ```
2. Use the new port for the SSH tunnel:
   ```bash
   ssh -L 8889:localhost:8889 user@remote-server
   ```
### 6.2 Jupyter is Unresponsive
- Check if the Jupyter server is running:
  ```bash
  ps aux | grep jupyter
  ```
- Ensure the server firewall allows the chosen port.
### 6.3 Enable Additional Security
Set a secure password if not using SSH:
```bash
jupyter notebook password
```
This generates a hash in the configuration file to protect your Jupyter server.

---
## 7. Additional Optimizations

### 7.1 Keep the Server Running with `tmux` or `screen`
To prevent Jupyter from stopping when you disconnect, use `tmux` or `screen`: [[screen]]
### 7.2 Verify Available Kernels
If you have multiple Conda environments, ensure they are registered as kernels:
```bash
python -m ipykernel install --user --name=my_env --display-name "Python (my_env)"
```
---
## 8. Best Practices
- Always use SSH tunneling for secure connections.
- Change the default port to avoid conflicts and enhance security.
- Configure SSL certificates for non-SSH access.

---

With this guide, you'll have a fully functional Jupyter setup on your remote server, accessible from your local machine. Enjoy coding!