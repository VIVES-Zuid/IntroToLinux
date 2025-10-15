# 8. case Statements


```bash
#!/bin/bash
# Script: system-info.sh

echo "System Information Menu"
echo "1) Date and Time"
echo "2) Current Users"
echo "3) Disk Usage"
echo "4) Memory Usage"
echo "5) Exit"

read -p "Enter your choice (1-5): " choice

case $choice in
    1)
        echo "Current Date and Time:"
        date
        ;;
    2)
        echo "Currently Logged Users:"
        who
        ;;
    3)
        echo "Disk Usage:"
        df -h
        ;;
    4)
        echo "Memory Usage:"
        free -h
        ;;
    5)
        echo "Goodbye!"
        exit 0
        ;;
    *)
        echo "Invalid choice: $choice"
        echo "Please enter a number between 1 and 5"
        exit 1
        ;;
esac
```

### Pattern Matching in case:

```bash
#!/bin/bash

read -p "Enter file extension: " ext

case $ext in
    txt|doc|docx)
        echo "Text document"
        ;;
    jpg|jpeg|png|gif)
        echo "Image file"
        ;;
    mp3|wav|flac)
        echo "Audio file"
        ;;
    mp4|avi|mkv)
        echo "Video file"
        ;;
    [Zz][Ii][Pp])
        echo "Archive file (zip)"
        ;;
    *)
        echo "Unknown file type"
        ;;
esac
```
---

## Navigation

**Previous:** [← Loops](07-loops.md)  
**Next:** [→ Script Examples And Best Practices](09-script-examples-and-best-practices.md)  
**Lesson Home:** [↑ Lesson 08: Scripting](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
