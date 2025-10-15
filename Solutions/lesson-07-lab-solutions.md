# Solutions for Lesson 7: Text Processing and Filters

## Lab 7.1: grep Practice (15 minutes)

### Solution:

```bash
# 1. Create test data
cat > sample.log << EOF
2024-01-15 10:30:45 INFO User login successful
2024-01-15 10:31:02 ERROR Failed to connect to database
2024-01-15 10:31:15 INFO User logout  
2024-01-15 10:32:30 WARNING Low disk space
2024-01-15 10:33:45 ERROR Database connection timeout
2024-01-15 10:34:12 INFO System backup completed
EOF

# Verify test data:
cat sample.log
wc -l sample.log                        # Should show 6 lines

# 2. Practice grep patterns
grep "ERROR" sample.log
# Output:
# 2024-01-15 10:31:02 ERROR Failed to connect to database
# 2024-01-15 10:33:45 ERROR Database connection timeout

grep -i "error" sample.log              # Case-insensitive search
# Same output as above (since ERROR is already uppercase)

grep -n "INFO" sample.log               # Show line numbers
# Output:
# 1:2024-01-15 10:30:45 INFO User login successful
# 3:2024-01-15 10:31:15 INFO User logout
# 6:2024-01-15 10:34:12 INFO System backup completed

grep -c "2024-01-15" sample.log         # Count matching lines
# Output: 6

grep -v "INFO" sample.log               # Invert match (show non-INFO lines)
# Output:
# 2024-01-15 10:31:02 ERROR Failed to connect to database
# 2024-01-15 10:32:30 WARNING Low disk space
# 2024-01-15 10:33:45 ERROR Database connection timeout

# Additional grep options:
grep -w "User" sample.log               # Whole word match
grep -l "ERROR" sample.log              # Show filename (useful with multiple files)
grep -A 1 "ERROR" sample.log            # Show 1 line after match
grep -B 1 "ERROR" sample.log            # Show 1 line before match
grep -C 1 "ERROR" sample.log            # Show 1 line before and after

# 3. Regular expressions
grep "^2024" sample.log                 # Lines starting with "2024"
# Output: All lines (they all start with 2024)

grep "completed$" sample.log            # Lines ending with "completed"
# Output: 2024-01-15 10:34:12 INFO System backup completed

grep -E "(ERROR|WARNING)" sample.log    # Extended regex: ERROR OR WARNING
# Output:
# 2024-01-15 10:31:02 ERROR Failed to connect to database
# 2024-01-15 10:32:30 WARNING Low disk space
# 2024-01-15 10:33:45 ERROR Database connection timeout

# More regex examples:
grep "10:3[0-5]" sample.log             # Times between 10:30 and 10:35
grep -E "\b[A-Z]{4,}\b" sample.log      # Words with 4+ uppercase letters
grep -E "User (login|logout)" sample.log # User login or logout events
```

## Lab 7.2: Text Processing Pipeline (20 minutes)

### Solution:

```bash
# 1. Create sample data
cat > employees.csv << EOF
Name,Department,Salary,Years
John,IT,50000,5
Jane,HR,45000,3
Bob,IT,55000,7
Alice,Finance,60000,4
Charlie,IT,48000,2
David,HR,52000,6
EOF

# Verify data:
cat employees.csv
echo "Total records: $(wc -l < employees.csv)"  # Should show 7

# 2. Extract information
cut -d, -f1,2 employees.csv             # Names and departments
# Output:
# Name,Department
# John,IT
# Jane,HR
# Bob,IT
# Alice,Finance
# Charlie,IT
# David,HR

cut -d, -f3 employees.csv | grep -v "Salary"  # Salaries only (no header)
# Output:
# 50000
# 45000
# 55000
# 60000
# 48000
# 52000

# Extract specific columns with headers:
head -1 employees.csv && tail -n +2 employees.csv | cut -d, -f1,3
# Shows names and salaries with proper header

# 3. Analysis pipeline
grep -v "^Name" employees.csv | \       # Exclude header
    cut -d, -f2 | \                     # Extract department column
    sort | \                            # Sort departments
    uniq -c | \                         # Count unique departments
    sort -nr                            # Sort by count (highest first)

# Expected output:
#       3 IT
#       2 HR
#       1 Finance

# Alternative analysis methods:
grep -v "^Name" employees.csv | awk -F, '{print $2}' | sort | uniq -c | sort -nr

# 4. Salary analysis
grep -v "^Name" employees.csv | \       # Exclude header
    cut -d, -f3 | \                     # Extract salary column
    sort -n | \                         # Sort numerically
    tail -3                             # Show top 3 salaries

# Expected output:
# 52000
# 55000
# 60000

# More salary analysis:
echo "Lowest salary:"
grep -v "^Name" employees.csv | cut -d, -f3 | sort -n | head -1

echo "Highest salary:"
grep -v "^Name" employees.csv | cut -d, -f3 | sort -n | tail -1

echo "Average salary:"
grep -v "^Name" employees.csv | cut -d, -f3 | \
    awk '{sum+=$1; count++} END {printf "%.2f\n", sum/count}'

# Department-wise analysis:
echo "IT employees:"
grep ",IT," employees.csv | cut -d, -f1,3

echo "Senior employees (5+ years):"
grep -v "^Name" employees.csv | awk -F, '$4 >= 5 {print $1, $4}'
```

