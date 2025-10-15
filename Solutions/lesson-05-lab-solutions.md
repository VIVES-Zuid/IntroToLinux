# Solutions for Lesson 5: Advanced Commands and Control Operators

## Lab 5.1: Quotes and Variables (10 minutes)

### Solution:

```bash
# 1. Variable practice
NAME="Linux Student"             # Variable with space - needs quotes
COURSE="Introduction to Linux"   # Variable with space - needs quotes

echo "Student: $NAME"            # Double quotes: variable expansion
# Output: Student: Linux Student

echo 'Student: $NAME'            # Single quotes: literal text
# Output: Student: $NAME

echo "Course: \"$COURSE\""       # Escaped quotes inside double quotes
# Output: Course: "Introduction to Linux"

# Additional variable examples:
echo "Welcome, $NAME, to $COURSE"
echo 'The variable $USER contains:' $USER
echo "Today is $(date +%A)"      # Command substitution

# 2. File names with spaces
mkdir "My Documents"             # Create directory with space in name
cd "My Documents"                # Navigate to it (quotes required)
pwd                              # Should show: /path/to/My Documents

touch "important file.txt"       # Create file with space in name
ls -la                           # List contents
# Should show: important file.txt

# Alternative methods for spaces:
touch my\ important\ file.txt    # Escape spaces with backslash
touch 'another file.txt'         # Single quotes work too

cd ..                           # Go back up

# 3. Mixed quotes
echo "User's home is: $HOME"     # Double quotes with apostrophe
# Output: User's home is: /home/username

echo 'Use $HOME for home directory'  # Single quotes - literal
# Output: Use $HOME for home directory

# Complex examples:
echo "The user's name is '$USER' and home is '$HOME'"
echo 'To use variables, write: echo "$VARIABLE"'
echo "Command: echo 'Hello World'"
```

## Lab 5.2: Command Discovery (10 minutes)

### Solution:

```bash
# 1. Check command types
type ls                          # Shows: ls is /bin/ls (or /usr/bin/ls)
type cd                          # Shows: cd is a shell builtin
type echo                        # Shows: echo is a shell builtin (usually)

# More detailed type information:
type -a echo                     # Shows all locations of echo
type -t ls                       # Shows type only: file
type -t cd                       # Shows type only: builtin

# Check if commands exist:
type grep >/dev/null && echo "grep available" || echo "grep not found"

# 2. Find command locations
which ls                         # Shows: /bin/ls or /usr/bin/ls
which bash                       # Shows: /bin/bash
which nonexistent               # Shows nothing (command not found)

# Handle commands that might not exist:
which python3 && echo "Python3 found" || echo "Python3 not installed"

# 3. Find available commands
command -v python3              # Portable way to find commands
command -v docker               # Check if Docker is installed
command -v git                  # Check if Git is installed

# Test results:
if command -v git >/dev/null; then
    echo "Git is available at: $(command -v git)"
    git --version
else
    echo "Git is not installed"
fi

# Alternative discovery methods:
whereis ls                       # Shows binary, source, manual locations
locate bash                      # Find all files containing 'bash'

# 4. Explore alternatives
type -a echo                     # Show all echo implementations
# Might show:
# echo is a shell builtin
# echo is /bin/echo

type -a test                     # Show all test implementations
# Might show:
# test is a shell builtin
# test is /usr/bin/test

# Check what's available in PATH:
echo $PATH | tr ':' '\n' | head -5
ls /usr/bin | head -10           # Sample of available commands
```

## Lab 5.3: Aliases (15 minutes)

### Solution:

```bash
# 1. Create useful aliases
alias ll='ls -la'               # Long listing with hidden files
alias h='history'               # Short for history
alias c='clear'                 # Short for clear
alias psg='ps aux | grep'       # Process search

# Verify aliases were created:
alias                           # Show all current aliases

# 2. Test aliases
ll                              # Should show detailed file listing
# Example output:
# total 24
# drwxr-xr-x  3 user user 4096 Oct 15 10:30 .
# drwxr-xr-x 20 user user 4096 Oct 15 10:25 ..
# -rw-r--r--  1 user user   45 Oct 15 10:30 fruits.txt

h | tail -5                     # Show last 5 commands from history
c                               # Clear the screen

# Test the process search alias:
psg bash                        # Find bash processes
psg $USER                       # Find your processes

# 3. Create directory navigation aliases
alias ..='cd ..'                # Go up one directory
alias ...='cd ../..'            # Go up two directories
alias home='cd ~'               # Go to home directory

# Additional useful aliases:
alias la='ls -la'               # Same as ll
alias l='ls -CF'                # Column format
alias grep='grep --color=auto'  # Colored grep output

# 4. Test navigation
# First, create a test directory structure:
mkdir -p test/deep/directory
cd test/deep/directory
pwd                             # Should show: /path/to/test/deep/directory

...                             # Should go up two levels
pwd                             # Should show: /path/to/test

..                              # Should go up one level
pwd                             # Should show: /path/to

home                            # Should go to home directory
pwd                             # Should show: /home/username

# 5. List and remove aliases
alias                           # Show all aliases
# Example output:
# alias ..='cd ..'
# alias ...='cd ../..'
# alias c='clear'
# alias h='history'
# alias home='cd ~'
# alias ll='ls -la'
# alias psg='ps aux | grep'

unalias c                       # Remove the 'c' alias
alias                           # Verify 'c' is gone
c                               # Should now give "command not found"

# More alias management:
unalias ll h                    # Remove multiple aliases
alias | grep -E '^(ll|h)='     # Verify they're gone (should show nothing)
```

