# Solutions for Lesson 9: Users and Groups

## Lab 7.1: User Creation and Management (20 minutes)

### Solution:

```bash
# Note: These commands require sudo privileges

# 1. Create test users
sudo useradd -m -s /bin/bash alice    # -m creates home directory, -s sets shell
sudo useradd -m -s /bin/bash bob
sudo useradd -m -s /bin/bash charlie

# Verify users were created:
grep -E "(alice|bob|charlie)" /etc/passwd
# Expected output showing user entries with UID, GID, home directories

# Check home directories were created:
ls -la /home/ | grep -E "(alice|bob|charlie)"

# 2. Set passwords
sudo passwd alice
# Enter password when prompted (e.g., "alice123")
sudo passwd bob
# Enter password when prompted (e.g., "bob123")
sudo passwd charlie
# Enter password when prompted (e.g., "charlie123")

# Alternative: Set passwords non-interactively (for testing)
echo "alice:alice123" | sudo chpasswd
echo "bob:bob123" | sudo chpasswd
echo "charlie:charlie123" | sudo chpasswd

# 3. Check user information
id alice
# Expected output: uid=1001(alice) gid=1001(alice) groups=1001(alice)

getent passwd alice
# Expected output: alice:x:1001:1001::/home/alice:/bin/bash

groups alice
# Expected output: alice : alice

# Additional user information commands:
finger alice  # If finger is installed
sudo cat /etc/shadow | grep alice  # Check encrypted password (first few chars)

# Check user's default shell:
echo $SHELL                    # Current user's shell
getent passwd alice | cut -d: -f7  # Alice's shell

# 4. Modify user properties
sudo usermod -c "Alice Johnson" alice     # Add full name (comment field)
sudo usermod -aG sudo bob                 # Add bob to sudo group

# Additional usermod examples:
sudo usermod -s /bin/zsh charlie          # Change shell for charlie
sudo usermod -e 2024-12-31 alice          # Set account expiration date

# 5. Verify changes
getent passwd alice
# Expected output: alice:x:1001:1001:Alice Johnson:/home/alice:/bin/bash

groups bob
# Expected output: bob : bob sudo

# Check sudo access for bob:
sudo -u bob sudo whoami  # Should return "root" if successful

# Verify charlie's shell change:
getent passwd charlie | cut -d: -f7  # Should show /bin/zsh

# Additional verification commands:
sudo chage -l alice     # Check password aging info
id bob                  # Check bob's group memberships with GID numbers
```

## Lab 7.2: Group Management (15 minutes)

### Solution:

```bash
# 1. Create project groups
sudo groupadd developers         # Create developers group
sudo groupadd testers           # Create testers group
sudo groupadd project-alpha     # Create project-specific group

# Verify groups were created:
getent group | grep -E "(developers|testers|project-alpha)"
# Or check specific groups:
getent group developers
getent group testers
getent group project-alpha

# Check group IDs:
grep -E "(developers|testers|project-alpha)" /etc/group

# 2. Add users to groups
sudo usermod -aG developers alice      # Add alice to developers (append to groups)
sudo usermod -aG developers bob        # Add bob to developers
sudo usermod -aG testers charlie       # Add charlie to testers
sudo usermod -aG project-alpha alice   # Add alice to project-alpha
sudo usermod -aG project-alpha charlie # Add charlie to project-alpha

# Alternative method using gpasswd:
# sudo gpasswd -a alice developers
# sudo gpasswd -a bob developers

# 3. Verify group membership
groups alice
# Expected output: alice : alice developers project-alpha

groups bob
# Expected output: bob : bob sudo developers

groups charlie
# Expected output: charlie : charlie testers project-alpha

getent group developers
# Expected output: developers:x:1004:alice,bob

getent group testers
# Expected output: testers:x:1005:charlie

getent group project-alpha
# Expected output: project-alpha:x:1006:alice,charlie

# Additional group management commands:
sudo gpasswd -A alice developers      # Make alice admin of developers group
sudo gpasswd -M alice,bob developers  # Set group members (replaces existing)

# Check which groups a user belongs to with detailed info:
id alice  # Shows all groups with both names and GID numbers

# List all groups on the system:
cut -d: -f1 /etc/group | sort

# Find groups with specific GID range:
awk -F: '$3 >= 1000 && $3 < 2000 {print $1}' /etc/group
```

