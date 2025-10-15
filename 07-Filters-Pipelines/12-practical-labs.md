# 12. Practical Labs


### Lab 7.1: grep Practice (15 minutes)

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

# 2. Practice grep patterns
grep "ERROR" sample.log
grep -i "error" sample.log
grep -n "INFO" sample.log
grep -c "2024-01-15" sample.log
grep -v "INFO" sample.log

# 3. Regular expressions
grep "^2024" sample.log
grep "completed$" sample.log
grep -E "(ERROR|WARNING)" sample.log
```

### Lab 7.2: Text Processing Pipeline (20 minutes)

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

# 2. Extract information
cut -d, -f1,2 employees.csv
cut -d, -f3 employees.csv | grep -v "Salary"

# 3. Analysis pipeline
grep -v "^Name" employees.csv | \
    cut -d, -f2 | \
    sort | \
    uniq -c | \
    sort -nr

# 4. Salary analysis
grep -v "^Name" employees.csv | \
    cut -d, -f3 | \
    sort -n | \
    tail -3
```

### Lab 7.3: Advanced Text Processing (20 minutes)

```bash
# 1. Create mixed data
cat > mixed_data.txt << EOF
User: john@example.com (ID: 1001)
User: jane@company.org (ID: 1002)  
User: bob@test.net (ID: 1003)
Admin: alice@admin.com (ID: 2001)
Guest: temp@guest.tmp (ID: 3001)
EOF

# 2. Extract emails
grep -o '[a-zA-Z0-9._%+-]\+@[a-zA-Z0-9.-]\+\.[a-zA-Z]\{2,\}' mixed_data.txt

# 3. Extract and sort IDs
grep -o 'ID: [0-9]\+' mixed_data.txt | \
    cut -d' ' -f2 | \
    sort -n

# 4. Complex transformation
sed 's/User: \([^@]*\)@.*/Username: \1/' mixed_data.txt | \
    grep "Username:"

# 5. Character manipulation
tr '[:lower:]' '[:upper:]' < mixed_data.txt | \
    tr -d '()' | \
    tr -s ' '
```

### Lab 7.4: System Information Processing (15 minutes)

```bash
# 1. Process running processes
ps aux | head -n 1
ps aux | grep -v "^USER" | sort -k4 -nr | head -5

# 2. Memory usage analysis
ps aux | \
    grep -v "^USER" | \
    awk '{print $4, $11}' | \
    sort -nr | \
    head -10

# 3. User activity
who | cut -d' ' -f1 | sort | uniq -c

# 4. Disk usage formatting
df -h | grep -v "^Filesystem" | \
    sort -k5 -nr | \
    head -5
```
---

## Navigation

**Previous:** [← Building Powerful Pipelines](11-building-powerful-pipelines.md)  
**Next:** [→ Best Practices](13-best-practices.md)  
**Lesson Home:** [↑ Lesson 07: Filters Pipelines](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
