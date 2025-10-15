# Solutions for Lesson 10: File Permissions and Tools

## Lab 10.1: Permission Management (20 minutes)

### Solution:

```bash
# 1. Create test environment
mkdir ~/permission_lab
cd ~/permission_lab
mkdir private shared public
touch private/secret.txt shared/team.txt public/readme.txt

# Create some content in the files:
echo "Top secret information" > private/secret.txt
echo "Team collaboration file" > shared/team.txt
echo "Public documentation" > public/readme.txt

# Check initial permissions:
ls -la
ls -la private/ shared/ public/

# 2. Set different permission levels

# Private directory and file (owner only):
chmod 700 private/                    # rwx------
chmod 600 private/secret.txt         # rw-------

# Shared directory and file (owner and group):
chmod 770 shared/                     # rwxrwx---
chmod 660 shared/team.txt            # rw-rw----

# Public directory and file (everyone can read):
chmod 755 public/                     # rwxr-xr-x
chmod 644 public/readme.txt          # rw-r--r--

# 3. Verify permissions
echo "=== Permission Verification ==="
ls -la
echo
echo "Private directory contents:"
ls -la private/
echo
echo "Shared directory contents:"
ls -la shared/
echo
echo "Public directory contents:"
ls -la public/

# Test access with different permission levels:
echo
echo "=== Access Testing ==="

# Test private file access:
echo "Reading private file as owner:"
cat private/secret.txt

# Test public file access:
echo "Reading public file:"
cat public/readme.txt

# Show numeric permissions:
echo
echo "=== Numeric Permissions ==="
stat -c "%a %n" private/ private/secret.txt
stat -c "%a %n" shared/ shared/team.txt
stat -c "%a %n" public/ public/readme.txt

# 4. Test access (create test user if possible)
# Note: This requires sudo privileges and may not work in all environments

echo
echo "=== Creating Test User for Access Testing ==="
if sudo useradd -m testuser 2>/dev/null; then
    echo "Test user created successfully"
    
    # Test access as different user:
    echo "Testing access as testuser:"
    
    # This should fail (permission denied):
    sudo -u testuser cat private/secret.txt 2>&1 || echo "Expected: Permission denied for private file"
    
    # This should work:
    sudo -u testuser cat public/readme.txt 2>&1 && echo "Success: Public file accessible"
    
    # Clean up test user:
    sudo userdel -r testuser 2>/dev/null
    echo "Test user cleaned up"
else
    echo "Cannot create test user (sudo required), skipping access tests"
fi

# Additional permission examples:
echo
echo "=== Additional Permission Examples ==="

# Create files with specific permissions using umask:
umask 077                             # Very restrictive umask
touch restrictive_file.txt
ls -la restrictive_file.txt          # Should show 600 permissions

umask 022                             # Standard umask
touch standard_file.txt
ls -la standard_file.txt             # Should show 644 permissions

# Set permissions using symbolic notation:
chmod u+x shared/team.txt             # Add execute for user
chmod g-w shared/team.txt             # Remove write for group
chmod o+r private/secret.txt          # Add read for others (not recommended!)
chmod o-r private/secret.txt          # Remove read for others

# Show the changes:
ls -la shared/team.txt private/secret.txt

# Reset to original permissions:
chmod 660 shared/team.txt
chmod 600 private/secret.txt

echo "Permissions reset to original values"
```

## Lab 10.2: Special Permissions (15 minutes)

### Solution:

```bash
# 1. Create shared project directory
echo "=== Setting up Shared Project Directory ==="

# Create the directory (requires sudo):
sudo mkdir -p /opt/shared_project

# Create developers group if it doesn't exist:
sudo groupadd developers 2>/dev/null || echo "Developers group already exists"

# Set group ownership:
sudo chgrp developers /opt/shared_project

# 2. Set SGID bit for automatic group inheritance
sudo chmod 2775 /opt/shared_project    # SGID + rwxrwxr-x

# Verify the SGID bit is set:
ls -ld /opt/shared_project
# Should show: drwxrwsr-x ... /opt/shared_project
# Note the 's' in the group execute position

# Show numeric representation:
stat -c "%a %n" /opt/shared_project    # Should show 2775

# 3. Test the setup
echo "=== Testing SGID Functionality ==="

# Add current user to developers group (for testing):
sudo usermod -aG developers $USER

# Change to the directory:
cd /opt/shared_project

# Create a test file:
touch test_file.txt
echo "Test content" > test_file.txt

# Check the file's group ownership:
ls -la test_file.txt
# The file should automatically inherit the 'developers' group

# Verify group inheritance:
stat -c "File: %n, Group: %G" test_file.txt

# Create a subdirectory to test directory inheritance:
mkdir test_subdir
ls -ld test_subdir
# Subdirectory should also have 'developers' group and SGID bit

# 4. Create sticky bit directory
echo
echo "=== Setting up Sticky Bit Directory ==="

mkdir ~/temp_shared
chmod 1777 ~/temp_shared               # Sticky bit + rwxrwxrwx

# Verify sticky bit is set:
ls -ld ~/temp_shared
# Should show: drwxrwxrwt ... temp_shared
# Note the 't' at the end (sticky bit)

# Show numeric representation:
stat -c "%a %n" ~/temp_shared          # Should show 1777

# Test sticky bit functionality:
echo "=== Testing Sticky Bit ==="

cd ~/temp_shared

# Create files as current user:
touch user1_file.txt
echo "User 1 content" > user1_file.txt

# In a real scenario, other users could create files here,
# but only file owners could delete their own files
echo "Sticky bit prevents users from deleting others' files"

# Show file ownership:
ls -la user1_file.txt

# Additional special permission examples:
echo
echo "=== Additional Special Permissions ==="

cd ~/permission_lab

# SUID example (be very careful with this in real scenarios):
echo '#!/bin/bash' > test_suid.sh
echo 'echo "Running as: $(whoami)"' >> test_suid.sh
echo 'echo "Real user: $USER"' >> test_suid.sh
chmod 4755 test_suid.sh               # SUID + rwxr-xr-x

ls -la test_suid.sh
# Should show: -rwsr-xr-x ... test_suid.sh
# Note the 's' in the user execute position

# Test SUID script (results may vary based on system configuration):
./test_suid.sh

# Remove SUID bit for security:
chmod 755 test_suid.sh

# Show all special permissions at once:
echo
echo "=== Special Permission Summary ==="
echo "SUID (4000): Runs with file owner's privileges"
echo "SGID (2000): Runs with file group's privileges (files) or inherits group (directories)"
echo "Sticky (1000): Only file owner can delete files in directory"

# Create examples of each:
mkdir special_examples
cd special_examples

# SGID on file (less common):
echo '#!/bin/bash' > sgid_script.sh
echo 'echo "Group ID: $(id -g)"' >> sgid_script.sh
chmod 2755 sgid_script.sh

# Combined special permissions:
touch combined_file
chmod 6644 combined_file               # SUID + SGID
chmod 7644 combined_file               # SUID + SGID + Sticky (unusual for files)

ls -la sgid_script.sh combined_file

# Clean up:
cd ~/permission_lab
rm -rf special_examples
```

## Lab 10.3: find Command Practice (25 minutes)

### Solution:

