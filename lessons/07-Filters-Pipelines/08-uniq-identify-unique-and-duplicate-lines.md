# 8. uniq: Identify Unique and Duplicate Lines


uniq works on adjacent duplicate lines, so input should usually be sorted first.

```bash
# Remove duplicate lines
sort file.txt | uniq

# Count occurrences
sort file.txt | uniq -c

# Show only duplicates
sort file.txt | uniq -d

# Show only unique lines (no duplicates)
sort file.txt | uniq -u

# Ignore case
sort file.txt | uniq -i

# Skip fields
sort file.txt | uniq -f1        # Skip first field when comparing
```

### Practical uniq Examples:

```bash
# Find most frequently used commands
history | awk '{print $2}' | sort | uniq -c | sort -nr | head -10

# Count unique users
cut -d: -f1 /etc/passwd | sort | uniq | wc -l

# Find duplicate files by name
find . -type f -exec basename {} \; | sort | uniq -d

# Analyze log patterns
grep "ERROR" log.txt | cut -d' ' -f3 | sort | uniq -c
```
---

## Navigation

**Previous:** [← Sort Sorting Text](07-sort-sorting-text.md)  
**Next:** [→ Comm Compare Sorted Files](09-comm-compare-sorted-files.md)  
**Lesson Home:** [↑ Lesson 07: Filters Pipelines](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
