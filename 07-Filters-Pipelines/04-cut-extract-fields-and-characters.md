# 4. cut: Extract Fields and Characters


cut extracts specific columns or character ranges from text.

### Cut by Character Position:

```bash
# Extract specific characters
cut -c1-5 file.txt             # Characters 1-5
cut -c1,3,5 file.txt           # Characters 1, 3, and 5
cut -c10- file.txt             # From character 10 to end
cut -c-10 file.txt             # First 10 characters

# Examples
echo "Hello World" | cut -c1-5  # "Hello"
echo "2024-01-15" | cut -c1-4   # "2024"
```

### Cut by Fields (Delimiter-based):

```bash
# Default delimiter is TAB
cut -f1 file.txt               # First field
cut -f1,3 file.txt             # Fields 1 and 3
cut -f2- file.txt              # From field 2 to end

# Custom delimiter
cut -d: -f1 /etc/passwd        # Extract usernames
cut -d: -f1,3 /etc/passwd      # Username and UID
cut -d, -f2 data.csv           # Second column from CSV

# Space delimiter
cut -d' ' -f1 file.txt
```

### Practical cut Examples:

```bash
# Extract usernames from /etc/passwd
cut -d: -f1 /etc/passwd

# Get IP addresses from network output
ip addr | grep "inet " | cut -d' ' -f6

# Extract domains from URLs
echo "https://www.example.com/path" | cut -d/ -f3

# Process CSV data
cut -d, -f1,3 data.csv         # First and third columns
cut -d, -f2- data.csv          # All columns except first

# Extract date components
date | cut -d' ' -f1-3         # Day, month, date
echo "2024-01-15" | cut -d- -f1 # Year
```
---

## Navigation

**Next:** [→ Tr Character Translation And Deletion](05-tr-character-translation-and-deletion.md)  
**Previous:** [← Grep Pattern Matching Master](03-grep-pattern-matching-master.md)  
**Lesson Home:** [↑ Lesson 7: Filters & Pipelines](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
