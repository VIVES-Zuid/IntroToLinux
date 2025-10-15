# 1. Working with Whitespace and Quotes


### Whitespace in Commands

Commands are separated by whitespace, but sometimes you need literal spaces in arguments:

```bash
# Problem: This creates multiple files
touch my file.txt
# Creates: "my" and "file.txt"

# Solutions:
touch "my file.txt"        # Double quotes
touch 'my file.txt'        # Single quotes  
touch my\ file.txt         # Backslash escape
touch "my file.txt"        # Preferred method
```

### Single Quotes vs Double Quotes

#### Single Quotes ('): Literal String
- Everything inside is treated literally
- No variable expansion
- No command substitution
- Can't contain single quotes

```bash
NAME="Linux"
echo 'Hello $NAME'         # Output: Hello $NAME
echo 'The cost is $5'      # Output: The cost is $5
echo 'Today is $(date)'    # Output: Today is $(date)
```

#### Double Quotes ("): Interpreted String
- Variable expansion occurs
- Command substitution occurs  
- Escape characters work
- Can contain single quotes

```bash
NAME="Linux"
echo "Hello $NAME"         # Output: Hello Linux
echo "The cost is \$5"     # Output: The cost is $5
echo "Today is $(date)"    # Output: Today is Mon Jan 1 12:00:00 UTC 2024
```

### Practical Examples:

```bash
# Working with filenames containing spaces
ls "Program Files"
cd "My Documents"
cp "old file.txt" "new file.txt"

# Combining quotes
echo "User's home directory: $HOME"
echo 'Use "double quotes" for variables'

# Mixed content
MESSAGE="Hello"
echo "$MESSAGE 'World' from Linux"
```
---

## Navigation

**Previous:** [← Learning Objectives](00-learning-objectives.md)  
**Next:** [→ Special Characters And Echo](02-special-characters-and-echo.md)  
**Lesson Home:** [↑ Lesson 05: Echo Alias Operators](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
