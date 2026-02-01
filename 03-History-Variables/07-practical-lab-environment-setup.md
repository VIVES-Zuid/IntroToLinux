# 7. Practical Lab: Environment Setup


### Lab 3.1: Exploring Environment Variables (10 minutes)

```bash
# 1. Display your current environment
env | head -20

# 2. Check specific important variables
echo "Home: $HOME"
echo "User: $USER"  
echo "Shell: $SHELL"
echo "Path: $PATH"

# 3. Create custom variables
MY_NAME="Your Name"
MY_COURSE="Linux Introduction"
echo "Student: $MY_NAME"
echo "Course: $MY_COURSE"

# 4. Export a variable and verify
export MY_COURSE
env | grep MY_COURSE
```

### Lab 3.2: Command History Practice (10 minutes)

```bash
# 1. Run several commands
ls
pwd
whoami
date
uname -a

# 2. View history
history

# 3. Practice history shortcuts
!!              # Repeat last command
!ls             # Repeat last ls command
history | tail -5  # Show last 5 commands

# 4. Practice interactive search
# Press Ctrl+R and type 'ls' to search
```

### Lab 3.3: PATH Exploration (10 minutes)

```bash
# 1. Examine your PATH
echo $PATH
echo $PATH | tr ':' '\n'

# 2. Find command locations
which ls
which cat
which bash
type ls
type cd

# 3. Create a simple script
mkdir ~/mybin
cd ~/mybin
echo '#!/bin/bash' > hello.sh
echo 'echo "Hello from my script!"' >> hello.sh
chmod +x hello.sh

# 4. Try running script
./hello.sh      # Works with explicit path
hello.sh        # Fails - not in PATH

# 5. Add to PATH and test
export PATH="$HOME/mybin:$PATH"
hello.sh        # Now it works!
```
---

## Navigation

**Next:** [→ Advanced Environment Concepts](08-advanced-environment-concepts.md)  
**Previous:** [← Shell Configuration Files](06-shell-configuration-files.md)  
**Lesson Home:** [↑ Lesson 3: History & Variables](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
