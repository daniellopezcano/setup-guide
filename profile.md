**Function and Purpose of `.profile`**
- **Purpose**: `.profile` is used to configure environment variables and execute scripts when you log in for the first time (a **login shell**). This happens when you start a virtual terminal (TTY) or connect via SSH.
- **Typical Contents**:
    - Global environment variables like `PATH`, `EDITOR`, or `LANG`.
    - Initialization of other files (like `.bashrc`).
- **Name and Origin**:
    - Historically derived from UNIX systems and adopted to initialize the user environment upon login.
    - Used in classical shells like the Bourne Shell (`sh`).

```bash
# ~/.profile

# Message indicating the start of execution
echo -e "\033[1;34m[INFO]\033[0m Starting .profile execution..."

# ~/.profile

# Message indicating the start of execution
echo -e "\033[1;34m[INFO]\033[0m Starting .profile execution..."

##########

# Guard to prevent multiple executions
if [ -n "$PROFILE_ALREADY_SOURCED" ]; then
    echo "[EXE] .profile already sourced, skipping."
    return
fi
export PROFILE_ALREADY_SOURCED=1

# If Zsh is available and not already running, source .zshrc
if [ -n "$(command -v zsh)" ] && [ -z "$ZSH_VERSION" ]; then
	echo "[INFO] .profile: Switching to Zsh..."
	exec zsh -l
fi

# Only proceed if the shell is interactive
case $- in
    *i*) echo "[EXE] .profile: Interactive shell detected."; ;;
    *) echo "[EXE] .profile: Non-interactive shell detected, exiting."; return ;;
esac

# Source the appropriate shell configuration
if [ -n "$BASH_VERSION" ] && [ -f "$HOME/.bashrc" ]; then
    echo "[EXE] .profile: Sourcing .bashrc..."
    source "$HOME/.bashrc"
elif [ -n "$ZSH_VERSION" ] && [ -f "$HOME/.zshrc" ]; then
    echo "[EXE] .profile: Sourcing .zshrc..."
    source "$HOME/.zshrc"
fi

echo "[EXE] .profile execution completed."

##########

# Setting up global environment variables
echo -e "\033[1;34m[INFO]\033[0m Setting up global environment variables..."
export PATH="$HOME/bin:$PATH"  # Add ~/bin to PATH for custom scripts
if [[ ":$PATH:" == *":$HOME/bin:"* ]]; then  # Check if PATH was successfully updated
    echo -e "\033[1;32m[SUCCESS]\033[0m PATH updated successfully."
else
    echo -e "\033[1;31m[ERROR]\033[0m Failed to update PATH."
fi

# Set the default text editor to Vim
export EDITOR="vim"
if [[ "$EDITOR" == "vim" ]]; then  # Check if the EDITOR variable is correctly set
    echo -e "\033[1;32m[SUCCESS]\033[0m Editor set to Vim."
else
    echo -e "\033[1;31m[ERROR]\033[0m Failed to set editor."
fi

# Set the system locale to English with UTF-8 encoding
export LANG="en_US.UTF-8"
if [[ "$LANG" == "en_US.UTF-8" ]]; then  # Check if the locale is correctly set
    echo -e "\033[1;32m[SUCCESS]\033[0m Locale set to en_US.UTF-8."
else
    echo -e "\033[1;31m[ERROR]\033[0m Failed to set locale."
fi

# Initialize Conda if available
echo -e "\033[1;34m[INFO]\033[0m Checking for Conda initialization..."
if [[ -f "$HOME/miniconda3/etc/profile.d/conda.sh" ]]; then  # Check if Conda is installed
    echo -e "\033[1;34m[INFO]\033[0m Initializing Conda..."
    . "$HOME/miniconda3/etc/profile.d/conda.sh" || {  # Source the Conda configuration script
        echo -e "\033[1;31m[ERROR]\033[0m Error initializing Conda."
        return  # Stop execution if an error occurs
    }
    echo -e "\033[1;32m[SUCCESS]\033[0m Conda initialized successfully."
else
    echo -e "\033[1;33m[WARN]\033[0m Conda not found. Skipping initialization."
fi

# Configure performance limits for file descriptors
echo -e "\033[1;34m[INFO]\033[0m Setting performance limits..."
ulimit -n 65535  # Increase the maximum number of open file descriptors
if [[ $? -eq 0 ]]; then  # Check if the command executed successfully
    echo -e "\033[1;32m[SUCCESS]\033[0m Performance limits set successfully."
else
    echo -e "\033[1;31m[ERROR]\033[0m Failed to set performance limits."
fi

# Configure SSH agent forwarding
echo -e "\033[1;34m[INFO]\033[0m Configuring SSH agent forwarding..."
export SSH_AUTH_SOCK="$XDG_RUNTIME_DIR/ssh-agent.socket"  # Define the SSH agent socket
if [[ "$SSH_AUTH_SOCK" == "$XDG_RUNTIME_DIR/ssh-agent.socket" ]]; then  # Check if the variable is correctly set
    echo -e "\033[1;32m[SUCCESS]\033[0m SSH agent forwarding configured."
else
    echo -e "\033[1;31m[ERROR]\033[0m Failed to configure SSH agent forwarding."
fi

# Message indicating the end of execution
echo -e "\033[1;32m[SUCCESS]\033[0m .profile execution completed."
```




```bash
if [ "$HOSTNAME" = "atlas-248" ];
then
	
	module purge
	module load OpenMPI/4.0.3-GCC-9.3.0
	module load FFTW/3.3.8-gompi-2020a
	module load GSL/2.6-GCC-9.3.0
	module load HDF5/1.10.6-gompi-2020a
	module load CMake/.3.16.4-GCCcore-9.3.0
	module load OpenBLAS/0.3.9-GCC-9.3.0
	
	source ~/.bashrc
	conda deactivate

	## conda activate VE_20221210
	
	export JUPYTER_CONFIG_DIR=/lscratch/dlopez/programs/.jupyter
	export JUPYTER_DATA_DIR=/lscratch/dlopez/programs/.jupyter

else
	module purge

	## This is for having mpicc and the MPI functions
	module load OpenMPI/2.1.1-GCC-6.4.0-2.28

	## This is for having the GSL libraries
	module load GSL/2.4-GCCcore-6.4.0

	## This is for having HDF5 i/o libraries
	## It also loads FFTW3, which is a required dependency
	module load HDF5/1.10.1-foss-2017b

	## Python module
	module load Python/2.7.14-foss-2017b
fi

export SYSTYPE=DIPC

## This is for having color-tagged results when executing ls
alias ls='ls -lhrt --color'
```