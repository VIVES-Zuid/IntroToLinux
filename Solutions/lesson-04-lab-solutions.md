# Solutions for Lesson 4: I/O Redirection and Pipes

## Lab 4.1: Basic Redirection (10 minutes)

### Solution:

```bash
# 1. Create test data
echo "Apple" > fruits.txt        # Create file with first fruit
echo "Banana" >> fruits.txt      # Append second fruit
echo "Cherry" >> fruits.txt      # Append third fruit
echo "Date" >> fruits.txt        # Append fourth fruit

# Verify the file:
cat fruits.txt
# Expected output:
# Apple
# Banana
# Cherry
# Date

# 2. Practice output redirection
ls -la > directory_contents.txt  # Redirect stdout to file
cat directory_contents.txt       # View the captured output

# Test overwrite vs append:
echo "First line" > test.txt     # Creates/overwrites file
echo "Second line" >> test.txt   # Appends to file
cat test.txt                     # Shows both lines

# 3. Practice error redirection
ls /nonexistent > output.txt 2> errors.txt
cat output.txt                   # Should be empty
cat errors.txt                   # Should contain error message
# Expected in errors.txt: ls: cannot access '/nonexistent': No such file or directory

# Test with existing and non-existing files:
ls fruits.txt /nonexistent > mixed_output.txt 2> mixed_errors.txt
cat mixed_output.txt             # Shows: fruits.txt
cat mixed_errors.txt             # Shows error for /nonexistent

# 4. Combine stdout and stderr
ls /nonexistent fruits.txt > combined.txt 2>&1
cat combined.txt
# Expected output:
# ls: cannot access '/nonexistent': No such file or directory
# fruits.txt

# Alternative syntax:
ls /nonexistent fruits.txt &> combined2.txt  # Bash shorthand
cat combined2.txt                # Same result as above
```

## Lab 4.2: Pipe Practice (15 minutes)

### Solution:

```bash
# 1. Basic pipes
ls | wc -l                       # Count files in current directory
ps aux | wc -l                   # Count running processes
cat /etc/passwd | wc -l          # Count users in system

# Examples with expected results:
ls | wc -l                       # Might show: 5 (if 5 files in directory)
ps aux | head -5                 # Show first 5 processes

# 2. Text processing
cat fruits.txt | sort            # Sort fruits alphabetically
# Expected output:
# Apple
# Banana
# Cherry
# Date

cat fruits.txt | sort | tac      # Sort then reverse
# Expected output:
# Date
# Cherry
# Banana
# Apple

# Additional text processing:
cat fruits.txt | sort | head -2  # First 2 after sorting
cat fruits.txt | tail -2         # Last 2 lines

# 3. System information
ps aux | head -1                 # Show process header
ps aux | grep $USER              # Show only your processes

# More system examples:
ps aux | grep bash               # Find bash processes
df -h | grep -v tmpfs           # Disk usage without temp filesystems

# 4. History analysis
history | tail -20               # Last 20 commands
history | grep ls                # All ls commands in history
history | wc -l                  # Total commands in history

# Advanced history analysis:
history | awk '{print $2}' | sort | uniq -c | sort -nr | head -5
# Shows most used commands
```

## Lab 4.3: Advanced Combinations (15 minutes)

### Solution:

```bash
# 1. Create sample data
seq 1 100 > numbers.txt          # Numbers 1-100
seq 50 150 >> numbers.txt        # Append 50-150 (creates duplicates)

# Verify data:
head numbers.txt                 # Should show: 1, 2, 3, 4, 5...
tail numbers.txt                 # Should show: 146, 147, 148, 149, 150
wc -l numbers.txt               # Should show: 151 (100 + 51 lines)

# 2. Process the data
cat numbers.txt | sort -n | uniq > unique_numbers.txt
cat numbers.txt | sort -n | uniq -d > duplicate_numbers.txt

# Verify results:
wc -l unique_numbers.txt         # Should show: 150 (unique numbers 1-150)
cat duplicate_numbers.txt        # Should show: 50, 51, 52... 100 (duplicates)

# 3. Analysis pipeline
cat numbers.txt | \
    sort -n | \
    uniq -c | \
    sort -nr | \
    head -10 > most_frequent.txt

cat most_frequent.txt
# Expected output (numbers appearing twice first):
#       2 100
#       2 99
#       2 98
#       ... (duplicates with count 2)
#       1 150
#       1 149
#       ... (unique numbers with count 1)

# 4. Save and display
cat numbers.txt | sort -n | tee sorted_numbers.txt | wc -l
# Should show: 151 (line count)
# And create sorted_numbers.txt file

# Verify tee worked:
head sorted_numbers.txt          # Should show: 1, 1, 2, 3, 4... (sorted)
```

