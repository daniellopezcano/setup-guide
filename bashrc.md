**Function and Purpose of `.bashrc`**
- **Purpose**: `.bashrc` is a configuration script executed every time a **non-login interactive shell** is started (e.g., when you open a new terminal in a graphical environment).
- **Typical Contents**:
    - Aliases (e.g., `alias ll="ls -lah"`).
    - Session-specific variables (e.g., `export PATH`).
    - Configuration for interactive tools like Zsh, Git, or Conda.
- **Name and Origin**:
    - The name derives from **Bash Run Commands** (`rc` is a convention to indicate configuration files).
    - Bash looks for and executes this file to configure the interactive shell environment.

```bash
# ~/.bashrc

# Message indicating the start of execution
echo -e "\033[1;34m[INFO]\033[0m Starting .bashrc execution..."

##########

# Avoid sourcing if already sourced in the same session
if [ -n "$BASHRC_ALREADY_SOURCED" ]; then
    echo "[EXE] .bashrc: Already sourced, skipping."
    return
fi
export BASHRC_ALREADY_SOURCED=1

# Only proceed if the shell is interactive
case $- in
    *i*)
        echo "[EXE] .bashrc: Interactive shell detected, loading configurations..."
        ;;
    *)
        echo "[EXE] .bashrc: Non-interactive shell detected, exiting."
        return
        ;;
esac

##########

# Avoid duplicate entries in history.
echo -e "\033[1;34m[INFO]\033[0m Configuring HISTCONTROL and Setting history size..."
HISTCONTROL=ignoreboth
HISTSIZE=1000
HISTFILESIZE=2000
echo -e "\033[1;32m[SUCCESS]\033[0m HISTCONTROL and Setting history size configured."

# Make `less` more friendly for non-text input files.
echo -e "\033[1;34m[INFO]\033[0m Configuring 'lesspipe'..."
[ -x $(command -v lesspipe) ] && eval "$(SHELL=/bin/sh lesspipe)" || echo -e "\033[1;31m[WARN]\033[0m 'lesspipe' not found or not executable."
echo -e "\033[1;32m[SUCCESS]\033[0m lesspipe configured."

# Set a fancy prompt if the terminal supports color
echo -e "\033[1;34m[INFO]\033[0m Configuring prompt colors..."
case "$TERM" in
    xterm-color|*-256color) color_prompt=yes;;
esac

if [ -n "$force_color_prompt" ]; then
    if [ -x "$(command -v tput)" ] && tput setaf 1 >&/dev/null; then
        color_prompt=yes
    else
        color_prompt=
    fi
fi

if [ "$color_prompt" = yes ]; then
    PS1='\[\e]0;\u@\h: \w\a\]\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi

# Ensure no unintentional newlines are introduced
PS1="${PS1//\\n/}"

unset color_prompt force_color_prompt
echo -e "\033[1;32m[SUCCESS]\033[0m prompt colors configured."

# Set xterm title to user@host:dir
echo -e "\033[1;34m[INFO]\033[0m Configuring terminal title..."
case "$TERM" in
    xterm*|rxvt*)
        PS1="\[\e]0;${debian_chroot:+($debian_chroot)}\u@\h: \w\a\]$PS1"
        ;;
esac
echo -e "\033[1;32m[SUCCESS]\033[0m prompt colors configured."

# Enable color support for `ls` and add handy aliases if dircolors is available.
echo -e "\033[1;34m[INFO]\033[0m Checking for dircolors support..."
if [ -x $(command -v dircolors) ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    echo -e "\033[1;32m[SUCCESS]\033[0m dircolors configured."
else
    echo -e "\033[1;31m[WARN]\033[0m dircolors not found."
fi

# Unified alias section.
echo -e "\033[1;34m[INFO]\033[0m Loading aliases..."
# Aliases for `ls` command.
alias ls='ls --color=auto'
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'

# Aliases for `grep` command.
alias grep='grep --color=auto'
alias fgrep='fgrep --color=auto'
alias egrep='egrep --color=auto'

echo -e "\033[1;32m[SUCCESS]\033[0m prompt colors configured."

# FZF setup (if installed).
echo -e "\033[1;34m[INFO]\033[0m Checking for FZF setup..."
if command -v fzf > /dev/null; then
    echo -e "\033[1;34m[INFO]\033[0m Configuring FZF..."
    export FZF_DEFAULT_OPTS="--height 40% --layout=reverse --border"
    export FZF_CTRL_T_COMMAND='find . -type f'
    export FZF_ALT_C_COMMAND='find . -type d'
    bindkey '^R' fzf-history-widget || echo -e "\033[1;31m[ERROR]\033[0m Error configuring FZF history binding."
    echo -e "\033[1;32m[SUCCESS]\033[0m FZF setup configured."
else
    echo -e "\033[1;33m[WARN]\033[0m FZF not found."
fi

function extract() {
    echo -e "\033[1;34m[INFO]\033[0m Extracting file: $1..."
    if [ -f "$1" ]; then
        case "$1" in
            *.tar.bz2)   tar xjf "$1"   ;;
            *.tar.gz)    tar xzf "$1"   ;;
            *.bz2)       bunzip2 "$1"  ;;
            *.rar)       unrar x "$1"  ;;
            *.gz)        gunzip "$1"   ;;
            *.tar)       tar xf "$1"   ;;
            *.tbz2)      tar xjf "$1"  ;;
            *.tgz)       tar xzf "$1"  ;;
            *.zip)       unzip "$1"    ;;
            *.Z)         uncompress "$1";;
            *)           echo -e "\033[1;31m[ERROR]\033[0m Cannot extract $1: unsupported format." ;;
        esac
        echo -e "\033[1;32m[SUCCESS]\033[0m extract function configured."
    else
        echo -e "\033[1;31m[ERROR]\033[0m $1 is not a valid file."
    fi
}

# Conda initialization (if available).
echo -e "\033[1;34m[INFO]\033[0m Initializing Conda..."
__conda_setup="$('$HOME/miniconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
    echo -e "\033[1;32m[SUCCESS]\033[0m Conda initialized successfully."
else
    if [ -f "$HOME/miniconda3/etc/profile.d/conda.sh" ]; then
        . "$HOME/miniconda3/etc/profile.d/conda.sh" || echo -e "\033[1;31m[ERROR]\033[0m Error sourcing conda.sh."
        echo -e "\033[1;32m[SUCCESS]\033[0m Conda.sh sourced successfully."
    else
        echo -e "\033[1;31m[ERROR]\033[0m Conda setup not found. Please check your Miniconda installation."
        export PATH="$HOME/miniconda3/bin:$PATH"
    fi
fi
unset __conda_setup

# Message indicating the end of execution
echo -e "\033[1;32m[SUCCESS]\033[0m .bashrc execution completed."
```