```bash
# 1. Create test files
echo "=== Creating Test Environment ==="

mkdir -p ~/find_lab/{docs,images,scripts,logs}

# Create various files:
touch ~/find_lab/docs/file{1..5}.txt
touch ~/find_lab/images/photo{1..3}.jpg
touch ~/find_lab/scripts/script{1..2}.sh
touch ~/find_lab/logs/app.log ~/find_lab/logs/error.log

# Create some additional test files:
touch ~/find_lab/docs/manual.pdf
touch ~/find_lab/docs/readme.md
touch ~/find_lab/images/image{4..6}.png
touch ~/find_lab/scripts/backup.py
touch ~/find_lab/logs/access.log
touch ~/find_lab/logs/debug.log

# Add content to some files:
echo "This is a text document" > ~/find_lab/docs/file1.txt
echo "#!/bin/bash" > ~/find_lab/scripts/script1.sh
echo "echo 'Hello World'" >> ~/find_lab/scripts/script1.sh
echo "Error: Database connection failed" > ~/find_lab/logs/error.log
echo "INFO: Application started" > ~/find_lab/logs/app.log

# 2. Make some files executable
chmod +x ~/find_lab/scripts/*.sh
chmod +x ~/find_lab/scripts/*.py

# Verify the setup:
echo "Test environment created:"
tree ~/find_lab 2>/dev/null || find ~/find_lab -type f | sort

# 3. Practice find commands
echo
echo "=== Basic find Commands ==="

cd ~/find_lab

# Find all .txt files:
echo "All .txt files:"
find . -name "*.txt"

# Find all .txt files with full path:
find $(pwd) -name "*.txt"

# Find executable files:
echo
echo "Executable files:"
find . -executable -type f

# Alternative way to find executable files:
find . -type f -perm -111

# Find files larger than 0 bytes:
echo
echo "Files with content:"
find . -size +0c

# Find empty files:
echo
echo "Empty files:"
find . -size 0

# Find files by type:
echo
echo "Regular files only:"
find . -type f | wc -l

echo "Directories only:"
find . -type d

# 4. Advanced find operations
echo
echo "=== Advanced find Commands ==="

# Find and list with details:
echo "Detailed listing of .log files:"
find . -name "*.log" -exec ls -la {} \;

# Find and show file content:
echo
echo "Content of .log files:"
find . -name "*.log" -exec echo "=== {} ===" \; -exec cat {} \;

# Find with multiple criteria:
echo
echo "Text files larger than 10 bytes:"
find . -name "*.txt" -size +10c

# Find files by permissions:
echo
echo "Files with 755 permissions:"
find . -type f -perm 755

# Find files with specific permission bits:
echo
echo "Files writable by owner:"
find . -type f -perm -u+w

# 5. Time-based searches
echo
echo "=== Time-based Searches ==="

# Create files with different timestamps:
touch -t 202401150800 ~/find_lab/old_file.txt
touch ~/find_lab/new_file.txt

# Find files modified in last minute:
echo "Files modified in last minute:"
find . -type f -mmin -1

# Find files modified in last hour:
echo "Files modified in last hour:"
find . -type f -mtime -1

# Find files older than 1 day (none in our test):
echo "Files older than 1 day:"
find . -type f -mtime +1

# Find files by access time:
echo "Files accessed today:"
find . -type f -atime 0

# 6. Complex searches with actions
echo
echo "=== Complex find Operations ==="

# Find and execute multiple actions:
echo "Setting executable permissions on shell scripts:"
find . -name "*.sh" -exec chmod 755 {} \;

# Verify the change:
find . -name "*.sh" -exec ls -la {} \;

# Find and copy files:
mkdir backup
echo "Backing up log files:"
find . -name "*.log" -exec cp {} backup/ \;
ls -la backup/

# Find and count:
echo "Number of .txt files:"
find . -name "*.txt" | wc -l

# Find with size formatting:
echo "Files with sizes:"
find . -type f -exec echo "{} : $(stat -c%s {} 2>/dev/null || echo 0) bytes" \;

# Find and sort by size:
echo
echo "Files sorted by size:"
find . -type f -exec stat -c "%s %n" {} \; | sort -n

# 7. Using find with xargs
echo
echo "=== Using find with xargs ==="

# More efficient for many files:
echo "File types using xargs:"
find . -type f | xargs file

# Safe xargs with null delimiters:
echo "Safe xargs with unusual filenames:"
find . -type f -print0 | xargs -0 ls -la | head -5

# Count lines in all text files:
echo "Total lines in all .txt files:"
find . -name "*.txt" | xargs wc -l

# 8. Complex boolean operations
echo
echo "=== Boolean Operations ==="

# OR operation:
echo "Files ending in .txt OR .log:"
find . \( -name "*.txt" -o -name "*.log" \)

# AND operation:
echo "Executable .sh files:"
find . -name "*.sh" -executable

# NOT operation:
echo "Files NOT ending in .txt:"
find . -type f ! -name "*.txt"

# Complex combination:
echo "Large executable files OR small text files:"
find . \( -type f -executable -size +50c \) -o \( -name "*.txt" -size -100c \)
```

## Lab 10.4: Security Audit (15 minutes)

### Solution:

