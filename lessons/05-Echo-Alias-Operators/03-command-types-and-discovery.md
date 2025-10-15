# 3. Command Types and Discovery


### Types of Commands:

#### 1. Built-in Commands
- Part of the shell itself
- Examples: `cd`, `echo`, `pwd`, `type`

#### 2. External Commands  
- Separate executable files
- Found in PATH directories
- Examples: `ls`, `cat`, `grep`

#### 3. Aliases
- Custom shortcuts you create
- Examples: `ll` (often aliased to `ls -la`)

#### 4. Functions
- Custom shell functions
- More complex than aliases

### Command Discovery:

```bash
# Check command type
type command_name
type cd          # cd is a shell builtin
type ls          # ls is /bin/ls
type ll          # ll is aliased to 'ls -l'

# Find command location
which command_name
which ls         # /bin/ls
which python3    # /usr/bin/python3

# Show all possible commands
type -a command_name
type -a echo     # Shows builtin and external versions

# Check if command exists
command -v command_name
if command -v git > /dev/null; then
    echo "Git is installed"
fi
```

### Practical Examples:

```bash
# Explore your system
type bash
type cd
type ls
which ls
which grep
which python3

# Check for optional software
command -v docker && echo "Docker available"
command -v git && echo "Git available"
command -v python3 && echo "Python 3 available"
```
---

## Navigation

**Previous:** [← Special Characters And Echo](02-special-characters-and-echo.md)  
**Next:** [→ Aliases Creating Command Shortcuts](04-aliases-creating-command-shortcuts.md)  
**Lesson Home:** [↑ Lesson 05: Echo Alias Operators](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