## Lab 4.4: System Monitoring (10 minutes)

### Solution:

```bash
# 1. Process monitoring
ps aux | sort -k4 -nr | head -5 > top_memory_processes.txt
cat top_memory_processes.txt
# Expected: Processes sorted by memory usage (column 4), highest first

# Alternative memory sorting:
ps aux --sort=-%mem | head -6 > top_memory_alt.txt

# 2. Disk usage
df -h | grep -v tmpfs > disk_usage.txt
cat disk_usage.txt
# Expected: Filesystem usage without temporary filesystems

# More detailed disk analysis:
df -h | grep -E '^/dev/' > physical_disks.txt
du -h ~ | sort -hr | head -10 > largest_home_dirs.txt

# 3. User activity
who | tee current_users.txt | wc -l
cat current_users.txt
# Shows currently logged-in users and counts them

# Additional user monitoring:
last | head -10 > recent_logins.txt
w > detailed_user_activity.txt

# 4. Command frequency
history | awk '{print $2}' | sort | uniq -c | sort -nr | head -10 > command_frequency.txt
cat command_frequency.txt
# Expected output:
#      25 ls
#      18 cd
#      12 cat
#      10 echo
#       8 grep
#       ... (your most used commands)

# Advanced system monitoring:
netstat -tuln | grep LISTEN > listening_ports.txt
free -h | grep '^Mem:' > memory_status.txt
uptime > system_uptime.txt
```

## Advanced Pipeline Examples:

```bash
# Complex log analysis:
cat /var/log/syslog | grep ERROR | tail -10 > recent_errors.txt

# Network monitoring:
ss -tuln | awk '{print $5}' | cut -d: -f2 | sort -n | uniq -c > port_usage.txt

# System resource summary:
{
    echo "=== System Summary ==="
    echo "Date: $(date)"
    echo "Users: $(who | wc -l)"
    echo "Processes: $(ps aux | wc -l)"
    echo "Memory: $(free -h | grep '^Mem:' | awk '{print $3"/"$2}')"
    echo "Disk: $(df -h / | tail -1 | awk '{print $3"/"$2" ("$5")"}')"
} > system_summary.txt

cat system_summary.txt
```

## Troubleshooting Common Issues:

```bash
# Issue 1: Permission denied
ls /root > root_contents.txt 2>&1    # Capture error
cat root_contents.txt                # Check what happened

# Issue 2: Broken pipe
yes | head -5                        # 'yes' stops when head finishes

# Issue 3: File overwritten accidentally
echo "important" > important.txt
echo "oops" > important.txt          # Overwrites!
# Solution: Use >> for append or set noclobber

# Issue 4: Pipeline fails in middle
cat nonexistent.txt | sort | wc -l   # First command fails, others don't run
# Check exit codes: echo ${PIPESTATUS[@]}

# Issue 5: Special characters in filenames
touch "file with spaces.txt"
cat "file with spaces.txt" | wc -l   # Quotes needed
```

## Verification Commands:

```bash
# Check all lab results:
echo "=== Lab 4.1 Results ==="
ls -la fruits.txt directory_contents.txt errors.txt combined.txt

echo "=== Lab 4.2 Results ==="
cat fruits.txt | sort
ps aux | grep $USER | wc -l

echo "=== Lab 4.3 Results ==="
wc -l numbers.txt unique_numbers.txt duplicate_numbers.txt
head -3 most_frequent.txt

echo "=== Lab 4.4 Results ==="
head -3 top_memory_processes.txt
cat current_users.txt
head -5 command_frequency.txt
```

## Key Learning Points:

1. **Redirection**: `>` (overwrite), `>>` (append), `2>` (errors)
2. **Pipes**: Connect output of one command to input of another
3. **Combining**: `2>&1` combines stderr with stdout
4. **tee**: Save to file AND display on screen
5. **Pipeline**: Chain multiple commands for complex processing

---

## Navigation

**Previous:** [← Lesson 3: Shell Environment and Variables](lesson-03-lab-solutions.md)  
**Next:** [→ Lesson 5: Advanced Commands and Control Operators](lesson-05-lab-solutions.md)  
**Home:** [↑ Solutions Index](README.md)