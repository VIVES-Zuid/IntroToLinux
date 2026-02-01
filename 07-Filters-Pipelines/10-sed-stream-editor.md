# 10. sed: Stream Editor


sed is a powerful stream editor for filtering and transforming text.

### Basic sed Operations:

```bash
# Substitute (replace)
sed 's/old/new/' file.txt         # Replace first occurrence per line
sed 's/old/new/g' file.txt        # Replace all occurrences (global)
sed 's/old/new/2' file.txt        # Replace second occurrence per line

# Case-insensitive substitution
sed 's/old/new/gi' file.txt

# Delete lines
sed '/pattern/d' file.txt         # Delete lines containing pattern
sed '1d' file.txt                 # Delete first line
sed '$d' file.txt                 # Delete last line
sed '1,5d' file.txt               # Delete lines 1-5

# Print specific lines
sed -n '1,5p' file.txt            # Print lines 1-5
sed -n '/pattern/p' file.txt      # Print lines matching pattern
```

### Advanced sed Examples:

```bash
# Multiple substitutions
sed -e 's/old1/new1/g' -e 's/old2/new2/g' file.txt

# In-place editing
sed -i 's/old/new/g' file.txt     # Modify file directly
sed -i.bak 's/old/new/g' file.txt # Keep backup as file.txt.bak

# Use different delimiter
sed 's|/old/path|/new/path|g' file.txt

# Add text
sed '1i\New first line' file.txt  # Insert before line 1
sed '$a\New last line' file.txt   # Append after last line
```
---

## Navigation

**Next:** [→ Building Powerful Pipelines](11-building-powerful-pipelines.md)  
**Previous:** [← Comm Compare Sorted Files](09-comm-compare-sorted-files.md)  
**Lesson Home:** [↑ Lesson 7: Filters & Pipelines](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
