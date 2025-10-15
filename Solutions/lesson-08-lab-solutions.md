# Solutions for Lesson 8: Shell Scripting Fundamentals

## Lab 8.1: Basic Script Creation (15 minutes)

### Solution:

```bash
# 1. Create a simple greeting script
cat > greeting.sh << 'EOF'
#!/bin/bash
# Simple greeting script

echo "Hello! Welcome to shell scripting!"
echo "Today is: $(date)"
echo "You are logged in as: $USER"
echo "Your home directory is: $HOME"
echo "Current directory: $(pwd)"
echo "System uptime: $(uptime -p)"
EOF

# 2. Make executable and test
chmod +x greeting.sh
./greeting.sh

# Expected output:
# Hello! Welcome to shell scripting!
# Today is: Mon Oct 15 10:30:45 UTC 2024
# You are logged in as: username
# Your home directory is: /home/username
# Current directory: /home/username
# System uptime: up 2 hours, 15 minutes

# 3. Create an interactive script
cat > interactive.sh << 'EOF'
#!/bin/bash
# Interactive script

echo "=== User Information Collector ==="
read -p "What's your name? " name
read -p "What's your favorite color? " color
read -p "What's your age? " age

echo
echo "=== Summary ==="
echo "Hello, $name!"
echo "Your favorite color is $color."
echo "You are $age years old."
echo "Nice to meet you!"

# Create a simple profile
echo "Creating profile file..."
cat > user_profile.txt << PROFILE
Name: $name
Favorite Color: $color
Age: $age
Date Created: $(date)
PROFILE

echo "Profile saved to user_profile.txt"
EOF

chmod +x interactive.sh
# Run interactively: ./interactive.sh

# Additional script examples:
cat > system_info.sh << 'EOF'
#!/bin/bash
# System information script

echo "=== System Information ==="
echo "Hostname: $(hostname)"
echo "Operating System: $(uname -o)"
echo "Kernel Version: $(uname -r)"
echo "Architecture: $(uname -m)"
echo "Current User: $USER"
echo "Shell: $SHELL"
echo "Home Directory: $HOME"
echo "Current Path: $PWD"
echo "Date and Time: $(date)"
echo "Uptime: $(uptime -p)"
echo "Load Average: $(uptime | awk -F'load average:' '{print $2}')"

echo
echo "=== Memory Information ==="
free -h

echo
echo "=== Disk Usage ==="
df -h | head -5

echo
echo "=== Network Information ==="
hostname -I | awk '{print "IP Address: " $1}'
EOF

chmod +x system_info.sh
./system_info.sh
```

## Lab 8.2: File Processing Script (20 minutes)

### Solution:

