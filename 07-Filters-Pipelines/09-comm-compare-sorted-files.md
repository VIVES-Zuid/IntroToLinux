# 9. comm: Compare Sorted Files


comm compares two sorted files line by line.

```bash
# Prepare sorted files
sort file1.txt > sorted1.txt
sort file2.txt > sorted2.txt

# Compare files (3 columns: unique to file1, unique to file2, common)
comm sorted1.txt sorted2.txt

# Show only lines unique to first file
comm -23 sorted1.txt sorted2.txt

# Show only lines unique to second file
comm -13 sorted1.txt sorted2.txt

# Show only common lines
comm -12 sorted1.txt sorted2.txt
```
---

## Navigation

**Next:** [→ Sed Stream Editor](10-sed-stream-editor.md)  
**Previous:** [← Uniq Identify Unique And Duplicate Lines](08-uniq-identify-unique-and-duplicate-lines.md)  
**Lesson Home:** [↑ Lesson 7: Filters & Pipelines](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
