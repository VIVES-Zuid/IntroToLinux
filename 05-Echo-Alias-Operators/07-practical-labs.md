# 7. Practical Labs


### Lab 5.1: Quotes and Variables (10 minutes)

```bash
# 1. Variable practice
NAME="Linux Student"
COURSE="Introduction to Linux"

echo "Student: $NAME"
echo 'Student: $NAME'
echo "Course: \"$COURSE\""

# 2. File names with spaces
mkdir "My Documents"
cd "My Documents"
touch "important file.txt"
ls -la
cd ..

# 3. Mixed quotes
echo "User's home is: $HOME"
echo 'Use $HOME for home directory'
```

### Lab 5.2: Command Discovery (10 minutes)

```bash
# 1. Check command types
type ls
type cd  
type echo
which ls
which bash

# 2. Find available commands
command -v python3
command -v docker
command -v git

# 3. Explore alternatives
type -a echo
type -a test
```

### Lab 5.3: Aliases (15 minutes)

```bash
# 1. Create useful aliases
alias ll='ls -la'
alias h='history'
alias c='clear'
alias psg='ps aux | grep'

# 2. Test aliases
ll
h | tail -5
c

# 3. Create directory navigation aliases
alias ..='cd ..'
alias ...='cd ../..'
alias home='cd ~'

# 4. Test navigation
..
pwd
...
pwd
home
pwd

# 5. List and remove aliases
alias
unalias c
alias
```

### Lab 5.4: Control Operators (15 minutes)

```bash
# 1. Sequential execution
pwd; date; whoami

# 2. Conditional execution
mkdir testdir && echo "Directory created"
mkdir testdir && echo "Created" || echo "Failed"

# 3. Error handling
cd /nonexistent || echo "Directory not found"
cd /tmp && pwd || echo "Could not change directory"

# 4. Background processes
sleep 10 &
jobs
sleep 5 &
jobs

# 5. Complex chains
cd /tmp && \
    mkdir lab5 && \
    cd lab5 && \
    touch file1.txt file2.txt && \
    ls -la || \
    echo "Something went wrong"
```
---

## Navigation

**Next:** [→ Best Practices](08-best-practices.md)  
**Previous:** [← Line Continuation And Escaping](06-line-continuation-and-escaping.md)  
**Lesson Home:** [↑ Lesson 5: Echo, Alias & Operators](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
