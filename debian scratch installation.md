# 1. **Preparation**
1. On a working system, visit the [Debian Official Website](https://www.debian.org/download).
2. Select the **Stable** release for reliability. Opt for:
   - **Netinst ISO**: Minimal installation (downloads required packages during setup).
   - **DVD ISO**: For offline installation (contains more prepackaged software).
3. Insert a USB drive (minimum 4GB for netinst, 8GB for DVD ISO).
4. Identify the USB device:
   ```bash
   lsblk
   ```
   - Note the device name (e.g., `/dev/sdb`).
5. download balena etcher to mount the iso on the usb https://etcher.balena.io/ Follow the instructions to create the bootable with the linux iso
---
# 2. **Prepare the New Machine**
1. Insert the bootable USB.
2. Power on the machine and access BIOS/UEFI (common keys: `Del`, `F2`, `F12`, `Esc`).
3. **Set Boot Priority**:
   - Enable UEFI (recommended).
   - Set the USB as the primary boot device.
4. **Disable Secure Boot** if needed (for unsigned drivers or firmware).
---
# 3. Install Debian
## 3.1 Boot into the Installer
Restart the machine with the USB inserted. The system should boot into the Debian installer. If it doesn’t:
- Recheck BIOS settings.
- Confirm the USB was prepared correctly.
## 3.2 Follow the Installer Steps
#### Key Steps:
- **Language**: Choose a language for the installer.
- **Keyboard Layout**: Match your physical keyboard layout.
- **Network Configuration**:
  - Wired connections are auto-detected.
  - For Wi-Fi, ensure the firmware is loaded, or prepare it beforehand by downloading the appropriate firmware packages (e.g., `firmware-iwlwifi`) and placing them on a USB drive.
## 3.3 Partition Disks
Choose **Guided - Use Entire Disk** for simplicity or **Manual** for advanced setups.
#### Suggested Layout:
- `/` (Root): 20-50GB.
- `swap`: Match your RAM if ≤16GB; otherwise, 2-4GB.
- `/home`: Remaining space for user data.
## 3.4 Set Up User Accounts
- **Root Password**: Optional (recommended to create an admin user instead).
- **Admin User**: Create a username and password for daily use (e.g., `dlopez`).
---
# 4. Post-Installation Setup
## 4.1 Remove USB
Avoid re-entering the installer on reboot by removing the installation USB drive.
## 4.2 Reboot and Update
Log in and run:
```bash
sudo apt update && sudo apt upgrade -y
```
## 4.3 Resolve User Permissions Issue
If the admin user cannot run `sudo` commands, ensure the user is added to the `sudo` group:
1. Switch to the root account:
   ```bash
   su -
   ```
2. Add the user to the `sudo` group:
   ```bash
   usermod -aG sudo user
   ```
3. Log out and back in, or reboot for changes to take effect.
---
# 5. Install and Configure Essential Tools
## 5.1 Install Essential Tools
Install commonly used tools and utilities, including a C compiler for building software from source and `curl` for downloading scripts:
```bash
sudo apt install build-essential curl wget git vim -y
```
## 5.2 Install Drivers
For hardware requiring proprietary drivers:
- **NVIDIA GPUs**:
  ```bash
  sudo apt install nvidia-driver
  ```
- **AMD GPUs**:
  ```bash
  sudo apt install firmware-amd-graphics
  ```
- **Wi-Fi or Ethernet Firmware**:
  ```bash
  sudo apt install firmware-iwlwifi firmware-linux-free
  ```
## 5.3 Install Desktop Environment
If not installed during setup, you can add a desktop environment:
```bash
sudo apt install task-gnome-desktop
```
## 5.4 Install GNOME Tweaks and Dash-to-Panel
1. Install GNOME Tweaks for customizing your desktop:
   ```bash
   sudo apt install gnome-tweaks
   ```
2. Install `Dash-to-Panel` for a more accessible taskbar:
   ```bash
   sudo apt install gnome-shell-extension-dash-to-panel
   ```
3. Enable and configure the extension:
   ```bash
   gnome-extensions enable dash-to-panel@jderose9.github.com
   gnome-extensions prefs dash-to-panel
   ```
   - Disable intelligent auto-hide to keep the taskbar always visible.
## 5.5 Add Flatpak for Modern Applications
Flatpak provides access to a wide range of modern applications:
```bash
sudo apt install flatpak gnome-software-plugin-flatpak
sudo flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```
## 5.6 Pin Applications to the Taskbar
To pin Google Chrome to the taskbar:
1. Create a `.desktop` file for Chrome:
   ```bash
   nano ~/.local/share/applications/google-chrome.desktop
   ```
   Add the following content:
   ```
   [Desktop Entry]
   Version=1.0
   Name=Google Chrome
   Exec=/home/dlopez/chrome/opt/google/chrome/google-chrome %U
   Terminal=false
   Icon=/home/dlopez/chrome/opt/google/chrome/product_logo_256.png
   Type=Application
   Categories=Network;WebBrowser;
   StartupNotify=true
   ```
Save the file and add Chrome to favorites via GNOME Tweaks or by right-clicking its icon.