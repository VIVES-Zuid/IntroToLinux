# 3. Comments and Documentation

Everything after a `#` is a **comment**: bash ignores it. Comments explain **why** the script does something, for your colleagues and for yourself in 3 months.

```bash
#!/bin/bash

# This is a single-line comment

# A longer explanation is just
# several comment lines
# after each other

echo "Code here"   # a comment at the end of a line

# Disable a command temporarily by putting # in front:
# echo "This won't run"
```

> ⚠️ The shebang `#!/bin/bash` on line 1 is the only line starting with `#` that is **not** a comment.

### A Simple Script Header

Start every script with a short header: what does it do and how do you use it?

```bash
#!/bin/bash
#
# Script:  backup-home.sh
# Purpose: Copy my documents to a backup folder
# Author:  Your Name
# Usage:   ./backup-home.sh
#

echo "Starting backup at $(date)"
```

> ℹ️ **Advanced** – You may also see `: '...'` used for a multi-line comment block. It works, but simply using `#` on every line is clearer.

---

## Navigation

**Next:** [→ Variables In Shell Scripts](04-variables-in-shell-scripts.md)  
**Previous:** [← Creating Your First Script](02-creating-your-first-script.md)  
**Lesson Home:** [↑ Lesson 8: Shell Scripting Fundamentals](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
