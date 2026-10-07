# 8. Practical Labs


### Lab 10.1: Permission Management (20 minutes)

```bash
# 1. Create test environment
mkdir ~/permission_lab
cd ~/permission_lab
mkdir private shared public
touch private/secret.txt shared/team.txt public/readme.txt

# 2. Set different permission levels
chmod 700 private/
chmod 600 private/secret.txt

chmod 770 shared/
chmod 660 shared/team.txt

chmod 755 public/
chmod 644 public/readme.txt

# 3. Verify permissions
ls -la
ls -la private/ shared/ public/

# 4. Test access (try as different user if possible)
# Create test user first: sudo useradd -m testuser
```

### Lab 10.2: Special Permissions (15 minutes)

```bash
# 1. Create shared project directory
sudo mkdir /opt/shared_project
sudo chgrp developers /opt/shared_project

# 2. Set SGID bit for automatic group inheritance
sudo chmod 2775 /opt/shared_project

# 3. Test the setup
cd /opt/shared_project
touch test_file.txt
ls -la test_file.txt    # Should show 'developers' group

# 4. Create sticky bit directory
mkdir ~/temp_shared
chmod 1777 ~/temp_shared
ls -ld ~/temp_shared    # Should show 't' at the end
```

### Lab 10.3: find Command Practice (25 minutes)

```bash
# 1. Create test files
mkdir -p ~/find_lab/{docs,images,scripts,logs}
touch ~/find_lab/docs/file{1..5}.txt
touch ~/find_lab/images/photo{1..3}.jpg
touch ~/find_lab/scripts/script{1..2}.sh
touch ~/find_lab/logs/app.log ~/find_lab/logs/error.log

# 2. Make some files executable
chmod +x ~/find_lab/scripts/*.sh

# 3. Practice find commands
# Find all .txt files
find ~/find_lab -name "*.txt"

# Find executable files
find ~/find_lab -executable -type f

# Find files larger than 0 bytes
find ~/find_lab -size +0c

# Find and list with details
find ~/find_lab -name "*.log" -exec ls -la {} \;

# 4. Complex searches
# Find files modified in last minute
find ~/find_lab -mmin -1

# Find by type and execute action
find ~/find_lab -name "*.sh" -exec chmod 755 {} \;
```

### Lab 10.4: Security Audit (15 minutes)

```bash
# 1. Find potential security issues
# Find SUID files
sudo find /usr -perm -4000 -type f 2>/dev/null | head -10

# Find world-writable files
find /tmp -perm -002 -type f 2>/dev/null | head -10

# Find files with no owner
sudo find /home -nouser 2>/dev/null

# 2. Check file permissions on important files
ls -la /etc/passwd /etc/shadow /etc/sudoers

# 3. Verify umask settings
umask
umask -S

# 4. Create test with current umask
touch test_umask_file
mkdir test_umask_dir
ls -la test_umask_*
```
---

## Navigation

**Next:** [→ Security Best Practices](09-security-best-practices.md)  
**Previous:** [← System Tools](07-system-tools.md)  
**Lesson Home:** [↑ Lesson 10: Permissions](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
