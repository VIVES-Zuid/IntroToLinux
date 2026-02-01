# 4. Variables in Shell Scripts


### Variable Declaration and Usage:

```bash
#!/bin/bash

# Variable assignment (no spaces around =)
NAME="Linux User"
AGE=25
PI=3.14159
TODAY=$(date)

# Using variables (with $ prefix)
echo "Hello, $NAME"
echo "You are $AGE years old"
echo "Today is $TODAY"

# Alternative syntax (recommended for complex cases)
echo "Hello, ${NAME}"
echo "File: ${NAME}.txt"
```

### Variable Types:

#### String Variables:
```bash
#!/bin/bash

# Simple strings
GREETING="Hello World"
FILE_NAME="document.txt"

# Strings with spaces (quotes required)
FULL_NAME="John Doe"
MESSAGE="Welcome to Linux scripting"

# Using variables in strings
USER="alice"
HOME_DIR="/home/$USER"
echo "User $USER lives in $HOME_DIR"
```

#### Numeric Variables:
```bash
#!/bin/bash

# Numbers (treated as strings unless used in arithmetic)
COUNT=10
PRICE=29.99

# Arithmetic operations
NUMBER1=5
NUMBER2=3
SUM=$((NUMBER1 + NUMBER2))
PRODUCT=$((NUMBER1 * NUMBER2))

echo "Sum: $SUM"
echo "Product: $PRODUCT"
```

#### Command Substitution:
```bash
#!/bin/bash

# Capture command output in variables
CURRENT_DATE=$(date)
USER_COUNT=$(who | wc -l)
DISK_USAGE=$(df -h | grep "/$" | awk '{print $5}')

echo "Today is: $CURRENT_DATE"
echo "Users logged in: $USER_COUNT"
echo "Root disk usage: $DISK_USAGE"
```

### Environment Variables:

```bash
#!/bin/bash

# Use existing environment variables
echo "Current user: $USER"
echo "Home directory: $HOME"
echo "Current path: $PWD"
echo "Shell: $SHELL"

# Set environment variables for child processes
export MY_VAR="Available to child processes"

# Local variables (not exported)
LOCAL_VAR="Only in this script"
```

### Variable Best Practices:

```bash
#!/bin/bash

# Use uppercase for constants
readonly PI=3.14159
readonly CONFIG_FILE="/etc/myapp.conf"

# Use lowercase for local variables
user_name="john"
temp_file="/tmp/processing.tmp"

# Use descriptive names
backup_directory="/backups"
log_file="/var/log/script.log"

# Quote variables to handle spaces
FILE_NAME="my document.txt"
cp "$FILE_NAME" backup/

# Default values
USERNAME=${1:-"guest"}              # Use $1 or "guest" if empty
CONFIG_DIR=${CONFIG_DIR:-"/etc"}    # Use env var or default
```
---

## Navigation

**Next:** [→ User Input And Command Line Arguments](05-user-input-and-command-line-arguments.md)  
**Previous:** [← Comments And Documentation](03-comments-and-documentation.md)  
**Lesson Home:** [↑ Lesson 8: Scripting](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
