# 3. Creating and Managing Users


### Adding Users:

#### adduser (Interactive - Debian/Ubuntu)
```bash
# Interactive user creation (recommended for desktop)
sudo adduser newuser

# This prompts for:
# - Password
# - Full name
# - Room number
# - Work phone
# - Home phone
# - Other information
```

#### useradd (Command-line - Universal)
```bash
# Basic user creation
sudo useradd newuser

# Create user with home directory
sudo useradd -m newuser

# Create user with specific shell
sudo useradd -m -s /bin/bash newuser

# Create user with specific UID and GID
sudo useradd -m -u 1500 -g users newuser

# Create user with multiple groups
sudo useradd -m -G sudo,adm,plugdev newuser

# Create system user (no home directory)
sudo useradd -r systemuser

# Create user with expiration date
sudo useradd -m -e 2024-12-31 tempuser
```

#### Common useradd Options:
- `-m`: Create home directory
- `-s shell`: Set default shell
- `-g group`: Set primary group
- `-G groups`: Set secondary groups
- `-u UID`: Set specific UID
- `-c comment`: Set GECOS field (full name)
- `-e date`: Set expiration date
- `-r`: Create system account

### Setting Passwords:

```bash
# Set password for user
sudo passwd username

# Set password for current user
passwd

# Force password change on next login
sudo passwd -e username

# Lock user account
sudo passwd -l username

# Unlock user account
sudo passwd -u username

# Check password status
sudo passwd -S username
```

### Modifying Users:

#### usermod Command:
```bash
# Change username
sudo usermod -l newname oldname

# Change user's primary group
sudo usermod -g newgroup username

# Add user to secondary groups (append)
sudo usermod -aG sudo,adm username

# Replace user's secondary groups
sudo usermod -G sudo,adm username

# Change user's home directory
sudo usermod -d /new/home -m username

# Change user's shell
sudo usermod -s /bin/zsh username

# Lock user account
sudo usermod -L username

# Unlock user account
sudo usermod -U username

# Set account expiration
sudo usermod -e 2024-12-31 username
```

#### chsh: Change Shell
```bash
# Change current user's shell
chsh

# Change another user's shell
sudo chsh -s /bin/zsh username

# List available shells
cat /etc/shells
```

#### chfn: Change Full Name (GECOS)
```bash
# Change current user's information
chfn

# Change specific user's information
sudo chfn username
```

### Deleting Users:

```bash
# Delete user but keep home directory
sudo userdel username

# Delete user and home directory
sudo userdel -r username

# Delete user and all files owned by user
sudo userdel -r -f username

# Alternative (Debian/Ubuntu)
sudo deluser username
sudo deluser --remove-home username
```
---

## Navigation

**Next:** [→ Group Management](04-group-management.md)  
**Previous:** [← User Information Files](02-user-information-files.md)  
**Lesson Home:** [↑ Lesson 9: Users & Groups](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
