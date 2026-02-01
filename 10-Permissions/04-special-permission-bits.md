# 4. Special Permission Bits


### Set User ID (SUID) - 4000
When set on executable files, the program runs with the owner's privileges instead of the executor's.

```bash
# Set SUID bit
chmod 4755 program        # or chmod u+s program

# Example: passwd command
ls -l /usr/bin/passwd
# -rwsr-xr-x root root /usr/bin/passwd

# Users can change their password because passwd runs as root
```

### Set Group ID (SGID) - 2000
On files: Program runs with group's privileges
On directories: New files inherit the directory's group

```bash
# Set SGID bit on file
chmod 2755 program        # or chmod g+s program

# Set SGID bit on directory (very useful!)
chmod 2775 shared_dir/    # New files inherit group
chmod g+s shared_dir/

# Example: shared project directory
sudo mkdir /opt/project
sudo chgrp developers /opt/project
sudo chmod 2775 /opt/project
# Now all files created in /opt/project belong to 'developers' group
```

### Sticky Bit - 1000
On directories: Only owner can delete their own files (like /tmp)

```bash
# Set sticky bit
chmod 1777 directory/     # or chmod +t directory/

# Example: /tmp directory
ls -ld /tmp
# drwxrwxrwt root root /tmp

# Users can create files but only delete their own
```

### Viewing Special Bits:

```bash
# Look for special characters in permissions
ls -l file_with_suid      # -rwsr-xr-x (s in user execute)
ls -l file_with_sgid      # -rwxr-sr-x (s in group execute)
ls -ld dir_with_sticky    # drwxrwxrwt (t in other execute)

# Find files with special bits
find /usr -perm -4000 2>/dev/null    # SUID files
find /usr -perm -2000 2>/dev/null    # SGID files
find / -perm -1000 -type d 2>/dev/null # Sticky bit directories
```
---

## Navigation

**Next:** [→ Default Permissions With Umask](05-default-permissions-with-umask.md)  
**Previous:** [← Ownership With Chown And Chgrp](03-ownership-with-chown-and-chgrp.md)  
**Lesson Home:** [↑ Lesson 10: Permissions](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
