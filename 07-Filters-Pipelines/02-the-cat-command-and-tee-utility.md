# 2. The cat Command and tee Utility


### cat: Display and Concatenate Files

```bash
# Display file contents
cat file.txt

# Display multiple files
cat file1.txt file2.txt file3.txt

# Concatenate files into new file
cat file1.txt file2.txt > combined.txt

# Number lines
cat -n file.txt

# Show line endings and tabs
cat -A file.txt

# Remove blank lines
cat -s file.txt              # Squeeze multiple blank lines to one

# Create file with cat (Ctrl+D to end)
cat > newfile.txt
This is content
More content
^D
```

### tee: Split Output Stream

```bash
# Save to file AND display
command | tee output.txt

# Save to multiple files
command | tee file1.txt file2.txt

# Append to file
command | tee -a logfile.txt

# Practical examples
ps aux | tee processes.txt | grep firefox
ls -la | tee directory_listing.txt | wc -l
cat /etc/passwd | tee users_backup.txt | head -5
```
---

## Navigation

**Next:** [→ Grep Pattern Matching Master](03-grep-pattern-matching-master.md)  
**Previous:** [← Understanding Filters](01-understanding-filters.md)  
**Lesson Home:** [↑ Lesson 7: Filters & Pipelines](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
