
This guide assumes no preinstalled operating system on your machine and provides steps for preparing the USB drive and installing Debian on a bare-metal system.

---
## 1. **Preparation**
### 1.1 **Download the Debian ISO**
1. On a working system, visit the [Debian Official Website](https://www.debian.org/download).
2. Select the **Stable** release for reliability. Opt for:
   - **Netinst ISO**: Minimal installation (downloads required packages during setup).
   - **DVD ISO**: For offline installation (contains more prepackaged software).

**Why Stable?**
- Provides maximum reliability and long-term support, ideal for production or personal systems.

---

### 1.2 **Create a Bootable USB**
#### 1.2.1 **From a Linux/Mac System (Preferred)**
1. Insert a USB drive (minimum 4GB for netinst, 8GB for DVD ISO).
2. Identify the USB device:
   ```bash
   lsblk
   ```
   - Note the device name (e.g., `/dev/sdb`).
3. Use `dd` to create the bootable USB:
   ```bash
   sudo dd if=/path/to/debian.iso of=/dev/sdX bs=4M status=progress
   sync
   ```
   Replace `/dev/sdX` with your USB device name (e.g., `/dev/sdb`).

**Caution**: Double-check the device name to avoid overwriting important disks.

---

#### 1.2.2 **From Another Debian System**
If available, use `cp` or tools like `gnome-disk-utility`:
- **Copy with `cp`**:
   ```bash
   sudo cp /path/to/debian.iso /dev/sdX && sync
   ```
- **Graphical Tool**:
   - Open **Disks** (`gnome-disk-utility`), select the USB, and restore the ISO.

---

#### 1.2.3 **Alternative Method (Ventoy)**
If you have access to a system for USB preparation:
1. Download [Ventoy](https://www.ventoy.net/).
2. Install Ventoy on the USB:
   ```bash
   sudo bash Ventoy2Disk.sh -i /dev/sdX
   ```
3. Copy the Debian ISO to the USB. Ventoy allows multiboot setups.

---

## 2. **Prepare the New Machine**
### 2.1 **Assemble the Hardware**
- Double-check all connections, especially:
  - CPU, RAM, and storage devices.
  - Power supply connections.

### 2.2 **Boot from USB**
1. Insert the bootable USB.
2. Power on the machine and access BIOS/UEFI (common keys: `Del`, `F2`, `F12`, `Esc`).
3. **Set Boot Priority**:
   - Enable UEFI (recommended).
   - Set the USB as the primary boot device.
4. **Disable Secure Boot** if needed (for unsigned drivers or firmware).

---

## 3. **Install Debian**
### 3.1 **Boot into the Installer**
1. Restart the machine with the USB inserted.
2. The system should boot into the Debian installer. If it doesn’t:
   - Recheck BIOS settings.
   - Confirm the USB was prepared correctly.

---

### 3.2 **Follow the Installer Steps**
#### Key Steps:
1. **Language**: Choose a language for the installer.
2. **Keyboard Layout**: Match your physical keyboard layout.
3. **Network Configuration**:
   - Wired connections are auto-detected.
   - For Wi-Fi, ensure the firmware is loaded, or prepare it beforehand.

---

### 3.3 **Partition Disks**
1. Choose **Guided - Use Entire Disk** for simplicity or **Manual** for advanced setups.
2. Suggested Layout:
   - `/` (Root): 20-50GB.
   - `swap`: Match your RAM if ≤16GB; otherwise, 2-4GB.
   - `/home`: Remaining space.

---

### 3.4 **Install Base System**
- Select a Debian mirror close to your location for faster downloads.

---

### 3.5 **Set Up User Accounts**
- **Root Password**: Optional (recommended to create an admin user instead).
- **Admin User**:
   - Username and password for daily use.

---

### 3.6 **Install GRUB Bootloader**
- Install GRUB on the primary drive (e.g., `/dev/sda`) to ensure bootability.

---

## 4. **Post-Installation Setup**
### 4.1 **Remove USB**
- Avoid re-entering the installer on reboot.

### 4.2 **Reboot and Update**
- Log in and run:
```bash
sudo apt update && sudo apt upgrade -y
```

---

### 4.3 **Install Essential Tools**
```bash
sudo apt install build-essential curl wget git vim -y
```

---

### 4.4 **Install Drivers**
- NVIDIA GPUs:
```bash
sudo apt install nvidia-driver
```
- AMD GPUs:
```bash
sudo apt install firmware-amd-graphics
```
- For missing Wi-Fi or Ethernet firmware:
```bash
sudo apt install firmware-iwlwifi firmware-linux-free
```

---

## 5. **Optional Configurations**
### Enable TRIM for SSD:
```bash
sudo systemctl enable fstrim.timer
sudo systemctl start fstrim.timer
```

### Install Desktop Environment:
- Minimal installs may require:
```bash
sudo apt install gnome-desktop-environment
```

---

## 6. **Verification**
- Verify disk and system info:
```bash
df -h
uname -a
```

---

This guide ensures Debian installation from scratch on a machine with no preinstalled OS. Let me know if further clarifications are needed!