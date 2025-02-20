## **1. Overview**
This guide provides a **step-by-step** method to install **CUDA, cuDNN, and PyTorch with GPU acceleration** on **Linux distributions (Debian, Ubuntu, CentOS/RHEL, etc.)**. It includes troubleshooting tips, explanations of configurations, and verification steps.
### **System Requirements**
- **Linux-based OS (Debian, Ubuntu, CentOS/RHEL, etc.)**
- **NVIDIA GPU** (Compatible with CUDA)
- **Python 3.9+ (Conda recommended)**
- **Sufficient disk space (~10GB)**

## **2. Verify GPU and NVIDIA Driver**
Before installing CUDA, check that your system detects the **NVIDIA GPU** and that no conflicting drivers are installed.
### **2.1 Check if the GPU is detected**
```bash
lspci | grep -i nvidia
```
If your GPU is listed, proceed. Otherwise, check hardware connections.
### **2.2 Check if NVIDIA drivers are installed**
```bash
nvidia-smi
```
If you see an error like **"command not found"**, it means the NVIDIA driver is not installed.
### **2.3 Check if Nouveau driver is active (must be disabled)**
```bash
lsmod | grep nvidia
```
If no NVIDIA driver is listed, proceed with installation.

## **3. Install NVIDIA Drivers**
### **3.1 Enable Additional Repositories (for Debian-based distros)**
Edit the repository sources list (**Debian-based only**):
```bash
sudo nano /etc/apt/sources.list
```
Ensure these lines are present:
```
deb http://deb.debian.org/debian bookworm main contrib non-free non-free-firmware
deb http://security.debian.org/debian-security bookworm-security main contrib non-free non-free-firmware
deb http://deb.debian.org/debian bookworm-updates main contrib non-free non-free-firmware
```
Save the file (`Ctrl + X`, `Y`, `Enter`).
For **Ubuntu**, enable proprietary drivers via:
```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
```
For **CentOS/RHEL**, enable the **EPEL** repository:
```bash
sudo yum install epel-release -y
```
### **3.2 Update package lists**
For Debian/Ubuntu:
```bash
sudo apt update && sudo apt upgrade -y
```
For CentOS/RHEL:
```bash
sudo yum update -y
```
### **3.3 Install the NVIDIA driver**
For **Debian-based distros (Debian/Ubuntu)**:
```bash
sudo apt install -y nvidia-driver firmware-misc-nonfree
```
For **Ubuntu (alternative method)**:
```bash
sudo ubuntu-drivers install
```
For **RHEL/CentOS**:
```bash
sudo yum install -y nvidia-driver kernel-devel
```
After installation, **reboot the system**:
```bash
sudo reboot
```
### **3.4 Verify NVIDIA Driver Installation**
```bash
nvidia-smi
```
If the output shows driver version and GPU details, the installation was successful.

## **4. Install CUDA Toolkit**
### **4.1 Find CUDA version supported by the driver**
```bash
nvidia-smi
```
It should display **CUDA Version: 12.x** (or similar).
### **4.2 Download and install the CUDA repository key**
For **Debian/Ubuntu**:
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/YOUR_DISTRO/x86_64/cuda-keyring_1.1-1_all.deb
sudo dpkg -i cuda-keyring_1.1-1_all.deb
```
Replace `YOUR_DISTRO` with `debian12`, `ubuntu2204`, etc.
For **RHEL/CentOS**:
```bash
sudo rpm -i https://developer.download.nvidia.com/compute/cuda/repos/rhel8/x86_64/cuda-repo-rhel8-<version>.rpm
```
### **4.3 Update package lists**
For **Debian/Ubuntu**:
```bash
sudo apt update
```
For **RHEL/CentOS**:
```bash
sudo yum update
```
### **4.4 Install CUDA Toolkit**
For **Debian/Ubuntu**:
```bash
sudo apt install -y cuda-toolkit-12-x  # Replace `x` with the correct version
```
For **RHEL/CentOS**:
```bash
sudo yum install -y cuda-toolkit-12-x
```
### **4.5 Set environment variables**
```bash
echo 'export PATH=/usr/local/cuda-12.x/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-12.x/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```
### **4.6 Verify CUDA Installation**
```bash
nvcc --version
```
Expected output:
```
nvcc: NVIDIA (R) Cuda compiler
release 12.x, V12.x.XX, build XXXXXXX
```

## **5. Install cuDNN**
### **5.1 Install cuDNN for CUDA**
For **Debian/Ubuntu**:
```bash
sudo apt install -y cudnn-cuda-12
```
For **RHEL/CentOS**, download `.rpm` packages from the **NVIDIA Developer Website**.
### **5.2 Verify cuDNN Installation**
For **Debian/Ubuntu**:
```bash
dpkg -l | grep cudnn
```
For **RHEL/CentOS**:
```bash
rpm -qa | grep cudnn
```

## **6. Install PyTorch with CUDA Support**
### **6.1 Create a Virtual Environment (Recommended)**
```bash
conda create --name pytorch_env python=3.10 -y
conda activate pytorch_env
```
### **6.2 Install PyTorch with CUDA**
```bash
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
```
### **6.3 Verify PyTorch Installation**
Run the following in Python:
```python
import torch
print("PyTorch version:", torch.__version__)
print("CUDA available:", torch.cuda.is_available())
print("CUDA version:", torch.version.cuda)
print("GPU:", torch.cuda.get_device_name(0) if torch.cuda.is_available() else "No GPU detected")
```
Expected output:
```
PyTorch version: 2.5.1+cu121
CUDA available: True
CUDA version: 12.x
GPU: NVIDIA GPU Model
```

## **7. Final Verification and Performance Tuning**
### **7.1 Test PyTorch GPU Computation**
Run this script:
```python
import torch

cpu_tensor = torch.rand(3, 3)
print("Tensor on CPU:", cpu_tensor)

gpu_tensor = cpu_tensor.to("cuda")
print("Tensor on GPU:", gpu_tensor)
```
If the GPU tensor prints correctly, CUDA is fully working.
### **7.2 Enable cuDNN Optimizations (Optional)**
For better performance:
```python
torch.backends.cudnn.benchmark = True
torch.backends.cudnn.enabled = True
```

## **8. Troubleshooting**
### **Problem: `nvidia-smi` command not found**
Solution: Reinstall NVIDIA driver:
```bash
sudo apt install --reinstall nvidia-driver  # Debian/Ubuntu
sudo yum reinstall nvidia-driver  # RHEL/CentOS
```
### **Problem: `nvcc --version` not found**
Solution: Ensure CUDA is in the `PATH`:
```bash
echo 'export PATH=/usr/local/cuda-12.x/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```
### **Problem: PyTorch not detecting CUDA (`torch.cuda.is_available() = False`)**
Solution: Verify installation paths and restart the system.