# 5. Working with the PATH Variable


### Understanding PATH:
The PATH variable tells the shell where to look for executable commands. When you type a command, the shell searches these directories in order.

```bash
# View current PATH
echo $PATH

# See which directories are searched
echo $PATH | tr ':' '\n'
```

### How Command Resolution Works:
1. Shell checks if command is a built-in (like `cd`)
2. Shell checks each directory in PATH from left to right
3. First match is executed
4. If not found, "command not found" error

### Finding Command Locations:

```bash
# Find where a command is located
which command_name
which ls
which python

# Show all locations of a command
type command_name
type ls
type cd    # Shows it's a shell builtin

# More detailed information
type -a command_name
```

### Adding to PATH:
To add a directory to PATH (temporary - only for current session):

```bash
# Add directory to beginning of PATH
export PATH="/new/directory:$PATH"

# Add directory to end of PATH
export PATH="$PATH:/new/directory"

# Example: Add current directory to PATH
export PATH=".:$PATH"
```

### Making PATH Changes Permanent:
Add the export command to your shell configuration file:
- For bash: `~/.bashrc` or `~/.bash_profile`
- For zsh: `~/.zshrc`
---

## Navigation

**Next:** [→ Shell Configuration Files](06-shell-configuration-files.md)  
**Previous:** [← Command Line Editing](04-command-line-editing.md)  
**Lesson Home:** [↑ Lesson 3: History & Variables](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
