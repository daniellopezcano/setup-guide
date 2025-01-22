**Function and Purpose of `.zshrc`**
- **Purpose**: `.zshrc` is used to configure and customize the interactive behavior of Zsh, a modern shell. It is executed whenever you start an **interactive Zsh shell**, whether it’s a login or non-login session.
- **Typical Contents**:
    - Shell-specific configurations, such as aliases, prompts, and key bindings.
    - Initialization of plugins and themes (e.g., Oh My Zsh).
    - Loading other configuration files (like `.bashrc` or `.profile`).
- **Name and Origin**:
    - `.zshrc` stands for "Zsh run commands." It serves a similar purpose to `.bashrc` in Bash.
    - Exclusively used in Zsh, a more advanced shell designed to replace traditional ones like Bash.

```bash
# ~/.zshrc

# Message indicating the start of execution
echo -e "\033[1;34m[INFO]\033[0m Starting .zshrc execution..."

##########

# Avoid sourcing if already sourced in the same session
if [ -n "$ZSHRC_ALREADY_SOURCED" ]; then
    echo "[EXE] .zshrc: Already sourced, skipping."
    return
fi
export ZSHRC_ALREADY_SOURCED=1

# Only proceed if the shell is interactive
case $- in
    *i*)
        echo "[EXE] .zshrc: Interactive shell detected, loading configurations..."
        ;;
    *)
        echo "[EXE] .zshr: Non-interactive shell detected, exiting."
        return
        ;;
esac

##########

# Source the .profile file if it exists
echo -e "\033[1;34m[INFO]\033[0m Sourcing .profile..."
if [[ -f "$HOME/.profile" ]]; then  # Check if the .profile file exists
    source "$HOME/.profile" || {  # Source the .profile file
        echo -e "\033[1;31m[ERROR]\033[0m Error sourcing .profile."
        return  # Stop execution if an error occurs
    }
    echo -e "\033[1;32m[SUCCESS]\033[0m .profile sourced successfully."
else
    echo -e "\033[1;33m[WARN]\033[0m .profile not found. Skipping."
fi

# Source the .bashrc file if it exists
echo -e "\033[1;34m[INFO]\033[0m Sourcing .bashrc..."
if [[ -f "$HOME/.bashrc" ]]; then  # Check if the .bashrc file exists
    source "$HOME/.bashrc" || {  # Source the .bashrc file
        echo -e "\033[1;31m[ERROR]\033[0m Error sourcing .bashrc."
        return  # Stop execution if an error occurs
    }
    echo -e "\033[1;32m[SUCCESS]\033[0m .bashrc sourced successfully."
else
    echo -e "\033[1;33m[WARN]\033[0m .bashrc not found. Skipping."
fi

# Set up Oh My Zsh if installed
echo -e "\033[1;34m[INFO]\033[0m Setting up Oh My Zsh..."
export ZSH="$HOME/.oh-my-zsh"  # Define the path to Oh My Zsh

# Set the theme for Oh My Zsh
echo -e "\033[1;34m[INFO]\033[0m Setting Oh My Zsh theme..."
ZSH_THEME="robbyrussell"  # Use the robbyrussell theme

# Enable Oh My Zsh plugins
echo -e "\033[1;34m[INFO]\033[0m Enabling Oh My Zsh plugins..."
plugins=(git zsh-autosuggestions zsh-syntax-highlighting)  # Define the plugins

# Source the Oh My Zsh configuration
echo -e "\033[1;34m[INFO]\033[0m Sourcing Oh My Zsh configuration..."
if [[ -f "$ZSH/oh-my-zsh.sh" ]]; then  # Check if Oh My Zsh is installed
    source "$ZSH/oh-my-zsh.sh" || {  # Source the Oh My Zsh script
        echo -e "\033[1;31m[ERROR]\033[0m Failed to source Oh My Zsh."
        return  # Stop execution if an error occurs
    }
    echo -e "\033[1;32m[SUCCESS]\033[0m Oh My Zsh sourced successfully."
else
    echo -e "\033[1;33m[WARN]\033[0m Oh My Zsh not found. Skipping."
fi

# Message indicating the end of execution
echo -e "\033[1;32m[SUCCESS]\033[0m .zshrc execution completed."
```


