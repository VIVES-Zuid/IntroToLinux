# 8. Advanced Pipe Concepts


### Named Pipes (FIFOs):

```bash
# Create named pipe
mkfifo mypipe

# Use named pipe (in two terminals)
# Terminal 1: echo "Hello" > mypipe
# Terminal 2: cat < mypipe
```

### Process Substitution:

```bash
# Compare output of two commands
diff <(ls /bin) <(ls /usr/bin)

# Use command output as file input
sort <(cat file1.txt file2.txt)
```

### Pipeline Exit Status:

```bash
# Check exit status of pipeline
command1 | command2
echo $?  # Exit status of command2

# Check all commands in pipeline (bash)
set -o pipefail
command1 | command2
echo ${PIPESTATUS[@]}  # Exit status of all commands
```
---

## Navigation

**Next:** [→ Performance And Best Practices](09-performance-and-best-practices.md)  
**Previous:** [← Practical Labs](07-practical-labs.md)  
**Lesson Home:** [↑ Lesson 4: Redirects & Pipes](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
