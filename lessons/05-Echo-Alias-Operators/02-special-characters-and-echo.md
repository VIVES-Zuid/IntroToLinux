# 2. Special Characters and Echo


### Echo Command with Special Characters:

```bash
# Basic echo
echo "Hello World"

# Escape sequences (with -e option)
echo -e "Line 1\nLine 2"       # Newline
echo -e "Tab\there"            # Tab
echo -e "Bell\a"               # Bell sound
echo -e "Backspace\b"          # Backspace

# Without interpretation
echo "Literal\ntext"           # Output: Literal\ntext

# Suppress newline
echo -n "No newline"
```

### Special Characters in Variables:

```bash
# Dollar sign
PRICE="\$10"
echo "Price: $PRICE"

# Backslash
PATH_WIN="C:\\Users\\Admin"
echo "Windows path: $PATH_WIN"

# Mixed quotes
QUOTE='"Hello World"'
echo "He said: $QUOTE"
```

### Echo with Colors:

```bash
# ANSI color codes
echo -e "\033[31mRed text\033[0m"      # Red
echo -e "\033[32mGreen text\033[0m"    # Green  
echo -e "\033[33mYellow text\033[0m"   # Yellow
echo -e "\033[34mBlue text\033[0m"     # Blue

# Using variables for colors
RED='\033[31m'
GREEN='\033[32m'
RESET='\033[0m'

echo -e "${RED}Error message${RESET}"
echo -e "${GREEN}Success message${RESET}"
```
---

## Navigation

**Previous:** [← Working With Whitespace And Quotes](01-working-with-whitespace-and-quotes.md)  
**Next:** [→ Command Types And Discovery](03-command-types-and-discovery.md)  
**Lesson Home:** [↑ Lesson 05: Echo Alias Operators](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