## Lab 7.3: sudo Configuration (15 minutes)

### Solution:

```bash
# 1. Check current sudo privileges
sudo -l
# Shows current user's sudo permissions

# Check sudo version and configuration:
sudo -V | head -5

# 2. Add user to sudo group (already done for bob in previous lab)
sudo usermod -aG sudo alice    # Add alice to sudo group

# Verify alice is in sudo group:
groups alice
getent group sudo

# 3. Test sudo access
sudo -u alice sudo whoami      # Test alice's sudo access
# Expected output: root

# Test with password prompt simulation:
sudo -u alice sudo -l          # List alice's sudo privileges

# 4. Create custom sudo rule
# IMPORTANT: Always use visudo to edit sudoers file safely
# For demonstration, we'll show the content that should be added

echo "Creating custom sudo rule for charlie..."

# The rule to add (use visudo in real scenarios):
# charlie ALL=(ALL) NOPASSWD: /usr/bin/systemctl status *

# Add rule using visudo (safer method):
sudo bash -c 'echo "charlie ALL=(ALL) NOPASSWD: /usr/bin/systemctl status *" >> /etc/sudoers.d/charlie'

# Alternative: Use visudo command
# sudo visudo
# Then add the line: charlie ALL=(ALL) NOPASSWD: /usr/bin/systemctl status *

# Verify the rule was added:
sudo cat /etc/sudoers.d/charlie

# 5. Test custom rule
sudo -u charlie sudo systemctl status ssh
# Should work without password prompt

# Test that charlie can't run other sudo commands without password:
# sudo -u charlie sudo ls /root  # Should prompt for password

# Additional sudo examples:
echo "=== Additional sudo Configuration Examples ==="

# Create group-based sudo rule:
sudo bash -c 'echo "%developers ALL=(ALL) /usr/bin/apt update, /usr/bin/apt upgrade" >> /etc/sudoers.d/developers'

# User-specific rules with command restrictions:
sudo bash -c 'echo "alice ALL=(ALL) NOPASSWD: /bin/systemctl restart nginx" >> /etc/sudoers.d/alice-nginx'

# Test the rules:
sudo -u alice sudo -l  # List alice's sudo permissions

# Check sudo log entries:
sudo tail -10 /var/log/auth.log | grep sudo

# Remove custom rules (cleanup):
sudo rm -f /etc/sudoers.d/charlie
sudo rm -f /etc/sudoers.d/developers
sudo rm -f /etc/sudoers.d/alice-nginx
```

## Lab 7.4: User Security Audit (10 minutes)

### Solution:

