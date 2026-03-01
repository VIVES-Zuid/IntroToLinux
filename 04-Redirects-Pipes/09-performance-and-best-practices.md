# 9. Performance and Best Practices

### Efficient Pipelines:

- Put filters early in pipeline to reduce data
- Use appropriate tools for each task
- Avoid unnecessary pipes

```bash
# Less efficient
cat file.txt | grep pattern | head -10

## Watch out! Next two commands will have a different output!

# Returns the first 10 lines from file.txt containing the pattern
grep pattern file.txt | head -10

# Return only the line containing the pattern in the first 10 lines of file.txt
head -10 file.txt | grep pattern  # If pattern is common
```

### ℹ️ Memory Considerations:

- Pipes process data in streams (low memory)
- Temporary files use disk space
- Very large datasets may need special handling

### ℹ️ Error Handling:

```bash
# Exit on pipeline failure
set -o pipefail

# Check for command existence
if command -v somecommand > /dev/null; then
    somecommand | process_data
else
    echo "somecommand not found"
fi
```

---

## Navigation

**Next:** [→ Troubleshooting](10-troubleshooting.md)  
**Previous:** [← Advanced Pipe Concepts](08-advanced-pipe-concepts.md)  
**Lesson Home:** [↑ Lesson 4: Redirects & Pipes](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