```bash
# 1. Create test files
mkdir script_test && cd script_test
touch file{1..5}.txt
touch document{1..3}.pdf
touch image{1..2}.jpg
touch data{1..2}.csv
touch script{1..2}.sh

# Create some files with content:
echo "Text content" > file1.txt
echo "More text" > file2.txt
echo "PDF content" > document1.pdf
echo "Image data" > image1.jpg

# 2. Create file counter script
cat > count_files.sh << 'EOF'
#!/bin/bash
# Count files by extension

echo "File Statistics for: $(pwd)"
echo "===================="
echo "Generated on: $(date)"
echo

# Count different file types
total_files=0

for ext in txt pdf jpg csv sh; do
    # Count files with this extension
    count=$(ls *.$ext 2>/dev/null | wc -l)
    echo "$ext files: $count"
    total_files=$((total_files + count))
done

echo "===================="
echo "Total files: $total_files"
echo "Directory size: $(du -sh . | cut -f1)"

# Show largest files
echo
echo "Largest files:"
ls -lhS | head -6

# Show file details by type
echo
echo "=== File Details ==="
for ext in txt pdf jpg; do
    files=$(ls *.$ext 2>/dev/null)
    if [ -n "$files" ]; then
        echo
        echo "$ext files:"
        for file in $files; do
            size=$(stat -c%s "$file" 2>/dev/null || echo "0")
            echo "  $file ($size bytes)"
        done
    fi
done
EOF

chmod +x count_files.sh
./count_files.sh

# Expected output showing counts and details for each file type

# 3. Advanced file processing script
cat > analyze_files.sh << 'EOF'
#!/bin/bash
# Advanced file analysis script

echo "=== Advanced File Analysis ==="
echo "Directory: $(pwd)"
echo "Analysis Date: $(date)"
echo

# Function to get file count by extension
count_by_extension() {
    local ext="$1"
    find . -name "*.$ext" -type f 2>/dev/null | wc -l
}

# Function to get total size by extension
size_by_extension() {
    local ext="$1"
    find . -name "*.$ext" -type f -exec stat -c%s {} \; 2>/dev/null | \
        awk '{sum+=$1} END {print (sum ? sum : 0)}'
}

# Analyze different file types
extensions=("txt" "pdf" "jpg" "csv" "sh" "log")

echo "Extension | Count | Total Size (bytes)"
echo "----------|-------|------------------"

for ext in "${extensions[@]}"; do
    count=$(count_by_extension "$ext")
    size=$(size_by_extension "$ext")
    printf "%-9s | %-5s | %-15s\n" "$ext" "$count" "$size"
done

echo
echo "=== File Age Analysis ==="
echo "Files modified in last 24 hours:"
find . -type f -mtime -1 | wc -l

echo "Files older than 7 days:"
find . -type f -mtime +7 | wc -l

echo
echo "=== Permission Analysis ==="
echo "Executable files:"
find . -type f -executable | wc -l

echo "Readable files:"
find . -type f -readable | wc -l

echo "Writable files:"
find . -type f -writable | wc -l

# Check for empty files
echo
echo "=== Empty Files ==="
empty_files=$(find . -type f -empty)
if [ -n "$empty_files" ]; then
    echo "Empty files found:"
    echo "$empty_files"
else
    echo "No empty files found"
fi
EOF

chmod +x analyze_files.sh
./analyze_files.sh
```

## Lab 8.3: Conditional Logic Practice (20 minutes)

### Solution:

