# 2. Creating Your First Script


### Script File Structure:

```bash
#!/bin/bash
# This is a comment
# Script: hello-world.sh
# Purpose: Demonstrate basic script structure

echo "Hello, World!"
echo "This is my first shell script"
```

### The Shebang Line:
- `#!/bin/bash` - Tells system which interpreter to use
- Must be the first line of the script
- Common shebangs:
  - `#!/bin/bash` - Bash shell
  - `#!/bin/sh` - POSIX shell (more portable)
  - `#!/usr/bin/env bash` - Find bash in PATH

### Making Scripts Executable:

```bash
# Create script file
nano hello-world.sh

# Make executable
chmod +x hello-world.sh

# Run script
./hello-world.sh

# Or run without making executable
bash hello-world.sh
```

### Script Location and PATH:

```bash
# Run from current directory
./script.sh

# Make available system-wide
sudo cp script.sh /usr/local/bin/
sudo chmod +x /usr/local/bin/script.sh

# Now can run from anywhere
script.sh
```
---

## Navigation

**Previous:** [← Introduction To Shell Scripting](01-introduction-to-shell-scripting.md)  
**Next:** [→ Comments And Documentation](03-comments-and-documentation.md)  
**Lesson Home:** [↑ Lesson 08: Scripting](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
