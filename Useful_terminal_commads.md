## Terminal Shortcuts (General)

| **Shortcut/Command** | **Description**                       |
| -------------------- | ------------------------------------- |
| `Ctrl+C`             | Interrupt the current command.        |
| `Ctrl+Z`             | Suspend the current process.          |
| `Ctrl+D`             | Exit the current shell session.       |
| `Ctrl+L`             | Clear the terminal screen.            |
| `Ctrl+Shift+V`       | Paste text into the terminal.         |
| `Ctrl+Shift+C`       | Copy selected text from the terminal. |
| `Ctrl+Shift+N`       | Open a new terminal window.           |
| `Ctrl+Shift+T`       | Open a new terminal tab.              |
| `Ctrl+R`             | Search through command history.       |
## File and Directory Management in Terminal

| **Shortcut/Command**           | **Description**                                      |
|--------------------------------|----------------------------------------------------|
| `ls`                           | List files and directories in the current directory.|
| `ls -la`                       | List all files (including hidden) with details.     |
| `cd path/to/dir`               | Change to the specified directory.                 |
| `pwd`                          | Display the current directory.                     |
| `mkdir folder_name`            | Create a new directory.                            |
| `rm file_name`                 | Delete a file.                                     |
| `rm -r folder_name`            | Delete a directory and its contents.               |
| `cp source destination`        | Copy a file or directory.                          |
| `mv source destination`        | Move or rename a file or directory.                |
| `find /path -name "filename"`  | Search for a file or directory in a specific path.  |
## Process and System Management

| **Shortcut/Command**  | **Description**                                     |
| --------------------- | --------------------------------------------------- |
| `top`                 | Monitor processes in real-time (similar to `htop`). |
| `ps aux`              | List all active processes.                          |
| `kill PID`            | Terminate a process by its PID.                     |
| `pkill process_name`  | Terminate a process by its name.                    |
| `uptime`              | Show how long the system has been running.          |
| `df -h`               | Display disk usage in a human-readable format.      |
| `du -bhs folder_name` | Show the size of a directory.                       |
| `free -h`             | Show available and used RAM.                        |
| `uname -a`            | Display system information.                         |
## Package Management (apt)

| **Shortcut/Command**            | **Description**                            |
| ------------------------------- | ------------------------------------------ |
| `sudo apt update`               | Update the package index.                  |
| `sudo apt upgrade -y`           | Upgrade all installed packages.            |
| `sudo apt install package_name` | Install a package.                         |
| `sudo apt remove package_name`  | Uninstall a package.                       |
| `sudo apt autoremove`           | Automatically remove unnecessary packages. |
| `sudo apt search package_name`  | Search for a package in the repositories.  |
## Networking and Connection Commands

| **Shortcut/Command**                  | **Description**                                     |
|---------------------------------------|---------------------------------------------------|
| `ip a`                                | Show all network interfaces and their IPs.        |
| `ping hostname`                       | Check connectivity with a host (e.g., `ping google.com`). |
| `traceroute hostname`                 | Display the route taken by packets to a host.     |
| `curl http://example.com`             | Download content or make HTTP requests.          |
| `wget http://example.com/file`        | Download files from the web.                     |
| `ssh user@hostname`                   | Connect to a remote server via SSH.              |
## Zsh and Oh My Zsh

| **Shortcut/Command** | **Description**                                   |
| -------------------- | ------------------------------------------------- |
| `Tab`                | Advanced autocompletion.                          |
| `Ctrl+K`             | Clear from the cursor to the end of the line.     |
| `Ctrl+A`             | Move the cursor to the beginning of the line.     |
| `Ctrl+E`             | Move the cursor to the end of the line.           |
| `Ctrl+W`             | Delete the word to the left of the cursor.        |
| `Ctrl+T`             | Swap the last two characters before the cursor.   |
| `Ctrl+Y`             | Paste the last deleted text.                      |
| `source ~/.zshrc`    | Reload Zsh configuration.                         |
| `setopt auto_pushd`  | Enable automatic pushd (directory stack).         |
| `d`                  | Show the directory stack.                         |
| `pushd dir`          | Push a directory onto the stack and switch to it. |
| `popd`               | Pop the top directory off the stack.              |
##  Fzf (Fuzzy Finder)

