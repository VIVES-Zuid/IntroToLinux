# 5. Alias and Scripting

---

## 5.1 `alias` — Creating Command Shortcuts

An alias defines a shorthand for a longer command.

```bash
alias ll='ls -lh'             # define alias
alias gs='git status'
alias myip='curl ifconfig.me'

alias                         # list all current aliases
unalias ll                    # remove an alias
```

> ⚠️ Aliases defined at the command line are **temporary** (lost when the shell closes).  
> To make them **permanent**, add them to `~/.bashrc` and run `source ~/.bashrc`.

### Exercises

**5.1** Create a temporary alias `lsize` that runs `ls -lhS` (list with human-readable sizes, sorted by size).

**5.2** Create a permanent alias `mygrep` that runs `grep -i --color`. Explain the two steps needed.

---

## 5.2 Script Basics

Every bash script starts with a **shebang** line:

```bash
#!/bin/bash
```

Make a script executable and run it:

```bash
chmod +x script.sh
./script.sh
```

### Variables and User Input

```bash
name="Alice"                    # assign (no spaces around =)
echo $name                      # use variable
echo "Hello, ${name}!"          # braces for clarity

read name                       # read from stdin into variable
read -p "Enter name: " name     # with prompt
```

### Script Parameters

| Variable | Meaning |
|----------|---------|
| `$1`, `$2`, ... | Positional arguments |
| `$#` | Number of arguments passed |
| `$0` | Name of the script |
| `$@` | All arguments as separate words |

```bash
echo "Script name: $0"
echo "First argument: $1"
echo "Total arguments: $#"
```

### Exercises

**5.3** Write a script `hello.sh` that:
- If called with one argument, prints `Hello, <argument>!`
- If called with no arguments, prints `Hello, World!`
- If called with more than one argument, prints an error and exits with code 1.

---

## 5.3 Conditional Statements (`if`)

```bash
if [[ condition ]]; then
    # commands
elif [[ condition ]]; then
    # commands
else
    # commands
fi
```

### Common test conditions

| Condition | Meaning |
|-----------|---------|
| `$# -eq 0` | No arguments |
| `$# -eq 1` | Exactly 1 argument |
| `$# -gt 1` | More than 1 argument |
| `"$1" == "-l"` | First argument is `-l` |
| `-f "$1"` | `$1` is an existing file |
| `-d "$1"` | `$1` is an existing directory |
| `-z "$var"` | Variable is empty |

```bash
if [[ $# -eq 0 ]]; then
    echo "No arguments given."
    exit 1
fi

if [[ "$1" == "-v" ]]; then
    echo "Verbose mode"
fi
```

### Exercises

**5.4** Write a script `check_args.sh` that:
- Exits with an error message if no arguments are given
- Exits with an error message if more than one argument is given
- Otherwise prints: `Argument received: <value>`

---

## 5.4 `case` Statements

`case` is cleaner than a chain of `elif` when matching against a fixed set of values.

```bash
case "$variable" in
    value1)
        # commands
        ;;
    value2)
        # commands
        ;;
    *)
        # default (anything else)
        ;;
esac
```

```bash
case "$1" in
    -l) echo "Listing mode" ;;
    -d) echo "Delete mode" ;;
    *)  echo "Unknown option: $1"; exit 1 ;;
esac
```

### Exercises

**5.5** Write a script `mode.sh` that accepts exactly one argument:
- `-r`: prints `Read mode`
- `-w`: prints `Write mode`
- `-x`: prints `Execute mode`
- anything else: prints `Unknown mode` and exits with code 1

Validate that exactly one argument is provided (error if 0 or more than 1).

---

## 5.5 Putting It Together — Exam-Style Script

The exam scripting question always combines: user input, a log file with an absolute path, `if`/`case` for argument handling, and at least two different execution flows.

**Key checklist for exam scripts:**
- Use `#!/bin/bash` at the top
- Store the log file path in a variable using an **absolute path** (e.g., `/home/$USER/logfile.txt`)
- Use `read -p "..." var` to prompt for input
- Append to the log file with `>>`; never overwrite with `>`
- Add a timestamp with `$(date ...)`
- Validate `$#` at the start and print a clear error message for wrong usage
- End with `exit 0` (success) or `exit 1` (error)

### Exercise

**5.6** Write a script `studylog.sh` that:

When called **without arguments**:
- Asks the user for their name, a time slot (e.g., `14:00-16:00`), and a date (YYYY-MM-DD)
- Automatically adds the current timestamp using `$(date "+%Y-%m-%d %H:%M")`
- Appends one line to `/home/$USER/studylog.txt` in the format:
  `name#timeslot#date#timestamp`

When called with **`-l`**:
- Asks the user how many recent entries to show
- Shows that many lines from the **end** of the log file

When called with **`-d`**:
- Clears the log file (empties it) with confirmation message

When called with anything else or wrong number of arguments:
- Prints a clear error message and exits with code 1

> ⚠️ Use absolute paths for all file references.

---

## Navigation

**Previous:** [← Links and Permissions](04-links-and-permissions.md)
**Lesson Home:** [↑ Lesson 12: Exam Preparation Recap](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)

---

**Solutions:** [→ View all solutions](../Solutions/lesson-12-recap-solutions.md)