```bash
# Create file checker script
cat > file_checker.sh << 'EOF'
#!/bin/bash
# File and directory checker

# Check if argument provided
if [ $# -eq 0 ]; then
    echo "Usage: $0 <file_or_directory>"
    echo "This script analyzes files and directories"
    exit 1
fi

TARGET="$1"

echo "Checking: $TARGET"
echo "=================="

# Check if target exists
if [ -e "$TARGET" ]; then
    echo "✓ Exists"
    
    # Check what type it is
    if [ -f "$TARGET" ]; then
        echo "✓ Is a file"
        
        # Get file size
        if command -v stat >/dev/null 2>&1; then
            size=$(stat -c%s "$TARGET" 2>/dev/null || stat -f%z "$TARGET" 2>/dev/null)
            echo "  Size: $size bytes"
        else
            size=$(ls -l "$TARGET" | awk '{print $5}')
            echo "  Size: $size bytes"
        fi
        
        # Check permissions
        if [ -r "$TARGET" ]; then
            echo "  ✓ Readable"
        else
            echo "  ✗ Not readable"
        fi
        
        if [ -w "$TARGET" ]; then
            echo "  ✓ Writable"
        else
            echo "  ✗ Not writable"
        fi
        
        if [ -x "$TARGET" ]; then
            echo "  ✓ Executable"
        else
            echo "  ✗ Not executable"
        fi
        
        # Check if file is empty
        if [ -s "$TARGET" ]; then
            echo "  ✓ Contains data"
        else
            echo "  ⚠ Empty file"
        fi
        
        # Check file type
        if command -v file >/dev/null 2>&1; then
            file_type=$(file -b "$TARGET")
            echo "  Type: $file_type"
        fi
        
    elif [ -d "$TARGET" ]; then
        echo "✓ Is a directory"
        
        # Count contents
        if [ -r "$TARGET" ]; then
            file_count=$(ls -1 "$TARGET" 2>/dev/null | wc -l)
            echo "  Contains: $file_count items"
            
            # Show breakdown of contents
            dir_count=$(find "$TARGET" -maxdepth 1 -type d 2>/dev/null | wc -l)
            dir_count=$((dir_count - 1))  # Subtract the directory itself
            file_count_actual=$(find "$TARGET" -maxdepth 1 -type f 2>/dev/null | wc -l)
            
            echo "  Subdirectories: $dir_count"
            echo "  Files: $file_count_actual"
        else
            echo "  ✗ Cannot read directory contents"
        fi
        
        # Check directory permissions
        if [ -r "$TARGET" ]; then
            echo "  ✓ Readable"
        else
            echo "  ✗ Not readable"
        fi
        
        if [ -w "$TARGET" ]; then
            echo "  ✓ Writable"
        else
            echo "  ✗ Not writable"
        fi
        
        if [ -x "$TARGET" ]; then
            echo "  ✓ Accessible"
        else
            echo "  ✗ Not accessible"
        fi
        
    elif [ -L "$TARGET" ]; then
        echo "✓ Is a symbolic link"
        
        # Check where it points
        if command -v readlink >/dev/null 2>&1; then
            link_target=$(readlink "$TARGET")
            echo "  Points to: $link_target"
            
            # Check if link is broken
            if [ -e "$link_target" ]; then
                echo "  ✓ Link target exists"
            else
                echo "  ✗ Broken link"
            fi
        fi
        
    else
        echo "✓ Is a special file (device, socket, etc.)"
    fi
    
    # Show timestamps
    echo
    echo "Timestamps:"
    if command -v stat >/dev/null 2>&1; then
        echo "  Modified: $(stat -c %y "$TARGET" 2>/dev/null || stat -f %Sm "$TARGET" 2>/dev/null)"
        echo "  Accessed: $(stat -c %x "$TARGET" 2>/dev/null || stat -f %Sa "$TARGET" 2>/dev/null)"
    else
        echo "  Modified: $(ls -l "$TARGET" | awk '{print $6, $7, $8}')"
    fi
    
else
    echo "✗ Does not exist"
    
    # Check if parent directory exists
    parent_dir=$(dirname "$TARGET")
    if [ -d "$parent_dir" ]; then
        echo "  Parent directory exists: $parent_dir"
        if [ -w "$parent_dir" ]; then
            echo "  ✓ Parent directory is writable"
        else
            echo "  ✗ Parent directory is not writable"
        fi
    else
        echo "  ✗ Parent directory does not exist: $parent_dir"
    fi
fi

echo
echo "Analysis completed at: $(date)"
EOF

chmod +x file_checker.sh

# Test the script
echo "Testing file_checker.sh with different targets:"
./file_checker.sh count_files.sh      # Test with existing file
./file_checker.sh .                   # Test with directory
./file_checker.sh nonexistent         # Test with non-existing file

# Create additional test cases
touch empty_file.txt                  # Empty file
echo "content" > content_file.txt     # File with content
ln -s content_file.txt link_file.txt  # Symbolic link

./file_checker.sh empty_file.txt
./file_checker.sh content_file.txt
./file_checker.sh link_file.txt
```

## Lab 8.4: Loop Practice (15 minutes)

### Solution:

```bash
# Create system monitor script
cat > system_monitor.sh << 'EOF'
#!/bin/bash
# System monitoring with loops

echo "System Monitor"
echo "============="
echo "Starting monitoring at: $(date)"
echo

# Function to get memory usage percentage
get_memory_usage() {
    if command -v free >/dev/null 2>&1; then
        free | grep '^Mem:' | awk '{printf "%.1f%%", $3/$2 * 100}'
    else
        echo "N/A"
    fi
}

# Function to get load average
get_load_average() {
    if [ -f /proc/loadavg ]; then
        cat /proc/loadavg | cut -d' ' -f1-3
    else
        uptime | awk -F'load average:' '{print $2}' | sed 's/^[[:space:]]*//'
    fi
}

# Function to get disk usage
get_disk_usage() {
    df -h / 2>/dev/null | tail -1 | awk '{print $5}' || echo "N/A"
}

# Monitor for 5 iterations
for i in {1..5}; do
    echo "=== Iteration $i/5 ==="
    echo "Time: $(date)"
    echo "Load Average: $(get_load_average)"
    echo "Memory Usage: $(get_memory_usage)"
    echo "Disk Usage (/): $(get_disk_usage)"
    
    # Show top process by CPU
    if command -v ps >/dev/null 2>&1; then
        top_process=$(ps aux --sort=-%cpu | head -2 | tail -1 | awk '{print $11}')
        cpu_usage=$(ps aux --sort=-%cpu | head -2 | tail -1 | awk '{print $3}')
        echo "Top CPU Process: $top_process ($cpu_usage%)"
    fi
    
    # Show number of active processes
    if command -v ps >/dev/null 2>&1; then
        process_count=$(ps aux | wc -l)
        echo "Active Processes: $process_count"
    fi
    
    echo "---"
    
    # Sleep between iterations (except last one)
    if [ $i -lt 5 ]; then
        echo "Waiting 2 seconds..."
        sleep 2
        echo
    fi
done

echo
echo "Monitoring complete!"
echo "Final report generated at: $(date)"

# Generate summary report
echo
echo "=== Summary Report ==="
echo "Monitoring Duration: 10 seconds (5 iterations)"
echo "System: $(hostname)"
echo "User: $USER"
echo "Uptime: $(uptime -p 2>/dev/null || uptime)"
EOF

chmod +x system_monitor.sh
./system_monitor.sh

# Additional loop examples
cat > loop_examples.sh << 'EOF'
#!/bin/bash
# Various loop examples

echo "=== For Loop Examples ==="

# Loop through numbers
echo "Counting 1 to 5:"
for i in {1..5}; do
    echo "Number: $i"
done

echo
echo "Counting by 2s from 2 to 10:"
for i in {2..10..2}; do
    echo "Even number: $i"
done

echo
echo "Looping through files:"
for file in *.sh; do
    if [ -f "$file" ]; then
        echo "Script: $file ($(wc -l < "$file") lines)"
    fi
done

echo
echo "=== While Loop Examples ==="

# While loop counter
counter=1
echo "While loop counting to 3:"
while [ $counter -le 3 ]; do
    echo "Counter: $counter"
    counter=$((counter + 1))
done

echo
echo "Reading file line by line:"
if [ -f "/etc/passwd" ]; then
    echo "First 3 users from /etc/passwd:"
    head -3 /etc/passwd | while read line; do
        username=$(echo "$line" | cut -d: -f1)
        uid=$(echo "$line" | cut -d: -f3)
        echo "User: $username (UID: $uid)"
    done
fi

echo
echo "=== Until Loop Example ==="

# Until loop
countdown=3
echo "Countdown using until loop:"
until [ $countdown -eq 0 ]; do
    echo "T-minus $countdown"
    countdown=$((countdown - 1))
    sleep 1
done
echo "Liftoff!"

echo
echo "=== Nested Loop Example ==="

echo "Multiplication table (3x3):"
for i in {1..3}; do
    for j in {1..3}; do
        result=$((i * j))
        printf "%d x %d = %d  " $i $j $result
    done
    echo  # New line after each row
done

echo
echo "=== Loop Control Examples ==="

echo "Using break and continue:"
for i in {1..10}; do
    if [ $i -eq 3 ]; then
        echo "Skipping $i"
        continue
    fi
    if [ $i -eq 8 ]; then
        echo "Breaking at $i"
        break
    fi
    echo "Processing $i"
done
EOF

chmod +x loop_examples.sh
./loop_examples.sh
```

## Advanced Scripting Examples:

```bash
# Error handling and logging script
cat > advanced_script.sh << 'EOF'
#!/bin/bash
# Advanced script with error handling and logging

# Set script options
set -euo pipefail  # Exit on error, undefined variables, pipe failures

# Configuration
LOG_FILE="script.log"
DEBUG=false

# Logging function
log() {
    local level="$1"
    shift
    local message="$*"
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')
    echo "[$timestamp] [$level] $message" | tee -a "$LOG_FILE"
}

# Debug function
debug() {
    if [ "$DEBUG" = true ]; then
        log "DEBUG" "$*"
    fi
}

# Error handling function
handle_error() {
    local exit_code=$?
    local line_number=$1
    log "ERROR" "Script failed at line $line_number with exit code $exit_code"
    exit $exit_code
}

# Trap errors
trap 'handle_error $LINENO' ERR

# Main script logic
main() {
    log "INFO" "Script started"
    
    # Example of function with error checking
    if create_backup_directory; then
        log "INFO" "Backup directory created successfully"
    else
        log "ERROR" "Failed to create backup directory"
        return 1
    fi
    
    # Example of processing files
    process_files "*.txt"
    
    log "INFO" "Script completed successfully"
}

# Function to create backup directory
create_backup_directory() {
    local backup_dir="backup_$(date +%Y%m%d_%H%M%S)"
    
    debug "Creating backup directory: $backup_dir"
    
    if mkdir "$backup_dir"; then
        echo "Backup directory created: $backup_dir"
        return 0
    else
        return 1
    fi
}

# Function to process files
process_files() {
    local pattern="$1"
    local count=0
    
    debug "Processing files matching pattern: $pattern"
    
    for file in $pattern; do
        if [ -f "$file" ]; then
            log "INFO" "Processing file: $file"
            count=$((count + 1))
            # Simulate file processing
            sleep 0.1
        fi
    done
    
    log "INFO" "Processed $count files"
}

# Parse command line options
while getopts "dh" opt; do
    case $opt in
        d)
            DEBUG=true
            log "INFO" "Debug mode enabled"
            ;;
        h)
            echo "Usage: $0 [-d] [-h]"
            echo "  -d  Enable debug mode"
            echo "  -h  Show this help"
            exit 0
            ;;
        \?)
            log "ERROR" "Invalid option: -$OPTARG"
            exit 1
            ;;
    esac
done

# Run main function
main "$@"
EOF

chmod +x advanced_script.sh
./advanced_script.sh -d  # Run with debug mode
```

## Troubleshooting Common Issues:

```bash
# Common scripting mistakes and solutions:

# Issue 1: Missing shebang
echo '#!/bin/bash' > test_script.sh
echo 'echo "Hello"' >> test_script.sh
chmod +x test_script.sh
./test_script.sh  # Works correctly

# Issue 2: Variable expansion problems
name="John"
echo "Hello $name"        # Correct: Hello John
echo 'Hello $name'        # Wrong: Hello $name

# Issue 3: Command substitution
current_date=$(date)      # Correct
current_date=`date`       # Old style, still works
echo "Today is $current_date"

# Issue 4: Exit codes
./non_existent_command
echo "Exit code: $?"      # Shows non-zero for failure

true
echo "Exit code: $?"      # Shows 0 for success

# Issue 5: File test operators
file="test.txt"
[ -f "$file" ] && echo "File exists" || echo "File doesn't exist"
[ -r "$file" ] && echo "File is readable"
[ -w "$file" ] && echo "File is writable"
[ -x "$file" ] && echo "File is executable"
```

## Verification Commands:

```bash
# Test all created scripts:
echo "=== Testing Scripts ==="

if [ -x greeting.sh ]; then
    echo "✓ greeting.sh is executable"
else
    echo "✗ greeting.sh is not executable"
fi

if [ -x count_files.sh ]; then
    echo "✓ count_files.sh is executable"
else
    echo "✗ count_files.sh is not executable"
fi

if [ -x file_checker.sh ]; then
    echo "✓ file_checker.sh is executable"
else
    echo "✗ file_checker.sh is not executable"
fi

if [ -x system_monitor.sh ]; then
    echo "✓ system_monitor.sh is executable"
else
    echo "✗ system_monitor.sh is not executable"
fi

echo
echo "=== Script Line Counts ==="
wc -l *.sh

echo
echo "=== Quick Functionality Test ==="
echo "Testing file_checker.sh with current directory:"
./file_checker.sh . | head -10
```

## Key Learning Points:

1. **Shebang**: Always start scripts with `#!/bin/bash`
2. **Variables**: Use quotes to handle spaces and special characters
3. **Conditionals**: Test conditions with `[ ]` or `[[ ]]`
4. **Loops**: Use for, while, and until for repetitive tasks
5. **Functions**: Organize code into reusable functions
6. **Error Handling**: Check exit codes and handle failures gracefully
7. **User Input**: Use `read` for interactive scripts
8. **Command Substitution**: Use `$()` for capturing command output

---

## Navigation

**Previous:** [← Lesson 7: Text Processing and Filters](lesson-07-lab-solutions.md)  
**Next:** [→ Lesson 9: Users and Groups](lesson-09-lab-solutions.md)  
**Home:** [↑ Solutions Index](README.md)