# 3. Comments and Documentation


### Comment Types:

```bash
#!/bin/bash

# This is a single-line comment

# This is a multi-line comment
# spanning several lines
# to explain complex logic

echo "Code here"  # Inline comment

# Disable code temporarily
# echo "This won't run"

: '
This is a multi-line comment block
using the colon command and single quotes.
Everything between the quotes is ignored.
Useful for longer explanations.
'
```

### Documentation Best Practices:

```bash
#!/bin/bash
#
# Script Name: backup-system.sh
# Description: Creates incremental backups of important directories
# Author: Your Name
# Date: 2024-01-15
# Version: 1.0
#
# Usage: backup-system.sh [source_dir] [backup_dir]
#
# Examples:
#   backup-system.sh /home/user /backups/user
#   backup-system.sh /etc /backups/config
#

# Configuration variables
SOURCE_DIR="/home"
BACKUP_DIR="/backups"
DATE=$(date +%Y%m%d_%H%M%S)

# Main backup logic starts here
echo "Starting backup at $(date)"
```
---

## Navigation

**Previous:** [← Creating Your First Script](02-creating-your-first-script.md)  
**Next:** [→ Variables In Shell Scripts](04-variables-in-shell-scripts.md)  
**Lesson Home:** [↑ Lesson 08: Scripting](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