## Lab 7.3: Advanced Text Processing (20 minutes)

### Solution:

```bash
# 1. Create mixed data
cat > mixed_data.txt << EOF
User: john@example.com (ID: 1001)
User: jane@company.org (ID: 1002)  
User: bob@test.net (ID: 1003)
Admin: alice@admin.com (ID: 2001)
Guest: temp@guest.tmp (ID: 3001)
EOF

# Verify data:
cat mixed_data.txt

# 2. Extract emails
grep -o '[a-zA-Z0-9._%+-]\+@[a-zA-Z0-9.-]\+\.[a-zA-Z]\{2,\}' mixed_data.txt
# Output:
# john@example.com
# jane@company.org
# bob@test.net
# alice@admin.com
# temp@guest.tmp

# Alternative email extraction methods:
grep -oE '[^[:space:]]+@[^[:space:]]+' mixed_data.txt
awk '{for(i=1;i<=NF;i++) if($i ~ /@/) print $i}' mixed_data.txt | tr -d '()'

# Extract different email domains:
grep -o '@[a-zA-Z0-9.-]\+\.[a-zA-Z]\{2,\}' mixed_data.txt | sort | uniq -c

# 3. Extract and sort IDs
grep -o 'ID: [0-9]\+' mixed_data.txt | \  # Extract ID patterns
    cut -d' ' -f2 | \                      # Get just the numbers
    sort -n                                # Sort numerically

# Expected output:
# 1001
# 1002
# 1003
# 2001
# 3001

# Alternative ID extraction:
sed -n 's/.*ID: \([0-9]\+\).*/\1/p' mixed_data.txt | sort -n
awk '{match($0, /ID: ([0-9]+)/, arr); if(arr[1]) print arr[1]}' mixed_data.txt | sort -n

# 4. Complex transformation
sed 's/User: \([^@]*\)@.*/Username: \1/' mixed_data.txt | \
    grep "Username:"

# Expected output:
# Username: john
# Username: jane
# Username: bob

# More sed transformations:
sed 's/\(User\|Admin\|Guest\): \([^@]*\)@\([^[:space:]]*\).*/Role: \1, Name: \2, Domain: \3/' mixed_data.txt

# Extract role types:
sed 's/^\([^:]*\):.*/\1/' mixed_data.txt | sort | uniq -c

# 5. Character manipulation
tr '[:lower:]' '[:upper:]' < mixed_data.txt | \  # Convert to uppercase
    tr -d '()' | \                               # Remove parentheses
    tr -s ' '                                    # Squeeze multiple spaces

# Expected output: All uppercase, no parentheses, single spaces

# More tr examples:
tr -d '[:digit:]' < mixed_data.txt              # Remove all digits
tr '[:space:]' '_' < mixed_data.txt             # Replace spaces with underscores
tr -c '[:alnum:]\n' ' ' < mixed_data.txt        # Keep only alphanumeric and newlines

# Character analysis:
tr -cd '[:alpha:]' < mixed_data.txt | wc -c     # Count alphabetic characters
tr -cd '[:digit:]' < mixed_data.txt | wc -c     # Count digits
```

## Lab 7.4: System Information Processing (15 minutes)

### Solution:

```bash
# 1. Process running processes
ps aux | head -n 1                      # Show header
# Output: USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND

ps aux | grep -v "^USER" | sort -k4 -nr | head -5  # Top 5 by memory usage
# Shows processes sorted by memory usage (%MEM column)

# Alternative process analysis:
ps aux --sort=-%mem | head -6           # Built-in sort by memory
ps aux --sort=-%cpu | head -6           # Sort by CPU usage

# 2. Memory usage analysis
ps aux | \
    grep -v "^USER" | \                 # Skip header
    awk '{print $4, $11}' | \           # Extract memory % and command
    sort -nr | \                        # Sort by memory (highest first)
    head -10                            # Top 10

# Expected output: Memory percentage and command name pairs

# More detailed memory analysis:
ps aux | awk 'NR>1 {mem+=$4; count++} END {printf "Average memory usage: %.2f%%\n", mem/count}'

# Process count by user:
ps aux | awk 'NR>1 {users[$1]++} END {for(user in users) printf "%s: %d processes\n", user, users[user]}'

# 3. User activity
who | cut -d' ' -f1 | sort | uniq -c
# Shows count of sessions per user

# More user analysis:
who -u                                  # Show detailed user info
w                                       # Show what users are doing
last | head -10                         # Recent login history

# Login session analysis:
who | awk '{print $1}' | sort | uniq | wc -l  # Number of unique users
who | wc -l                             # Total number of sessions

# 4. Disk usage formatting
df -h | grep -v "^Filesystem" | \       # Skip header
    sort -k5 -nr | \                    # Sort by usage percentage
    head -5                             # Top 5 filesystems

# Expected output: Filesystems sorted by usage percentage

# More disk analysis:
df -h | awk 'NR>1 && $5+0 > 80 {print "WARNING: " $1 " is " $5 " full"}'
df -h | grep -E '[0-9]+%' | awk '{gsub(/%/, "", $5); if($5 > 90) print $1 " is critically full at " $5 "%"}'

# Disk space by directory:
du -sh /* 2>/dev/null | sort -hr | head -10

# Find large files:
find /var/log -type f -size +10M 2>/dev/null | head -5
```

