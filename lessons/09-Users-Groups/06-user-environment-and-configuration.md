# 6. User Environment and Configuration


### User's Startup Files:

#### Login Shells:
```bash
/etc/profile           # System-wide profile
~/.bash_profile        # User's bash profile
~/.bash_login          # Alternative login script
~/.profile             # Shell-agnostic profile
```

#### Interactive Non-Login Shells:
```bash
/etc/bash.bashrc       # System-wide bashrc
~/.bashrc              # User's bash configuration
```

### Common User Configuration Files:

```bash
~/.bashrc              # Bash configuration
~/.bash_history        # Command history
~/.bash_logout         # Logout script
~/.vimrc               # Vim editor configuration
~/.ssh/                # SSH keys and configuration
~/.gitconfig           # Git configuration
```

### Setting Up New User Environment:

```bash
#!/bin/bash
# Script: setup-user-environment.sh

USERNAME="$1"
if [ -z "$USERNAME" ]; then
    echo "Usage: $0 <username>"
    exit 1
fi

USER_HOME="/home/$USERNAME"

# Create user
sudo useradd -m -s /bin/bash "$USERNAME"
sudo passwd "$USERNAME"

# Set up basic configuration
sudo -u "$USERNAME" bash << EOF
# Create basic .bashrc
cat > "$USER_HOME/.bashrc" << 'BASHRC'
# Basic bash configuration
alias ll='ls -la'
alias la='ls -la'
alias l='ls -CF'
alias grep='grep --color=auto'

export EDITOR=nano
export HISTSIZE=1000
export HISTFILESIZE=2000

# Custom prompt
PS1='\u@\h:\w\$ '
BASHRC

# Create useful directories
mkdir -p "$USER_HOME/bin"
mkdir -p "$USER_HOME/scripts"
mkdir -p "$USER_HOME/tmp"

echo "User environment setup completed for $USERNAME"
EOF
```
---

## Navigation

**Previous:** [← The Sudo System](05-the-sudo-system.md)  
**Next:** [→ Practical User Management Scenarios](07-practical-user-management-scenarios.md)  
**Lesson Home:** [↑ Lesson 09: Users Groups](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
