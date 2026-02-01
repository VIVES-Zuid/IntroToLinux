# 7. sort: Sorting Text


```bash
# Basic sort
sort file.txt

# Reverse sort
sort -r file.txt

# Numeric sort
sort -n numbers.txt

# Sort by specific field
sort -k2 file.txt              # Sort by second field
sort -k2,2 file.txt            # Sort by second field only

# Sort with custom delimiter
sort -t: -k3 -n /etc/passwd    # Sort by UID (numeric)

# Remove duplicates while sorting
sort -u file.txt

# Case-insensitive sort
sort -i file.txt

# Human numeric sort (1, 2, 10 instead of 1, 10, 2)
sort -h file.txt
```

### Practical sort Examples:

```bash
# Sort by file size
ls -l | sort -k5 -n            # Sort by size (ascending)
ls -l | sort -k5 -nr           # Sort by size (descending)

# Sort by memory usage
ps aux | sort -k4 -nr | head -10

# Sort CSV by column
sort -t, -k2 data.csv

# Sort by date (assuming ISO format)
sort -k1 logfile.txt
```
---

## Navigation

**Next:** [→ Uniq Identify Unique And Duplicate Lines](08-uniq-identify-unique-and-duplicate-lines.md)  
**Previous:** [← Wc Word Line And Character Counting](06-wc-word-line-and-character-counting.md)  
**Lesson Home:** [↑ Lesson 7: Filters & Pipelines](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
