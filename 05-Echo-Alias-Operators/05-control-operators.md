# 5. Control Operators


Control operators let you chain commands and control their execution based on success or failure.

### Semicolon (;): Sequential Execution

```bash
# Run commands one after another regardless of success/failure
command1; command2; command3

# Examples
pwd; ls; date
cd /tmp; ls; pwd
mkdir test; cd test; touch file.txt
```

### Ampersand (&): Background Execution

```bash
# Run command in background
command &

# Examples
sleep 30 &              # Sleep in background
firefox &               # Start Firefox in background
cp large_file.iso backup/ &  # Copy large file in background

# Check background jobs
jobs
jobs -l

# Bring background job to foreground
fg %1

# Send job to background
bg %1
```

### Logical AND (&&): Execute if Previous Succeeds

```bash
# Run command2 only if command1 succeeds (exit status 0)
command1 && command2

# Examples
cd /tmp && ls            # List only if cd succeeds
mkdir backup && cp file.txt backup/
make && make install     # Install only if compilation succeeds
```

### Logical OR (||): Execute if Previous Fails

```bash
# Run command2 only if command1 fails (exit status ≠ 0)
command1 || command2

# Examples
cd /nonexistent || echo "Directory not found"
ping -c1 google.com || echo "No internet connection"
which python3 || echo "Python 3 not installed"
```

### Exit Status ($?): Check Command Success

```bash
# Check exit status of last command
echo $?
# 0 = success, non-zero = failure

# Examples
ls /home
echo $?              # Should be 0 (success)

ls /nonexistent
echo $?              # Non-zero (failure)

# Use in conditional logic
grep "pattern" file.txt
if [ $? -eq 0 ]; then
    echo "Pattern found"
else
    echo "Pattern not found"
fi
```

### Combining Control Operators:

```bash
# Complex chains
cd /tmp && mkdir test && cd test && touch file.txt || echo "Something failed"

# Backup with error handling
cp important.txt backup/ && echo "Backup successful" || echo "Backup failed"

# Install and configure
sudo apt update && sudo apt install nginx && sudo systemctl start nginx
```

### Comments (#): Documentation

```bash
# This is a comment
ls -la              # List files with details

# Multi-line operations with comments
cd /var/log &&      # Go to log directory
ls -la &&           # List log files  
tail -f syslog      # Follow system log
```
---

## Navigation

**Next:** [→ Line Continuation And Escaping](06-line-continuation-and-escaping.md)  
**Previous:** [← Aliases Creating Command Shortcuts](04-aliases-creating-command-shortcuts.md)  
**Lesson Home:** [↑ Lesson 5: Echo, Alias & Operators](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