```bash
# 1. Check users with shells
grep "/bin/bash\|/bin/sh" /etc/passwd
# Shows users with interactive shells

# More comprehensive shell check:
grep -E "/bin/(bash|sh|zsh|fish|csh|tcsh)$" /etc/passwd

# Count users with interactive shells:
grep -E "/bin/(bash|sh|zsh|fish|csh|tcsh)$" /etc/passwd | wc -l

# 2. Find users with UID 0 (should only be root)
awk -F: '$3 == 0 {print $1}' /etc/passwd
# Expected output: root (only)

# If other users show up, investigate:
awk -F: '$3 == 0 {print $1 ": " $0}' /etc/passwd

# 3. Check for empty passwords
sudo awk -F: '$2 == "" {print $1}' /etc/shadow
# Should show no output (no users with empty passwords)

# Check for users with no password set (different from empty):
sudo awk -F: '$2 == "*" || $2 == "!" {print $1}' /etc/shadow

# 4. Check sudo group members
getent group sudo
# Shows all users in sudo group

# Alternative ways to check sudo access:
getent group admin 2>/dev/null  # Some systems use admin group
grep -E "^(sudo|admin|wheel):" /etc/group

# 5. Check recent logins
last | head -10
# Shows recent login history

# More detailed login analysis:
last -n 20              # Last 20 logins
lastb | head -10        # Failed login attempts
who                     # Currently logged in users
w                       # Detailed current user activity

# 6. Check failed login attempts
sudo grep "Failed password" /var/log/auth.log | tail -10
# Shows recent failed login attempts

# More comprehensive security checks:
echo "=== Comprehensive Security Audit ==="

echo "1. Password Policy Check:"
sudo chage -l alice  # Check password aging for alice

echo "2. Account Status:"
sudo passwd -S alice bob charlie  # Check account status

echo "3. Login Shell Analysis:"
awk -F: '{shells[$7]++} END {for(shell in shells) printf "%s: %d users\n", shell, shells[shell]}' /etc/passwd

echo "4. User ID Analysis:"
echo "Users with UID < 1000 (system users):"
awk -F: '$3 < 1000 {print $1}' /etc/passwd | wc -l

echo "Users with UID >= 1000 (regular users):"
awk -F: '$3 >= 1000 {print $1}' /etc/passwd | wc -l

echo "5. Home Directory Check:"
echo "Users without home directories:"
awk -F: '{if(system("test -d " $6) != 0) print $1 ": " $6}' /etc/passwd

echo "6. Group Membership Analysis:"
echo "Users in multiple groups:"
for user in $(awk -F: '$3 >= 1000 {print $1}' /etc/passwd); do
    group_count=$(groups "$user" 2>/dev/null | wc -w)
    if [ "$group_count" -gt 2 ]; then  # User name + actual groups
        echo "$user: $(groups "$user" 2>/dev/null)"
    fi
done

echo "7. Sudo Access Audit:"
echo "Users with sudo access:"
getent group sudo | cut -d: -f4 | tr ',' '\n'

echo "8. File Permission Check:"
echo "Checking critical file permissions:"
ls -la /etc/passwd /etc/shadow /etc/group /etc/sudoers

echo "9. SSH Key Check:"
echo "Users with SSH keys:"
for home in /home/*; do
    if [ -d "$home/.ssh" ]; then
        user=$(basename "$home")
        key_count=$(find "$home/.ssh" -name "*.pub" 2>/dev/null | wc -l)
        echo "$user: $key_count SSH public keys"
    fi
done

echo "10. Recent Security Events:"
echo "Recent sudo usage:"
sudo grep "sudo:" /var/log/auth.log | tail -5

echo "Recent SSH connections:"
sudo grep "sshd.*Accepted" /var/log/auth.log | tail -5
```

## Advanced User Management Examples:

