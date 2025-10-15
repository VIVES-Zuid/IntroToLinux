# 6. wc: Word, Line, and Character Counting


```bash
# Count lines, words, characters
wc file.txt

# Count only lines
wc -l file.txt

# Count only words
wc -w file.txt

# Count only characters
wc -c file.txt

# Count only bytes
wc -m file.txt

# Multiple files
wc *.txt

# Practical examples
ps aux | wc -l                     # Count running processes
grep "error" log.txt | wc -l       # Count error lines
cat /etc/passwd | wc -l            # Count user accounts
```
---

## Navigation

**Previous:** [← Tr Character Translation And Deletion](05-tr-character-translation-and-deletion.md)  
**Next:** [→ Sort Sorting Text](07-sort-sorting-text.md)  
**Lesson Home:** [↑ Lesson 07: Filters Pipelines](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
