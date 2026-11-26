# 4. Variables in Shell Scripts

A variable is a **name** that holds a **value**. You already used variables in lesson 3; in a script they work the same way.

### Creating and Using a Variable

```
  name="Linux User"          echo "Hello, $name"
  ──┬─ ┬ ─────┬─────                     ──┬──
    │  │      └── value                    └── $ = "give me the value of"
    │  └── NO spaces around =
    └── variable name
```

```bash
#!/bin/bash

name="Linux User"
age=25

echo "Hello, $name"
echo "You are $age years old"
echo "File: ${name}.txt"      # { } shows where the name ends
```

> ⚠️ **No spaces around `=`!**
> ```bash
> name="Alice"      # ✅ correct
> name = "Alice"    # ❌ bash tries to run a command called "name"
> ```

### Double Quotes vs Single Quotes

```bash
name="Alice"
echo "Hello $name"     # Hello Alice    → " " : variables are filled in
echo 'Hello $name'     # Hello $name    → ' ' : everything literally
```

> 💡 Always put variables between **double quotes** when you use them: `cp "$file" backup/`. Without quotes, a filename with a space (`my file.txt`) becomes two separate words.

### Command Substitution: `$( )`

Put the **output** of a command in a variable:

```bash
#!/bin/bash

today=$(date +%F)
user_count=$(who | wc -l)
my_shell=$(grep "^$USER:" /etc/passwd | cut -d: -f7)

echo "Today is: $today"
echo "Users logged in: $user_count"
echo "My login shell: $my_shell"
```

### Arithmetic: `$(( ))`

```bash
#!/bin/bash

a=5
b=3
echo "Sum: $((a + b))"          # 8
echo "Product: $((a * b))"      # 15
echo "Division: $((10 / 3))"    # 3  ← only whole numbers!

counter=1
counter=$((counter + 1))        # counter is now 2
```

> ⚠️ Bash can only calculate with **whole numbers** (integers). `$((10 / 3))` gives `3`, and `$((2.5 + 1))` gives an error.

### Environment Variables

Some variables already exist when your script starts:

| Variable | Contains |
|----------|----------|
| `$USER` | your username |
| `$HOME` | your home directory |
| `$PWD` | the current directory |
| `$SHELL` | your login shell |
| `$PATH` | where bash looks for commands |

```bash
echo "Current user: $USER"
echo "Home directory: $HOME"
```

> ⚠️ Don't use these names for your own variables. `USER="alice"` in a script would overwrite the real value. Use **lowercase** names for your own variables (`user_name`, `backup_dir`) to avoid that.

### ℹ️ Advanced: readonly, export and Default Values

> ℹ️ **Advanced** – optional on a first read.

```bash
# A constant that cannot be changed
readonly CONFIG_FILE="/etc/myapp.conf"

# Make a variable available to programs started by this script
export MY_VAR="visible for child processes"

# Default value: use $1, or "guest" when $1 is empty
username=${1:-guest}
```

---

## Navigation

**Next:** [→ User Input And Command Line Arguments](05-user-input-and-command-line-arguments.md)  
**Previous:** [← Comments And Documentation](03-comments-and-documentation.md)  
**Lesson Home:** [↑ Lesson 8: Shell Scripting Fundamentals](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