```bash
# Bulk user creation script
cat > create_users.sh << 'EOF'
#!/bin/bash
# Bulk user creation script

users=("student1" "student2" "student3" "teacher1" "admin1")
default_password="TempPass123!"

for user in "${users[@]}"; do
    echo "Creating user: $user"
    
    # Create user with specific settings
    sudo useradd -m -s /bin/bash -c "Auto-created user" "$user"
    
    # Set temporary password
    echo "$user:$default_password" | sudo chpasswd
    
    # Force password change on first login
    sudo chage -d 0 "$user"
    
    echo "User $user created successfully"
done

echo "All users created. They must change passwords on first login."
EOF

chmod +x create_users.sh

# User cleanup script
cat > cleanup_users.sh << 'EOF'
#!/bin/bash
# Cleanup test users script

test_users=("alice" "bob" "charlie")
test_groups=("developers" "testers" "project-alpha")

echo "Cleaning up test users and groups..."

# Remove users
for user in "${test_users[@]}"; do
    if id "$user" >/dev/null 2>&1; then
        echo "Removing user: $user"
        sudo userdel -r "$user"  # -r removes home directory
    fi
done

# Remove groups
for group in "${test_groups[@]}"; do
    if getent group "$group" >/dev/null 2>&1; then
        echo "Removing group: $group"
        sudo groupdel "$group"
    fi
done

echo "Cleanup completed."
EOF

chmod +x cleanup_users.sh

# Password policy enforcement script
cat > password_policy.sh << 'EOF'
#!/bin/bash
# Password policy enforcement

echo "Checking password policies..."

# Check current policy
echo "Current password aging policy:"
sudo cat /etc/login.defs | grep -E "(PASS_MAX_DAYS|PASS_MIN_DAYS|PASS_WARN_AGE)"

# Set password policy for new users
echo "Setting password policy..."
sudo sed -i 's/^PASS_MAX_DAYS.*/PASS_MAX_DAYS\t90/' /etc/login.defs
sudo sed -i 's/^PASS_MIN_DAYS.*/PASS_MIN_DAYS\t7/' /etc/login.defs
sudo sed -i 's/^PASS_WARN_AGE.*/PASS_WARN_AGE\t14/' /etc/login.defs

echo "Password policy updated."
EOF

chmod +x password_policy.sh
```

## Troubleshooting Common Issues:

```bash
# Issue 1: User can't sudo
# Check if user is in sudo group:
groups username
# Add to sudo group if missing:
sudo usermod -aG sudo username

# Issue 2: "user is not in the sudoers file"
# Check sudoers configuration:
sudo visudo -c  # Check syntax
# Add user to appropriate group or sudoers file

# Issue 3: Password not working
# Reset password:
sudo passwd username
# Check account status:
sudo passwd -S username

# Issue 4: Home directory not created
# Create home directory manually:
sudo mkhomedir_helper username
# Or recreate user with home directory:
sudo userdel username && sudo useradd -m username

# Issue 5: Group membership not taking effect
# User needs to log out and back in, or:
newgrp groupname  # Switch to new group in current session
```

## Verification Commands:

```bash
# Verify all lab exercises:
echo "=== User Management Verification ==="

echo "Test users exist:"
for user in alice bob charlie; do
    if id "$user" >/dev/null 2>&1; then
        echo "✓ $user exists"
    else
        echo "✗ $user missing"
    fi
done

echo "Groups exist:"
for group in developers testers project-alpha; do
    if getent group "$group" >/dev/null 2>&1; then
        echo "✓ $group exists"
    else
        echo "✗ $group missing"
    fi
done

echo "Group memberships:"
echo "Alice groups: $(groups alice 2>/dev/null || echo 'N/A')"
echo "Bob groups: $(groups bob 2>/dev/null || echo 'N/A')"
echo "Charlie groups: $(groups charlie 2>/dev/null || echo 'N/A')"

echo "Sudo access:"
getent group sudo | grep -q alice && echo "✓ Alice has sudo" || echo "✗ Alice no sudo"
getent group sudo | grep -q bob && echo "✓ Bob has sudo" || echo "✗ Bob no sudo"

# Clean up (optional)
echo "To clean up test users and groups, run: ./cleanup_users.sh"
```

## Key Learning Points:

1. **User Creation**: Use `useradd` with proper options (-m for home, -s for shell)
2. **Password Management**: Use `passwd` and `chage` for password policies
3. **Group Management**: Use `groupadd`, `usermod -aG`, and `gpasswd` for groups
4. **sudo Configuration**: Use `visudo` safely, understand sudoers syntax
5. **Security Auditing**: Regular checks of users, groups, and permissions
6. **Best Practices**: Principle of least privilege, strong passwords, regular audits

---

## Navigation

**Previous:** [← Lesson 8: Shell Scripting Fundamentals](lesson-08-lab-solutions.md)  
**Next:** [→ Lesson 10: File Permissions and Tools](lesson-10-lab-solutions.md)  
**Home:** [↑ Solutions Index](README.md)