## Advanced Text Processing Examples:

```bash
# Complex log analysis:
cat > access.log << EOF
192.168.1.100 - - [15/Oct/2024:10:30:45] "GET /index.html HTTP/1.1" 200 1024
192.168.1.101 - - [15/Oct/2024:10:30:46] "POST /api/login HTTP/1.1" 200 512
192.168.1.100 - - [15/Oct/2024:10:30:47] "GET /dashboard HTTP/1.1" 403 0
192.168.1.102 - - [15/Oct/2024:10:30:48] "GET /images/logo.png HTTP/1.1" 200 2048
EOF

# Extract IP addresses and count requests:
awk '{print $1}' access.log | sort | uniq -c | sort -nr

# Find error responses (4xx, 5xx):
awk '$9 >= 400 {print $0}' access.log

# Extract and analyze request methods:
awk '{print $6}' access.log | tr -d '"' | sort | uniq -c

# Multiple file processing:
find /var/log -name "*.log" -exec grep -l "ERROR" {} \; 2>/dev/null | head -5

# Text statistics:
wc -lwc *.txt 2>/dev/null || echo "No .txt files found"

# Advanced sed operations:
echo "Hello World" | sed 's/\(.*\) \(.*\)/\2, \1/'  # Swap words
echo "line1\nline2\nline3" | sed '2d'               # Delete line 2
echo "apple,banana,cherry" | sed 's/,/\n/g'         # Replace commas with newlines
```

## Real-World Text Processing Scenarios:

```bash
# Configuration file processing:
cat > config.ini << EOF
[database]
host=localhost
port=5432
name=myapp

[cache]
host=redis.local
port=6379
EOF

# Extract all host values:
grep "^host=" config.ini | cut -d= -f2

# Process CSV with mixed delimiters:
echo "name;age,city|country" | tr ';,|' '\t' | cut -f1,3

# Clean up messy data:
echo "  John   Doe  " | sed 's/^[[:space:]]*//;s/[[:space:]]*$//' | tr -s ' '

# Extract URLs from HTML:
echo '<a href="http://example.com">Link</a>' | grep -o 'https\?://[^"]*'
```

## Troubleshooting Common Issues:

```bash
# Issue 1: grep not finding matches
grep "pattern" file.txt                 # Case sensitive
grep -i "pattern" file.txt              # Case insensitive
grep -F "pattern" file.txt              # Literal search (no regex)

# Issue 2: cut with wrong delimiter
echo "a:b:c" | cut -d: -f2              # Correct: uses : as delimiter
echo "a:b:c" | cut -d, -f2              # Wrong: uses , as delimiter (returns whole line)

# Issue 3: tr not working as expected
echo "Hello World" | tr 'a-z' 'A-Z'     # Correct: character sets
echo "Hello World" | tr 'hello' 'HELLO' # Wrong: character mapping

# Issue 4: sort not working numerically
echo -e "10\n2\n1" | sort               # Alphabetical: 1, 10, 2
echo -e "10\n2\n1" | sort -n            # Numerical: 1, 2, 10

# Issue 5: awk field separator
echo "a,b,c" | awk '{print $2}'         # Wrong: default separator is space
echo "a,b,c" | awk -F, '{print $2}'     # Correct: specify comma separator
```

## Verification Commands:

```bash
# Verify all lab results:
echo "=== grep Test ==="
echo "ERROR: test" | grep "ERROR" && echo "grep working"

echo "=== cut Test ==="
echo "a,b,c" | cut -d, -f2              # Should output: b

echo "=== sort Test ==="
echo -e "3\n1\n2" | sort -n             # Should output: 1, 2, 3

echo "=== Pipeline Test ==="
echo -e "apple\nbanana\napple" | sort | uniq -c | sort -nr
# Should show counts with apple first

echo "=== Text Processing ==="
echo "user@domain.com" | grep -o '@.*'  # Should output: @domain.com
```

## Key Learning Points:

1. **grep**: Powerful pattern searching with regex support
2. **cut**: Extract specific columns/fields from structured data
3. **tr**: Character-level transformations and deletions
4. **sort/uniq**: Data organization and deduplication
5. **Pipelines**: Combine multiple tools for complex text processing
6. **awk/sed**: Advanced text manipulation and programming

---

## Navigation

**Previous:** [← Lesson 6: File Operations, Globbing, and Archiving](lesson-06-lab-solutions.md)  
**Next:** [→ Lesson 8: Shell Scripting Fundamentals](lesson-08-lab-solutions.md)  
**Home:** [↑ Solutions Index](README.md)