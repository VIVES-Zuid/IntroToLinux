# 5. User Input and Command-Line Arguments


### Reading User Input:

```bash
#!/bin/bash

# Simple input
echo "What's your name?"
read name
echo "Hello, $name!"

# Input with prompt
read -p "Enter your age: " age
echo "You are $age years old"

# Silent input (for passwords)
read -s -p "Enter password: " password
echo  # New line after silent input
echo "Password entered"

# Multiple inputs
read -p "Enter first and last name: " first_name last_name
echo "Hello, $first_name $last_name"
```

### Command-Line Arguments:

```bash
#!/bin/bash
# Script: greet.sh

# Special variables for arguments
echo "Script name: $0"
echo "First argument: $1"
echo "Second argument: $2"
echo "Third argument: $3"
echo "All arguments: $@"
echo "Number of arguments: $#"

# Usage example: ./greet.sh John 25 Engineer
```

### Processing Arguments:

```bash
#!/bin/bash
# Script: file-manager.sh

# Check if enough arguments provided
if [ $# -lt 2 ]; then
    echo "Usage: $0 <source_file> <destination>"
    exit 1
fi

# Assign arguments to meaningful names
SOURCE_FILE="$1"
DESTINATION="$2"
OPERATION="${3:-copy}"  # Default to 'copy' if not provided

echo "Operation: $OPERATION"
echo "Source: $SOURCE_FILE"
echo "Destination: $DESTINATION"

# Perform operation based on arguments
case $OPERATION in
    "copy")
        cp "$SOURCE_FILE" "$DESTINATION"
        echo "File copied successfully"
        ;;
    "move")
        mv "$SOURCE_FILE" "$DESTINATION"
        echo "File moved successfully"
        ;;
    *)
        echo "Unknown operation: $OPERATION"
        exit 1
        ;;
esac
```
---

## Navigation

**Next:** [→ Conditional Statements](06-conditional-statements.md)  
**Previous:** [← Variables In Shell Scripts](04-variables-in-shell-scripts.md)  
**Lesson Home:** [↑ Lesson 8: Scripting](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
