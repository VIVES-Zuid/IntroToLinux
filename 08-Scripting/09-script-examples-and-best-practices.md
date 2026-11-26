# 9. Script Examples and Best Practices

### Example 1: Backup a Folder

Uses only what you learned so far: variables, `$( )`, an argument check, `if`, `tar` (lesson 6).

```bash
#!/bin/bash
#
# Script:  backup-folder.sh
# Purpose: Make a .tar.gz backup of a folder, with the date in the name
# Usage:   ./backup-folder.sh <folder>
#

# 1. Check the argument
if [ $# -ne 1 ]; then
    echo "Usage: $0 <folder>"
    exit 1
fi

folder="$1"
backup_dir="$HOME/backups"
today=$(date +%F)                       # e.g. 2024-01-15

# 2. Does the folder exist?
if [ ! -d "$folder" ]; then
    echo "Error: $folder is not a directory"
    exit 1
fi

# 3. Create the backup directory if it doesn't exist yet
mkdir -p "$backup_dir"

# 4. Make the backup
backup_file="$backup_dir/$(basename "$folder")-$today.tar.gz"
tar -czf "$backup_file" "$folder"

echo "Backup created: $backup_file"
```

```
 $ ./backup-folder.sh Documents
 Backup created: /home/student/backups/Documents-2024-01-15.tar.gz
```

> 💡 `basename /home/student/Documents` gives just the last part: `Documents`.

### Example 2: Count Files per Extension

```bash
#!/bin/bash
#
# Script:  count-files.sh
# Purpose: Show how many files of each type are in the current directory
#

for ext in txt pdf jpg sh; do
    count=$(ls *.$ext 2>/dev/null | wc -l)
    echo "$ext files: $count"
done
```

```
 $ ./count-files.sh
 txt files: 5
 pdf files: 3
 jpg files: 2
 sh files: 1
```

### ✅ Best Practices Checklist

| ✅ Do | Why |
|------|-----|
| Start with `#!/bin/bash` | the right interpreter runs your script |
| Add a header comment (purpose + usage) | others (and you, later) understand it |
| Check the arguments (`$#`) and print a usage line | a clear message instead of a strange error |
| Put variables in double quotes: `"$file"` | filenames with spaces keep working |
| Use clear lowercase names: `backup_dir`, not `x` | readable, and you don't overwrite `$USER`, `$HOME` ... |
| `exit 1` on an error, `exit 0` on success | other scripts can check if yours worked (`$?`) |
| Test with harmless data first | a script with `rm` in a loop can do a lot of damage, fast |

### 🐞 Debugging a Script

```bash
# Show every command before it runs (with the variables filled in)
bash -x ./backup-folder.sh Documents
```

```
 + '[' 1 -ne 1 ']'
 + folder=Documents
 + backup_dir=/home/student/backups
 ...
```

Lines starting with `+` show what bash really executes, which is very useful for finding typos and empty variables.

### ℹ️ Advanced: Functions

> ℹ️ **Advanced** – optional, not needed for the labs.

A function is a **named block of commands** you can call several times:

```bash
#!/bin/bash

log_message() {
    echo "$(date '+%H:%M:%S') - $1"     # $1 is the function's first argument
}

log_message "Script started"
log_message "Doing some work ..."
log_message "Script finished"
```

```
 10:30:01 - Script started
 10:30:01 - Doing some work ...
 10:30:01 - Script finished
```

Inside a function you can use `local name="..."` for a variable that only exists in that function, and `return 0` / `return 1` instead of `exit`.

---

## Navigation

**Next:** [→ Practical Labs](10-practical-labs.md)  
**Previous:** [← Case Statements](08-case-statements.md)  
**Lesson Home:** [↑ Lesson 8: Shell Scripting Fundamentals](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
