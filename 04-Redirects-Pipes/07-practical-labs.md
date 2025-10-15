# 7. Practical Labs


### Lab 4.1: Basic Redirection (10 minutes)

```bash
# 1. Create test data
echo "Apple" > fruits.txt
echo "Banana" >> fruits.txt
echo "Cherry" >> fruits.txt
echo "Date" >> fruits.txt

# 2. Practice output redirection
ls -la > directory_contents.txt
cat directory_contents.txt

# 3. Practice error redirection
ls /nonexistent > output.txt 2> errors.txt
cat output.txt
cat errors.txt

# 4. Combine stdout and stderr
ls /nonexistent fruits.txt > combined.txt 2>&1
cat combined.txt
```

### Lab 4.2: Pipe Practice (15 minutes)

```bash
# 1. Basic pipes
ls | wc -l
ps aux | wc -l
cat /etc/passwd | wc -l

# 2. Text processing
cat fruits.txt | sort
cat fruits.txt | sort | tac

# 3. System information
ps aux | head -1
ps aux | grep $USER

# 4. History analysis
history | tail -20
history | grep ls
history | wc -l
```

### Lab 4.3: Advanced Combinations (15 minutes)

```bash
# 1. Create sample data
seq 1 100 > numbers.txt
seq 50 150 >> numbers.txt

# 2. Process the data
cat numbers.txt | sort -n | uniq > unique_numbers.txt
cat numbers.txt | sort -n | uniq -d > duplicate_numbers.txt

# 3. Analysis pipeline
cat numbers.txt | \
    sort -n | \
    uniq -c | \
    sort -nr | \
    head -10 > most_frequent.txt

cat most_frequent.txt

# 4. Save and display
cat numbers.txt | sort -n | tee sorted_numbers.txt | wc -l
```

### Lab 4.4: System Monitoring (10 minutes)

```bash
# 1. Process monitoring
ps aux | sort -k4 -nr | head -5 > top_memory_processes.txt

# 2. Disk usage
df -h | grep -v tmpfs > disk_usage.txt

# 3. User activity
who | tee current_users.txt | wc -l

# 4. Command frequency
history | awk '{print $2}' | sort | uniq -c | sort -nr | head -10 > command_frequency.txt
```
---

## Navigation

**Previous:** [← Combining Redirection And Pipes](06-combining-redirection-and-pipes.md)  
**Next:** [→ Advanced Pipe Concepts](08-advanced-pipe-concepts.md)  
**Lesson Home:** [↑ Lesson 04: Redirects Pipes](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
