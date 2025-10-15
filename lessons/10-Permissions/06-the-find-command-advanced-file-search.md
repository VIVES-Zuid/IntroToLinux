# 6. The find Command: Advanced File Search


### Basic find Syntax:
```bash
find [path] [options] [tests] [actions]
```

### Finding by Name:

```bash
# Find exact name
find /home -name "filename.txt"

# Case-insensitive name search
find /home -iname "FILENAME.TXT"

# Wildcard patterns
find /home -name "*.txt"
find /home -name "file*"
find /home -name "test?.log"

# Multiple directories
find /home /tmp -name "*.conf"
```

### Finding by Type:

```bash
# Find files by type
find /home -type f        # Regular files
find /home -type d        # Directories
find /home -type l        # Symbolic links
find /home -type c        # Character devices
find /home -type b        # Block devices

# Combine type and name
find /var/log -type f -name "*.log"
```

### Finding by Size:

```bash
# Find by exact size
find /home -size 100c     # Exactly 100 bytes
find /home -size 1M       # Exactly 1 megabyte

# Find by size range
find /home -size +1M      # Larger than 1MB
find /home -size -1k      # Smaller than 1KB
find /home -size +10M -size -100M  # Between 10MB and 100MB

# Size units: c (bytes), k (KB), M (MB), G (GB)
```

### Finding by Time:

```bash
# Find by modification time
find /home -mtime -7      # Modified in last 7 days
find /home -mtime +30     # Modified more than 30 days ago
find /home -mtime 1       # Modified exactly 1 day ago

# Find by access time
find /home -atime -1      # Accessed in last day

# Find by change time (metadata)
find /home -ctime -1      # Changed in last day

# Using minutes instead of days
find /tmp -mmin -60       # Modified in last 60 minutes
find /tmp -amin +120      # Accessed more than 120 minutes ago
```

### Finding by Permissions:

```bash
# Find by exact permissions
find /home -perm 644      # Exactly 644 permissions
find /home -perm u=rw,g=r,o=r

# Find with at least these permissions
find /home -perm -644     # At least 644 permissions

# Find with any of these permissions
find /home -perm /644     # Any of 644 permissions

# Find executable files
find /home -perm -111     # Executable by someone
find /home -executable    # Executable by current user

# Find writable files
find /home -writable      # Writable by current user
```

### Finding by Owner:

```bash
# Find by user
find /home -user john
find /home -uid 1000

# Find by group
find /home -group developers
find /home -gid 100

# Find files with no valid owner
find /home -nouser
find /home -nogroup
```

### Actions with find:

```bash
# Execute commands on found files
find /tmp -name "*.tmp" -delete
find /home -name "*.bak" -exec rm {} \;
find /var/log -name "*.log" -exec gzip {} \;

# Confirm before action
find /tmp -name "*.tmp" -ok rm {} \;

# Execute with multiple files (more efficient)
find /home -name "*.txt" -exec ls -l {} +

# Print with custom format
find /home -name "*.conf" -printf "%p %s bytes\n"

# Copy found files
find /home -name "*.jpg" -exec cp {} /backup/images/ \;
```

### Complex find Examples:

```bash
# Find large files in /var
find /var -type f -size +100M -exec ls -lh {} \;

# Find and remove old log files
find /var/log -name "*.log" -mtime +30 -delete

# Find SUID files (security audit)
find /usr -perm -4000 -type f -exec ls -l {} \;

# Find world-writable files (security risk)
find /home -perm -002 -type f -exec ls -l {} \;

# Find empty files and directories
find /tmp -empty -delete

# Find duplicate files by name
find /home -name "*.txt" -exec basename {} \; | sort | uniq -d

# Complex permission search
find /opt -type f \( -perm -4000 -o -perm -2000 \) -exec ls -l {} \;

# Find files owned by deleted users
find /home -nouser -exec ls -l {} \;
```
---

## Navigation

**Previous:** [← Default Permissions With Umask](05-default-permissions-with-umask.md)  
**Next:** [→ System Tools](07-system-tools.md)  
**Lesson Home:** [↑ Lesson 10: Permissions](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
