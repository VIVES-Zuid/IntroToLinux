# 6. Conditional Statements


### if-then-else Structure:

```bash
#!/bin/bash

# Basic if statement
if [ condition ]; then
    # commands to execute if condition is true
    echo "Condition is true"
fi

# if-else statement
if [ condition ]; then
    echo "Condition is true"
else
    echo "Condition is false"
fi

# if-elif-else statement
if [ condition1 ]; then
    echo "First condition is true"
elif [ condition2 ]; then
    echo "Second condition is true"
else
    echo "No conditions are true"
fi
```

### Test Conditions:

#### File Tests:
```bash
#!/bin/bash

FILE="/etc/passwd"

if [ -f "$FILE" ]; then
    echo "$FILE is a regular file"
fi

if [ -d "/home" ]; then
    echo "/home is a directory"
fi

if [ -r "$FILE" ]; then
    echo "$FILE is readable"
fi

if [ -w "$FILE" ]; then
    echo "$FILE is writable"
fi

if [ -x "/bin/ls" ]; then
    echo "/bin/ls is executable"
fi

# Other file tests:
# -e : exists
# -s : not empty
# -L : symbolic link
```

#### String Comparisons:
```bash
#!/bin/bash

read -p "Enter your name: " name

if [ "$name" = "admin" ]; then
    echo "Welcome, administrator!"
elif [ "$name" != "" ]; then
    echo "Hello, $name"
else
    echo "No name provided"
fi

# String tests:
# -z string : string is empty
# -n string : string is not empty
# string1 = string2 : strings are equal
# string1 != string2 : strings are not equal
```

#### Numeric Comparisons:
```bash
#!/bin/bash

read -p "Enter your age: " age

if [ "$age" -ge 18 ]; then
    echo "You are an adult"
elif [ "$age" -ge 13 ]; then
    echo "You are a teenager"
else
    echo "You are a child"
fi

# Numeric tests:
# -eq : equal
# -ne : not equal
# -gt : greater than
# -ge : greater than or equal
# -lt : less than
# -le : less than or equal
```

### Logical Operators:

```bash
#!/bin/bash

# AND operator
if [ "$age" -ge 18 ] && [ "$age" -le 65 ]; then
    echo "Working age"
fi

# OR operator
if [ "$name" = "admin" ] || [ "$name" = "root" ]; then
    echo "System administrator"
fi

# NOT operator
if [ ! -f "/etc/shadow" ]; then
    echo "Shadow file not found"
fi

# Complex conditions
if [ "$USER" = "root" ] && [ -w "/etc/passwd" ]; then
    echo "Root user with write access"
fi
```

### Practical Conditional Examples:

```bash
#!/bin/bash
# Script: backup-check.sh

BACKUP_DIR="/backups"
SOURCE_DIR="/home/user"

# Check if backup directory exists
if [ ! -d "$BACKUP_DIR" ]; then
    echo "Creating backup directory: $BACKUP_DIR"
    mkdir -p "$BACKUP_DIR"
fi

# Check if source directory exists
if [ ! -d "$SOURCE_DIR" ]; then
    echo "Error: Source directory $SOURCE_DIR does not exist"
    exit 1
fi

# Check available disk space
AVAILABLE=$(df "$BACKUP_DIR" | tail -1 | awk '{print $4}')
if [ "$AVAILABLE" -lt 1000000 ]; then  # Less than 1GB
    echo "Warning: Low disk space in backup directory"
    echo "Available: ${AVAILABLE}KB"
fi

echo "Backup checks completed successfully"
```
---

## Navigation

**Previous:** [← User Input And Command Line Arguments](05-user-input-and-command-line-arguments.md)  
**Next:** [→ Loops](07-loops.md)  
**Lesson Home:** [↑ Lesson 08: Scripting](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
