# 8. case Statements

`case` compares **one value** with a list of possibilities. It is a cleaner alternative to a long chain of `if ... elif ... elif ...`.

### The Structure

```
 case $choice in
     1)                  ◀── if $choice is "1"
         date
         ;;              ◀── end of this option (two semicolons!)
     2)                  ◀── if $choice is "2"
         who
         ;;
     *)                  ◀── * = anything else (the "default")
         echo "Invalid"
         ;;
 esac                    ◀── end of case (case backwards)
```

### Example: A Simple Menu

```bash
#!/bin/bash
# Script: system-info.sh

echo "1) Date and time"
echo "2) Logged in users"
echo "3) Disk usage"
read -p "Enter your choice (1-3): " choice

case $choice in
    1)
        date
        ;;
    2)
        who
        ;;
    3)
        df -h
        ;;
    *)
        echo "Invalid choice: $choice"
        exit 1
        ;;
esac
```

### Several Values for One Option: `|`

```bash
#!/bin/bash

read -p "Enter a file extension: " ext

case $ext in
    txt|doc|docx)
        echo "Text document"
        ;;
    jpg|jpeg|png)
        echo "Image file"
        ;;
    mp3|wav)
        echo "Audio file"
        ;;
    *)
        echo "Unknown file type"
        ;;
esac
```

> 💡 The patterns in `case` work like **globbing** (lesson 6): `*` matches anything, `?` one character, `[Yy]` one character from the list. For example, `[Yy]*)` matches `y`, `Y`, `yes` and `Yes`.

---

## Navigation

**Next:** [→ Script Examples And Best Practices](09-script-examples-and-best-practices.md)  
**Previous:** [← Loops](07-loops.md)  
**Lesson Home:** [↑ Lesson 8: Shell Scripting Fundamentals](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
