# 2. The chmod Command


### Changing Permissions with chmod

#### Symbolic Mode:
```bash
# Add permissions
chmod u+x file.txt        # Add execute for user
chmod g+w file.txt        # Add write for group
chmod o+r file.txt        # Add read for others
chmod a+x file.txt        # Add execute for all

# Remove permissions
chmod u-x file.txt        # Remove execute for user
chmod g-w file.txt        # Remove write for group
chmod o-r file.txt        # Remove read for others

# Set exact permissions
chmod u=rw file.txt       # User: read+write only
chmod g=r file.txt        # Group: read only
chmod o= file.txt         # Others: no permissions

# Complex combinations
chmod u+rw,g+r,o-rwx file.txt
chmod ug+rw,o= file.txt
```

#### Octal Mode:
```bash
# Set exact permissions with numbers
chmod 755 file.txt        # rwxr-xr-x
chmod 644 file.txt        # rw-r--r--
chmod 600 file.txt        # rw-------
chmod 777 file.txt        # rwxrwxrwx (dangerous!)

# Apply to directories recursively
chmod -R 755 directory/
chmod -R u+x,g+x directory/
```

### Common Permission Patterns:

```bash
# Executable scripts
chmod 755 script.sh       # Owner can modify, all can execute

# Private files
chmod 600 private.txt     # Only owner can read/write

# Shared files
chmod 664 shared.txt      # Owner/group can modify, others read

# Public directories
chmod 755 public_dir/     # All can enter and list

# Private directories
chmod 700 private_dir/    # Only owner can access

# Shared project directory
chmod 2775 project_dir/   # Group sticky bit + group write
```

### Practical chmod Examples:

```bash
# Make script executable
chmod +x script.sh

# Secure SSH keys
chmod 600 ~/.ssh/id_rsa
chmod 644 ~/.ssh/id_rsa.pub
chmod 700 ~/.ssh/

# Web server files
chmod 644 *.html          # Web pages
chmod 755 cgi-bin/        # CGI directory
chmod 755 *.cgi           # CGI scripts

# Backup files (read-only)
chmod 444 backup.tar.gz

# Log files (append only for group)
chmod 664 /var/log/application.log
```
---

## Navigation

**Previous:** [← Understanding Linux File Permissions](01-understanding-linux-file-permissions.md)  
**Next:** [→ Ownership With Chown And Chgrp](03-ownership-with-chown-and-chgrp.md)  
**Lesson Home:** [↑ Lesson 10: Permissions](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