```bash
# 1. Find potential security issues
echo "=== Security Audit ==="

# Find SUID files (run with owner's privileges):
echo "SUID files in /usr:"
sudo find /usr -perm -4000 -type f 2>/dev/null | head -10

# Find SGID files:
echo
echo "SGID files in /usr:"
sudo find /usr -perm -2000 -type f 2>/dev/null | head -10

# Find files with both SUID and SGID:
echo
echo "Files with both SUID and SGID:"
sudo find /usr -perm -6000 -type f 2>/dev/null

# Find world-writable files (security risk):
echo
echo "World-writable files in /tmp:"
find /tmp -perm -002 -type f 2>/dev/null | head -10

# Find world-writable directories without sticky bit:
echo
echo "World-writable directories without sticky bit:"
find /tmp -type d -perm -002 ! -perm -1000 2>/dev/null

# Find files with no owner (orphaned files):
echo
echo "Files with no owner in /home:"
sudo find /home -nouser 2>/dev/null | head -5

# Find files with no group:
echo
echo "Files with no group:"
sudo find /home -nogroup 2>/dev/null | head -5

# 2. Check file permissions on important files
echo
echo "=== Critical File Permissions ==="

echo "Important system files:"
ls -la /etc/passwd /etc/shadow /etc/group /etc/sudoers 2>/dev/null

# Check SSH configuration:
if [ -f /etc/ssh/sshd_config ]; then
    echo
    echo "SSH daemon configuration:"
    ls -la /etc/ssh/sshd_config
fi

# Check crontab files:
echo
echo "Crontab files:"
ls -la /etc/crontab 2>/dev/null
ls -la /var/spool/cron/crontabs/* 2>/dev/null | head -3

# 3. Verify umask settings
echo
echo "=== umask Settings ==="

# Show current umask:
echo "Current umask: $(umask)"
echo "Current umask (symbolic): $(umask -S)"

# Test file creation with current umask:
touch test_umask_file
mkdir test_umask_dir
echo "Files created with current umask:"
ls -la test_umask_*

# Show different umask values and their effects:
echo
echo "umask 022 creates files with 644 permissions"
echo "umask 002 creates files with 664 permissions"
echo "umask 077 creates files with 600 permissions"

# 4. Advanced security checks
echo
echo "=== Advanced Security Checks ==="

# Find large files that might be suspicious:
echo "Large files in /tmp (over 100MB):"
find /tmp -type f -size +100M 2>/dev/null | head -5

# Find recently modified system files:
echo
echo "Recently modified files in /etc (last 7 days):"
sudo find /etc -type f -mtime -7 2>/dev/null | head -10

# Find files with unusual permissions:
echo
echo "Files with 777 permissions (potential security risk):"
find /home -type f -perm 777 2>/dev/null | head -5

# Check for hidden files in user directories:
echo
echo "Hidden files in /home directories:"
sudo find /home -name ".*" -type f 2>/dev/null | head -10

# Find executable files in unusual locations:
echo
echo "Executable files in /tmp:"
find /tmp -type f -executable 2>/dev/null | head -5

# Check file capabilities (if supported):
if command -v getcap >/dev/null 2>&1; then
    echo
    echo "Files with capabilities:"
    sudo find /usr -type f -exec getcap {} \; 2>/dev/null | grep -v "=" | head -5
fi

# 5. Generate security report
echo
echo "=== Security Report Summary ==="

cat > security_report.txt << EOF
Security Audit Report
Generated: $(date)
User: $USER
System: $(hostname)

SUID Files Found: $(sudo find /usr -perm -4000 -type f 2>/dev/null | wc -l)
SGID Files Found: $(sudo find /usr -perm -2000 -type f 2>/dev/null | wc -l)
World-writable Files in /tmp: $(find /tmp -perm -002 -type f 2>/dev/null | wc -l)
Orphaned Files in /home: $(sudo find /home -nouser 2>/dev/null | wc -l)

Current umask: $(umask)

Critical File Permissions:
$(ls -la /etc/passwd /etc/shadow /etc/group /etc/sudoers 2>/dev/null)

Recommendations:
- Review SUID/SGID files regularly
- Monitor world-writable files
- Check for orphaned files
- Verify umask settings are appropriate
- Audit file permissions on critical system files
EOF

echo "Security report saved to security_report.txt"
cat security_report.txt

# Clean up test files:
rm -f test_umask_*
```

## Advanced Permission and Security Examples:

```bash
# ACL (Access Control Lists) examples (if supported):
if command -v setfacl >/dev/null 2>&1; then
    echo "=== Access Control Lists (ACL) ==="
    
    # Create test file:
    touch acl_test.txt
    echo "Test content" > acl_test.txt
    
    # Set ACL for specific user:
    setfacl -m u:alice:rw acl_test.txt 2>/dev/null || echo "ACL not supported or user alice doesn't exist"
    
    # Show ACL:
    getfacl acl_test.txt 2>/dev/null || echo "ACL not supported"
    
    # Remove test file:
    rm -f acl_test.txt
fi

# Extended attributes (if supported):
if command -v attr >/dev/null 2>&1; then
    echo
    echo "=== Extended Attributes ==="
    
    touch xattr_test.txt
    
    # Set extended attribute:
    attr -s description -V "Important file" xattr_test.txt 2>/dev/null || echo "Extended attributes not supported"
    
    # List extended attributes:
    attr -l xattr_test.txt 2>/dev/null || echo "Extended attributes not supported"
    
    rm -f xattr_test.txt
fi

# SELinux context (if SELinux is enabled):
if command -v getenforce >/dev/null 2>&1 && [ "$(getenforce 2>/dev/null)" != "Disabled" ]; then
    echo
    echo "=== SELinux Security Context ==="
    
    touch selinux_test.txt
    ls -Z selinux_test.txt 2>/dev/null || echo "SELinux context not available"
    rm -f selinux_test.txt
fi
```

## Troubleshooting Common Issues:

```bash
# Issue 1: Permission denied
# Check file permissions:
ls -la filename
# Check directory permissions:
ls -ld directory/
# Solution: Adjust permissions with chmod

# Issue 2: Cannot execute script
# Check if file is executable:
ls -la script.sh
# Make executable:
chmod +x script.sh

# Issue 3: SUID/SGID not working
# Check if filesystem supports SUID/SGID:
mount | grep $(df . | tail -1 | awk '{print $1}')
# Some filesystems mount with nosuid option

# Issue 4: find command too slow
# Use locate instead for filename searches:
locate filename
# Update locate database:
sudo updatedb

# Issue 5: Cannot change permissions
# Check if file is immutable:
lsattr filename 2>/dev/null
# Remove immutable attribute:
chattr -i filename 2>/dev/null
```

## Verification Commands:

```bash
# Verify all lab exercises:
echo "=== Lab Verification ==="

echo "Permission lab directory:"
if [ -d ~/permission_lab ]; then
    echo "✓ Permission lab directory exists"
    ls -la ~/permission_lab/ | head -5
else
    echo "✗ Permission lab directory missing"
fi

echo
echo "Find lab directory:"
if [ -d ~/find_lab ]; then
    echo "✓ Find lab directory exists"
    find ~/find_lab -type f | wc -l | xargs echo "Files created:"
else
    echo "✗ Find lab directory missing"
fi

echo
echo "Special permissions test:"
if [ -d /opt/shared_project ]; then
    echo "✓ Shared project directory exists"
    ls -ld /opt/shared_project
else
    echo "✗ Shared project directory missing"
fi

echo
echo "Security audit files:"
if [ -f security_report.txt ]; then
    echo "✓ Security report generated"
    wc -l security_report.txt
else
    echo "✗ Security report missing"
fi

# Clean up (optional):
echo
echo "To clean up lab files:"
echo "rm -rf ~/permission_lab ~/find_lab"
echo "sudo rm -rf /opt/shared_project"
echo "rm -f security_report.txt"
```

## Key Learning Points:

1. **File Permissions**: Use chmod with numeric (755) or symbolic (u+x) notation
2. **Special Permissions**: SUID (4), SGID (2), Sticky bit (1) for advanced access control
3. **find Command**: Powerful tool for locating files by various criteria
4. **Security Auditing**: Regular checks for unusual permissions and file ownership
5. **umask**: Controls default permissions for newly created files
6. **Best Practices**: Principle of least privilege, regular security audits, proper file organization

---

## Navigation

**Previous:** [← Lesson 9: Users and Groups](lesson-09-lab-solutions.md)  
**Next:** [→ Lesson 11: System Administration and Advanced Topics](lesson-11-lab-solutions.md)  
**Home:** [↑ Solutions Index](README.md)