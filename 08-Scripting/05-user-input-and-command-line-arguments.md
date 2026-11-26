# 5. User Input and Command-Line Arguments

A script can get information in two ways:

| Way | When | Example |
|-----|------|---------|
| `read` | the script **asks** a question while it runs | `What's your name? _` |
| arguments | you give the values **when you start** the script | `./greet.sh Alice` |

### Reading User Input: `read`

```bash
#!/bin/bash

# Ask a question, store the answer in the variable "name"
echo "What's your name?"
read name
echo "Hello, $name!"

# -p = prompt: question and answer on the same line
read -p "Enter your age: " age
echo "You are $age years old"
```

```
 $ ./ask.sh
 What's your name?
 Alice                  ◀── typed by the user, stored in $name
 Hello, Alice!
 Enter your age: 20     ◀── stored in $age
 You are 20 years old
```

> ℹ️ **Advanced** – `read -s -p "Password: " pw` hides what you type (handy for passwords). Print an empty `echo` afterwards, because the Enter key is hidden too.

### Command-Line Arguments: `$1`, `$2`, ...

The words you type **after** the script name are stored in special variables:

```
 $ ./greet.sh   Alice   25   Engineer
     ───┬────   ──┬──   ┬─   ───┬────
        │         │     │       │
       $0        $1    $2      $3

 $#  = 3                       (number of arguments)
 $@  = Alice 25 Engineer       (all arguments)
```

| Variable | Meaning |
|----------|---------|
| `$0` | name of the script |
| `$1`, `$2`, ... | first, second, ... argument |
| `$#` | **number** of arguments |
| `$@` | **all** arguments |

```bash
#!/bin/bash
# Script: greet.sh
# Usage:  ./greet.sh <name> <age>

echo "Script name: $0"
echo "Hello $1, you are $2 years old"
echo "You gave $# arguments: $@"
```

```
 $ ./greet.sh Alice 25
 Script name: ./greet.sh
 Hello Alice, you are 25 years old
 You gave 2 arguments: Alice 25
```

### Checking the Arguments

A good script checks whether it got enough arguments and explains how to use it if not:

```bash
#!/bin/bash
# Script: copy-file.sh
# Usage:  ./copy-file.sh <source> <destination>

if [ $# -lt 2 ]; then
    echo "Usage: $0 <source> <destination>"
    exit 1
fi

source_file="$1"
destination="$2"

cp "$source_file" "$destination"
echo "Copied $source_file to $destination"
```

> 💡 Give the arguments a **meaningful name** (`source_file="$1"`) at the top of the script. The rest of the script is much easier to read than with `$1` and `$2` everywhere.
>
> `if` and `exit 1` are explained in the next section.

---

## Navigation

**Next:** [→ Conditional Statements](06-conditional-statements.md)  
**Previous:** [← Variables In Shell Scripts](04-variables-in-shell-scripts.md)  
**Lesson Home:** [↑ Lesson 8: Shell Scripting Fundamentals](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
