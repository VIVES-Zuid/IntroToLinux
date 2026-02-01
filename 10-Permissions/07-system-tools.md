# 7. System Tools


### locate Command: Fast File Finding

```bash
# Install locate (if needed)
sudo apt install mlocate

# Update locate database
sudo updatedb

# Find files quickly
locate filename
locate "*.conf"
locate -i filename    # Case-insensitive

# Limit results
locate -n 10 filename

# Show only existing files
locate -e filename

# Search in specific directory
locate -d /home filename
```

### Date and Time Commands:

```bash
# Display current date and time
date

# Format date output
date "+%Y-%m-%d %H:%M:%S"
date "+%A, %B %d, %Y"

# Set date (requires root)
sudo date "2024-01-15 14:30:00"

# Show date in UTC
date -u

# Show date from timestamp
date -d @1642262400
date -d "2024-01-15"
date -d "yesterday"
date -d "next monday"

# Calculate date differences
date -d "2024-01-15" "+%s"    # Convert to timestamp
```

### Calendar Commands:

```bash
# Show current month calendar
cal

# Show specific month
cal 1 2024              # January 2024
cal january 2024

# Show entire year
cal 2024

# Show three months (previous, current, next)
cal -3

# Highlight today
cal -h                  # Some systems
```

### Sleep and Timing:

```bash
# Pause execution
sleep 5                 # 5 seconds
sleep 1m                # 1 minute
sleep 1h                # 1 hour
sleep 1d                # 1 day

# Time command execution
time ls -la
time find / -name "*.txt" 2>/dev/null

# Show execution time breakdown
/usr/bin/time -v ls -la
```
---

## Navigation

**Next:** [→ Practical Labs](08-practical-labs.md)  
**Previous:** [← The Find Command Advanced File Search](06-the-find-command-advanced-file-search.md)  
**Lesson Home:** [↑ Lesson 10: Permissions](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
