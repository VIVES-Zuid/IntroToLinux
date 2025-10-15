# 6. Shell Configuration Files


### Types of Configuration Files:

#### System-wide:
- `/etc/bash.bashrc` - System-wide bash configuration
- `/etc/profile` - System-wide environment variables

#### User-specific:
- `~/.bashrc` - Bash configuration for interactive non-login shells
- `~/.bash_profile` - Bash configuration for login shells
- `~/.profile` - Shell-agnostic profile
- `~/.bash_history` - Command history file

### Editing Configuration:

```bash
# Edit your bash configuration
nano ~/.bashrc

# Reload configuration without restarting shell
source ~/.bashrc
# or
. ~/.bashrc
```

### Common Configuration Examples:

```bash
# Add to ~/.bashrc

# Custom aliases
alias ll='ls -la'
alias la='ls -la'
alias grep='grep --color=auto'

# Custom environment variables
export EDITOR=nano
export BROWSER=firefox

# Add custom directory to PATH
export PATH="$HOME/bin:$PATH"

# Custom prompt
export PS1='\u@\h:\w\$ '
```
---

## Navigation

**Previous:** [← Working With The Path Variable](05-working-with-the-path-variable.md)  
**Next:** [→ Practical Lab Environment Setup](07-practical-lab-environment-setup.md)  
**Lesson Home:** [↑ Lesson 03: History Variables](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
