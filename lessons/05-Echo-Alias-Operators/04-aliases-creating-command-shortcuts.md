# 4. Aliases: Creating Command Shortcuts


### What Are Aliases?
Aliases are shortcuts that let you create custom commands or modify existing command behavior.

### Creating Aliases:

```bash
# Basic syntax
alias name='command'

# Simple aliases
alias ll='ls -la'
alias la='ls -la'
alias l='ls -CF'
alias h='history'
alias c='clear'

# Aliases with options
alias grep='grep --color=auto'
alias ls='ls --color=auto'
alias df='df -h'
alias du='du -h'

# Complex aliases
alias psg='ps aux | grep'
alias ports='netstat -tuln'
alias myip='curl ifconfig.me'
```

### Using Aliases:

```bash
# After creating alias ll='ls -la'
ll                    # Same as: ls -la
ll /etc              # Same as: ls -la /etc

# Temporary override alias
\ls                  # Run original ls command
/bin/ls              # Run original ls command
```

### Managing Aliases:

```bash
# List all aliases
alias

# List specific alias
alias ll

# Remove alias
unalias ll
unalias grep

# Remove all aliases
unalias -a
```

### Permanent Aliases:
Add aliases to your shell configuration file:

```bash
# Edit bash configuration
nano ~/.bashrc

# Add aliases
alias ll='ls -la'
alias grep='grep --color=auto'
alias update='sudo apt update && sudo apt upgrade'

# Reload configuration
source ~/.bashrc
```

### Practical Alias Examples:

```bash
# System administration
alias install='sudo apt install'
alias update='sudo apt update'
alias upgrade='sudo apt upgrade'
alias search='apt search'

# Navigation
alias ..='cd ..'
alias ...='cd ../..'
alias home='cd ~'
alias docs='cd ~/Documents'

# File operations
alias cp='cp -i'           # Interactive copy
alias mv='mv -i'           # Interactive move
alias rm='rm -i'           # Interactive remove
alias mkdir='mkdir -pv'    # Create parent dirs, verbose

# System monitoring
alias top='htop'
alias ps='ps aux'
alias mount='mount | column -t'
alias free='free -h'

# Git shortcuts (if you use git)
alias gs='git status'
alias ga='git add'
alias gc='git commit'
alias gp='git push'
alias gl='git log'
```
---

## Navigation

**Previous:** [← Command Types And Discovery](03-command-types-and-discovery.md)  
**Next:** [→ Control Operators](05-control-operators.md)  
**Lesson Home:** [↑ Lesson 05: Echo Alias Operators](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