## Lab 5.4: Control Operators (15 minutes)

### Solution:

```bash
# 1. Sequential execution
pwd; date; whoami               # Run three commands in sequence
# Example output:
# /home/username
# Mon Oct 15 10:30:45 UTC 2024
# username

# More examples:
echo "Starting"; sleep 2; echo "Done"  # With delay
ls; wc -l *.txt; echo "Files processed"

# 2. Conditional execution
mkdir testdir && echo "Directory created"
# If mkdir succeeds, echo runs
# Output: Directory created

mkdir testdir && echo "Created" || echo "Failed"
# Try to create directory again (will fail)
# Output: Failed (because directory already exists)

# Test with file operations:
touch newfile.txt && echo "File created successfully"
cp newfile.txt backup.txt && echo "Backup created" || echo "Backup failed"

# 3. Error handling
cd /nonexistent || echo "Directory not found"
# Output: Directory not found (after error message from cd)

cd /tmp && pwd || echo "Could not change directory"
# Output: /tmp (because cd succeeded)

# More error handling examples:
rm nonexistent.txt 2>/dev/null || echo "File didn't exist"
cp important.txt backup/ || { echo "Backup failed!"; exit 1; }

# 4. Background processes
sleep 10 &                      # Start 10-second sleep in background
echo "Process ID: $!"           # Show PID of background process
jobs                            # Show active background jobs
# Output: [1]+ Running    sleep 10 &

sleep 5 &                       # Start another background process
jobs                            # Show all background jobs
# Output: 
# [1]- Running    sleep 10 &
# [2]+ Running    sleep 5 &

# Wait for jobs to complete:
wait                            # Wait for all background jobs
jobs                            # Should show no jobs

# Foreground a background job:
sleep 15 &                      # Start background job
jobs                            # Note the job number
fg %1                           # Bring job 1 to foreground (Ctrl+C to stop)

# 5. Complex chains
cd /tmp && \
    mkdir lab5 && \
    cd lab5 && \
    touch file1.txt file2.txt && \
    ls -la || \
    echo "Something went wrong"

# If all commands succeed, you should see:
# total 8
# drwxr-xr-x 2 user user 4096 Oct 15 10:35 .
# drwxrwxrwt 15 root root 4096 Oct 15 10:35 ..
# -rw-r--r-- 1 user user    0 Oct 15 10:35 file1.txt
# -rw-r--r-- 1 user user    0 Oct 15 10:35 file2.txt

# Test with intentional failure:
cd /tmp && \
    mkdir lab5_existing && \
    cd nonexistent_dir && \
    touch file.txt || \
    echo "Chain failed as expected"
# Output: Chain failed as expected (after error from cd)

# Clean up:
cd ~ && rm -rf /tmp/lab5 /tmp/lab5_existing
```

## Advanced Control Operator Examples:

```bash
# Complex conditional chains:
[ -f "config.txt" ] && source config.txt || echo "No config file"

# Multiple background processes:
for i in {1..3}; do
    echo "Starting process $i"
    sleep $((i*2)) &
done
jobs
wait
echo "All processes completed"

# Error handling with logging:
{
    command1 &&
    command2 &&
    command3
} 2>&1 | tee operation.log || {
    echo "Operation failed, check operation.log"
    exit 1
}

# Process management:
long_running_command &
PID=$!
echo "Started process $PID"
sleep 5
kill $PID 2>/dev/null && echo "Process terminated"
```

## Troubleshooting Common Issues:

```bash
# Issue 1: Alias not working
alias ll='ls -la'
ll                              # Works in current session
# But won't work in new terminal unless added to ~/.bashrc

# Issue 2: Background job not starting
sleep 10&                       # Missing space before &
sleep 10 &                      # Correct syntax

# Issue 3: Control operator precedence
echo "test" && false || echo "this runs"    # "this runs" appears
echo "test" && { false || echo "this too"; }  # Different grouping

# Issue 4: Quote mixing
alias broken='echo "Hello $USER"'           # Works
alias problem='echo 'Hello $USER''          # Syntax error
alias fixed='echo '\''Hello $USER'\'''      # Escaped quotes

# Issue 5: Command substitution in aliases
alias date_long="echo Today is $(date)"     # Evaluated when alias is created
alias date_live='echo Today is $(date)'     # Evaluated when alias is used
```

## Verification Commands:

```bash
# Verify all lab exercises:
echo "=== Current Aliases ==="
alias

echo "=== Directory Navigation Test ==="
cd /tmp && pwd && cd ~ && pwd

echo "=== Background Jobs ==="
sleep 3 &
jobs
wait

echo "=== Command Types ==="
type ls cd echo which

echo "=== Variables and Quotes ==="
NAME="Test User"
echo "Name: $NAME"
echo 'Literal: $NAME'
```

## Key Learning Points:

1. **Quotes**: Double quotes allow variable expansion, single quotes are literal
2. **Command Types**: Built-ins vs external commands vs aliases
3. **Aliases**: Create shortcuts for frequently used commands
4. **Control Operators**: `&&` (AND), `||` (OR), `;` (sequence), `&` (background)
5. **Error Handling**: Use `||` to handle command failures gracefully

---

## Navigation

**Previous:** [← Lesson 4: I/O Redirection and Pipes](lesson-04-lab-solutions.md)  
**Next:** [→ Lesson 6: File Operations, Globbing, and Archiving](lesson-06-lab-solutions.md)  
**Home:** [↑ Solutions Index](README.md)