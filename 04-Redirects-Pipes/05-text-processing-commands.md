# 5. Text Processing Commands


### head and tail:

```bash
# First 10 lines (default)
head filename.txt
head /etc/passwd

# First n lines
head -n 5 filename.txt
head -5 filename.txt

# Last 10 lines (default)  
tail filename.txt
tail /var/log/syslog

# Last n lines
tail -n 20 filename.txt
tail -20 filename.txt

# Follow file changes (useful for logs)
tail -f /var/log/syslog

# Combine with pipes
ps aux | head -20
history | tail -15
```

### cat and tac:

```bash
# Display file contents
cat filename.txt

# Display multiple files
cat file1.txt file2.txt

# Number lines
cat -n filename.txt

# Show non-printing characters
cat -A filename.txt

# Reverse line order (tac = cat backwards)
tac filename.txt
seq 1 10 | tac
```

### more and less:

```bash
# Page through file content
more filename.txt
# Space: next page, Enter: next line, q: quit

# Enhanced pager (recommended)
less filename.txt
# Space/PageDown: next page
# b/PageUp: previous page  
# /: search forward
# ?: search backward
# q: quit

# Use with pipes
ps aux | less
history | less
man ls | less
```

### strings:

```bash
# Extract readable strings from binary files
strings /bin/ls
strings /usr/bin/file | head -20

# Minimum string length
strings -n 8 binaryfile

# Useful for examining unknown files
file unknown_file
strings unknown_file | head -10
```
---

## Navigation

**Previous:** [← Pipes Chaining Commands](04-pipes-chaining-commands.md)  
**Next:** [→ Combining Redirection And Pipes](06-combining-redirection-and-pipes.md)  
**Lesson Home:** [↑ Lesson 04: Redirects Pipes](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
