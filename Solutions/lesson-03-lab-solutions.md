# Solutions for Lesson 3: Shell Environment and Variables

## Lab 3.1: Exploring Environment Variables (10 minutes)

### Solution:

```bash
# 1. Display your current environment
env | head -20
# Expected output: List of environment variables like:
# HOME=/home/username
# USER=username
# SHELL=/bin/bash
# PATH=/usr/local/bin:/usr/bin:/bin

# Alternative command:
printenv | head -20

# 2. Check specific important variables
echo "Home: $HOME"          # Should show: Home: /home/username
echo "User: $USER"          # Should show: User: username
echo "Shell: $SHELL"        # Should show: Shell: /bin/bash
echo "Path: $PATH"          # Should show: Path: /usr/local/bin:/usr/bin:/bin:...

# 3. Create custom variables
MY_NAME="Your Name"         # Replace with actual name
MY_COURSE="Linux Introduction"
echo "Student: $MY_NAME"    # Should show: Student: Your Name
echo "Course: $MY_COURSE"   # Should show: Course: Linux Introduction

# Test variable access:
echo $MY_NAME               # Works in current shell
echo "$MY_NAME"             # Safer - handles spaces
echo '$MY_NAME'             # Literal - shows: $MY_NAME

# 4. Export a variable and verify
export MY_COURSE
env | grep MY_COURSE        # Should show: MY_COURSE=Linux Introduction

# Test the difference:
# Before export: variable only in current shell
# After export: variable available to child processes
```

## Lab 3.2: Command History Practice (10 minutes)

### Solution:

```bash
# 1. Run several commands
ls                          # List current directory
pwd                         # Show current path
whoami                      # Show current username
date                        # Show current date/time
uname -a                    # Show system information

# 2. View history
history                     # Show all command history
history 10                  # Show last 10 commands
history | tail -10          # Alternative way to show last 10

# 3. Practice history shortcuts
!!                          # Repeat last command (should repeat 'uname -a')
!ls                         # Repeat last ls command
!?pwd                       # Search for command containing 'pwd'
history | tail -5           # Show last 5 commands

# Additional history commands:
!5                          # Execute command number 5 from history
!-2                         # Execute command 2 positions back
^old^new                    # Replace 'old' with 'new' in last command

# 4. Practice interactive search
# Instructions for interactive search:
# Press Ctrl+R
# Type 'ls' (or any command to search for)
# Press Enter to execute, or Ctrl+R again for next match
# Press Esc to cancel search

# History navigation with arrow keys:
# Up arrow: Previous command
# Down arrow: Next command
```

## Lab 3.3: PATH Exploration (10 minutes)

### Solution:

```bash
# 1. Examine your PATH
echo $PATH
# Should show something like: /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin

# Make PATH readable:
echo $PATH | tr ':' '\n'
# Should show:
# /usr/local/bin
# /usr/bin
# /bin
# /usr/sbin
# /sbin

# 2. Find command locations
which ls                    # Should show: /bin/ls or /usr/bin/ls
which cat                   # Should show: /bin/cat or /usr/bin/cat
which bash                  # Should show: /bin/bash
type ls                     # Shows: ls is /bin/ls
type cd                     # Shows: cd is a shell builtin

# Additional location commands:
whereis ls                  # Shows multiple locations
command -v ls               # Portable way to find commands

# 3. Create a simple script
mkdir ~/mybin               # Create personal bin directory
cd ~/mybin                  # Navigate to it

# Create the script:
echo '#!/bin/bash' > hello.sh
echo 'echo "Hello from my script!"' >> hello.sh

# Make it executable:
chmod +x hello.sh

# Verify script creation:
ls -la hello.sh
cat hello.sh

# 4. Try running script
./hello.sh                  # Works with explicit path - Should show: Hello from my script!
hello.sh                    # Fails - not in PATH
# Expected error: hello.sh: command not found

# 5. Add to PATH and test
export PATH="$HOME/mybin:$PATH"

# Verify PATH update:
echo $PATH | tr ':' '\n' | head -5

# Now test the script:
hello.sh                    # Now it works! - Should show: Hello from my script!

# Verify it works from any directory:
cd /tmp
hello.sh                    # Should still work
cd ~                        # Return home
```

## Advanced Environment Variable Exercises:

```bash
# Temporary vs Permanent Variables:
MY_TEMP="Temporary"         # Only in current session
export MY_EXPORTED="Exported"  # Available to child processes

# Test in subshell:
bash -c 'echo "Temp: $MY_TEMP, Exported: $MY_EXPORTED"'
# Should show: Temp: , Exported: Exported

# Setting default values:
echo ${UNDEFINED_VAR:-"default value"}  # Shows: default value
echo ${USER:-"unknown"}                 # Shows your username

# Making PATH changes permanent:
echo 'export PATH="$HOME/mybin:$PATH"' >> ~/.bashrc
# Note: This will be active in new terminal sessions
```

## Troubleshooting Common Issues:

```bash
# Issue 1: Variable not found
echo $NONEXISTENT           # Shows nothing (empty)
echo "${NONEXISTENT:-default}"  # Shows: default

# Issue 2: Space in variable value
WRONG=Hello World           # Error: Word is a command
CORRECT="Hello World"       # Correct way
echo "$CORRECT"             # Shows: Hello World

# Issue 3: Command not found after PATH change
which hello.sh              # Should show: /home/username/mybin/hello.sh
echo $PATH | grep mybin     # Verify mybin is in PATH

# Issue 4: Script not executable
ls -la hello.sh             # Check if executable (x permission)
chmod +x hello.sh           # Make executable if needed

# Issue 5: PATH changes not persistent
echo $PATH                  # Check current session
# Add to ~/.bashrc for permanent changes
```

## Verification Commands:

```bash
# Check environment setup:
echo "=== Environment Variables ==="
echo "HOME: $HOME"
echo "USER: $USER"
echo "SHELL: $SHELL"
echo "MY_COURSE: $MY_COURSE"

echo "=== PATH Configuration ==="
echo $PATH | tr ':' '\n' | head -5

echo "=== Custom Script ==="
which hello.sh
hello.sh

echo "=== History ==="
history | tail -5
```

## Key Learning Points:

1. **Environment Variables**: Store system and user configuration
2. **Export**: Makes variables available to child processes
3. **PATH**: Determines where shell looks for commands
4. **History**: Provides command recall and editing capabilities
5. **Quotes**: Protect variables with spaces or special characters

---

## Navigation

**Previous:** [← Lesson 2: The Linux Console and Terminal](lesson-02-exercises-solutions.md)  
**Next:** [→ Lesson 4: I/O Redirection and Pipes](lesson-04-lab-solutions.md)  
**Home:** [↑ Solutions Index](README.md)