| **Shortcut/Command**                                                      | **Description**                                                |                                                   |
| ------------------------------------------------------------------------- | -------------------------------------------------------------- | ------------------------------------------------- |
| `fzf`                                                                     | Start an interactive search in the current directory.          |                                                   |
| `find .                                                                   | fzf`                                                           | Search files in the current directory with `fzf`. |
| `grep "pattern"                                                           | fzf`                                                           | Filter `grep` results interactively with `fzf`.   |
| `Alt+C`                                                                   | Quickly navigate between directories.                          |                                                   |
| `Ctrl+T`                                                                  | Search for files and insert their paths into the command line. |                                                   |
| `fzf --preview 'bat --style=numbers --color=always --line-range :500 {}'` | Preview file content while searching.                          |                                                   |
## Screen

| **Shortcut/Command**      | **Description**                                                |                              |
| ------------------------- | -------------------------------------------------------------- | ---------------------------- |
| `screen -S session_name`  | Start a new `screen` session with a custom name.               |                              |
| `Ctrl+a+d`                | Detach from the current session, leaving it in the background. |                              |
| `screen -ls`              | List all active `screen` sessions.                             |                              |
| `screen -r session_name`  | Reattach to a specific detached session by name.               |                              |
| `Ctrl+a+c`                | Create a new window within the session.                        |                              |
| `Ctrl+a n` / `Ctrl+a p`   | Switch to the next/previous window.                            |                              |
| `Ctrl+a w`                | List all windows in the current session.                       |                              |
| `Ctrl+a S`                | Split the screen horizontally into multiple regions.           |                              |
| `Ctrl+a                   | `                                                              | Split the screen vertically. |
| `Ctrl+a Tab`              | Switch between split regions.                                  |                              |
| `Ctrl+a X`                | Close the active region.                                       |                              |
| `Ctrl+a H`                | Start logging the session output to `screenlog.0`.             |                              |
| `Ctrl+a :multiuser on`    | Enable multiuser mode to share the session.                    |                              |
| `Ctrl+a :acladd username` | Grant another user access to the session (on the same system). |                              |

## TRIM

| **Shortcut/Command**             | **Description**                                           |
|----------------------------------|---------------------------------------------------------|
| `sudo fstrim -v /`               | Execute TRIM manually on the root partition.            |
| `sudo fstrim -v /home`           | Execute TRIM manually on the `/home` partition.         |
| `sudo systemctl start fstrim.timer` | Enable automatic TRIM.                                |
| `systemctl status fstrim.timer`  | Verify if the TRIM timer is active.                     |
| `sudo systemctl enable fstrim.timer` | Set TRIM to run automatically at regular intervals. |
## lm-sensors

| **Shortcut/Command**  | **Description**                                               |
| --------------------- | ------------------------------------------------------------- |
| `sensors`             | Display temperatures, voltages, and fan speeds.               |
| `sudo sensors-detect` | Detect available hardware sensors.                            |
| `watch -n 2 sensors`  | Continuously monitor hardware temperatures (every 2 seconds). |

## Conda Environment Management

| **Command**                          | **Description**                                                    |
|--------------------------------------|--------------------------------------------------------------------|
| `conda create --name env_name`       | Create a new environment.                                          |
| `conda create --name env_name python=3.9` | Create a new environment with Python 3.9.                         |
| `conda remove --name env_name --all` | Remove an environment and all its packages.                       |
| `conda env list`                     | List all available environments.                                   |
| `conda activate env_name`            | Activate an environment.                                           |
| `conda deactivate`                   | Deactivate the current environment.                                |
| `conda env export > env_name.yml`    | Export the environment to a YAML file.                             |
| `conda env create -f env_name.yml`   | Recreate an environment from a YAML file.                          |
