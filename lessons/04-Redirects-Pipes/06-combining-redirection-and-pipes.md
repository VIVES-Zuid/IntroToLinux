# 6. Combining Redirection and Pipes


### Complex Examples:

```bash
# Save command output to file and display
command | tee output.txt
ps aux | tee processes.txt | grep firefox

# Process data and save intermediate results
cat data.txt | sort | tee sorted.txt | uniq > unique.txt

# Error handling with pipes
ls /nonexistent 2>&1 | grep "No such file"

# Multiple redirections
(command1; command2) > combined_output.txt 2>&1
```

### Practical Scenarios:

```bash
# System monitoring pipeline
ps aux | awk '{print $4, $11}' | sort -nr | head -10 > top_memory.txt

# Log analysis
grep "ERROR" /var/log/application.log | \
    cut -d' ' -f1-3 | \
    sort | uniq -c | \
    sort -nr > error_summary.txt

# Text processing pipeline
cat document.txt | \
    tr '[:upper:]' '[:lower:]' | \
    sed 's/[^a-z ]//g' | \
    tr ' ' '\n' | \
    sort | \
    uniq -c | \
    sort -nr | \
    head -20 > word_frequency.txt
```
---

## Navigation

**Previous:** [← Text Processing Commands](05-text-processing-commands.md)  
**Next:** [→ Practical Labs](07-practical-labs.md)  
**Lesson Home:** [↑ Lesson 04: Redirects Pipes](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
