## 1. What is Zsh and Why Use It?
- **Zsh**: It is an advanced shell that extends the functionalities of Bash with features like advanced autocompletion, command suggestions, and customization options.
- **Oh My Zsh**: A framework that simplifies Zsh configuration with plugins, themes, and utilities.
### Advantages of Using Zsh:
- Improved autocompletion and syntax highlighting.
- Automatic suggestions based on previous commands.
- Highly customizable with themes and plugins.

---
## 2. Zsh Installation Without sudo permissions

```bash
# Download the latest Zsh source code archive.
wget https://sourceforge.net/projects/zsh/files/latest/download -O zsh-latest.tar.gz

# Extract the downloaded tar.gz file.
tar -xvf zsh-latest.tar.gz

# Remove the tar.gz file to save space.
rm -r zsh-latest.tar.gz

# Navigate into the extracted Zsh source directory.
cd zsh-*

# Configure the build process to install Zsh in the user's home directory.
./configure --prefix=$HOME/zsh

# Compile the source code into an executable binary.
make

# Install the compiled binaries into the specified prefix directory ($HOME/zsh).
make install

export SHELL=$HOME/zsh/bin/zsh

echo $SHELL  # This should output the path to Zsh: $HOME/zsh/bin/zsh
```

---
## 3. Install Oh My Zsh
   ```bash
   sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
   ```

---
## 4. Installing Zsh Plugins
### Recommended Plugins:
#### **zsh-autosuggestions**
- **Function**: Provides command suggestions based on history.
- **Installation**:
  ```bash
  git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
  ```
#### **zsh-syntax-highlighting**
- **Function**: Highlights command syntax based on validity.
- **Installation**:
  ```bash
  git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
  ```
#### Useful Commands:
- Use previously written commands with suggestions from `zsh-autosuggestions`.
- Check for syntax errors with `zsh-syntax-highlighting`.
---
## 5. Installing Additional Tools

### **5.1 `fzf` (Fuzzy Finder)**
- **Function**: Interactive search in files, history, etc.
- **Installation**:
  ```bash
	# Install `fzf`
    git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
    ~/.fzf/install --all
    
	# verify installation
    fzf --version
   ```
### **5.2** `screen` **(Terminal Multiplexer)**
- **Function**: Manage multiple terminal sessions and maintain persistent processes across disconnections.
- **Installation**:
    ```
    sudo apt update
    sudo apt install screen
    screen --version
    ```
For a manual see: [[screen]]
### **5.3 `htop` (System Monitor)**

- **Function**: Visual monitor for processes, CPU, memory, and more.
- **Installation**:
  ```bash
  sudo apt install htop -y
  ```
- **Useful Commands**:
    - `htop`: Opens the interactive monitor.
    - Navigate with arrow keys and use `F9` to kill processes.

---

## 6. Advanced Configuration for Linux

### **6.1 Enable TRIM for SSD**

#### **What is TRIM?**
TRIM allows the operating system to inform the SSD which blocks are no longer in use, optimizing performance and extending its lifespan.
#### **Steps for installation and configuration:**
1. **Run TRIM manually:**
    - For the root partition (`/`):
     ```bash
     sudo fstrim -v /
     ```
   - For other partitions (like `/home`):
     ```bash
     sudo fstrim -v /home
     ```
2. Enable automatic TRIM:
   ```bash
   sudo systemctl enable fstrim.timer
   sudo systemctl start fstrim.timer
   ```
   - Verify the timer is active:
     ```bash
     systemctl status fstrim.timer
     ```
### **6.2 Install and configure lm-sensors**

#### **What is lm-sensors?**
`lm-sensors` is a tool to monitor temperatures, voltages, and fan speeds of your hardware.
#### **Steps for installation and configuration:**
1. **Install lm-sensors:**
   ```bash
   sudo apt install lm-sensors -y
   ```
2. Detect available sensors:
   ```bash
   sudo sensors-detect
   ```
   - Accept the default options by typing `YES` for each question.
- **Automatically load the necessary modules:**
    - When asked whether to add modules to `/etc/modules`, select yes.

