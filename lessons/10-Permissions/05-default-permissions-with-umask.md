# 5. Default Permissions with umask


### Understanding umask
umask defines default permissions for newly created files and directories by specifying which permissions to remove.

```bash
# View current umask
umask
umask -S              # Symbolic display

# Common umask values:
# 022 = removes write for group/others (default for most systems)
# 002 = removes write for others only
# 077 = removes all permissions for group/others
```

### How umask Works:

```bash
# Default permissions without umask:
# Files: 666 (rw-rw-rw-)
# Directories: 777 (rwxrwxrwx)

# With umask 022:
# Files: 666 - 022 = 644 (rw-r--r--)
# Directories: 777 - 022 = 755 (rwxr-xr-x)

# Test umask effects:
umask 022
touch test_file
mkdir test_dir
ls -la test_file test_dir
```

### Setting umask:

```bash
# Set umask for current session
umask 022             # Standard umask
umask 002             # Group-friendly umask
umask 077             # Paranoid umask

# Set permanently in shell configuration
echo "umask 022" >> ~/.bashrc

# Set system-wide default
echo "umask 022" >> /etc/profile
```

### Practical umask Scenarios:

```bash
# Secure environment (personal use)
umask 077             # Only owner has access to new files

# Collaborative environment
umask 002             # Group can read/write, others can read

# Web development
umask 022             # Standard permissions for web files

# Shared development server
umask 002
# Combined with SGID directories for automatic group inheritance
```
---

## Navigation

**Previous:** [← Special Permission Bits](04-special-permission-bits.md)  
**Next:** [→ The Find Command Advanced File Search](06-the-find-command-advanced-file-search.md)  
**Lesson Home:** [↑ Lesson 10: Permissions](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
