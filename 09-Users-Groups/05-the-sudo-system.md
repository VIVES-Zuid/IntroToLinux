# 5. The sudo System


### Understanding sudo
`sudo` (substitute user do) allows regular users to execute commands with elevated privileges without knowing the root password.

### sudo Configuration: /etc/sudoers

```bash
# Edit sudoers file (NEVER edit directly!)
sudo visudo

# Example sudoers entries:
# User privilege specification
root    ALL=(ALL:ALL) ALL

# Members of admin group can run any command
%admin  ALL=(ALL) ALL

# Members of sudo group can run any command
%sudo   ALL=(ALL:ALL) ALL

# Allow user to run specific commands
alice   ALL=(ALL) /usr/bin/apt, /usr/bin/systemctl

# Allow group to run commands without password
%developers ALL=(ALL) NOPASSWD: /usr/bin/git, /usr/bin/make
```

### sudo Syntax in /etc/sudoers:
```
user/group  hosts=(run_as_users:run_as_groups) commands
```

### Common sudo Usage:

```bash
# Run command as root
sudo command

# Run command as specific user
sudo -u username command

# Switch to root shell
sudo -i
sudo -s

# Run previous command with sudo
sudo !!

# Check sudo privileges
sudo -l

# Validate sudo credentials (refresh timeout)
sudo -v

# List sudo commands in history
history | grep sudo
```

### sudo Configuration Examples:

```bash
# Allow user to manage services
john    ALL=(ALL) /bin/systemctl start *, /bin/systemctl stop *, /bin/systemctl restart *

# Allow group to manage web server
%webadmin ALL=(ALL) /usr/sbin/nginx, /usr/sbin/apache2ctl

# Allow user to install packages without password
alice   ALL=(ALL) NOPASSWD: /usr/bin/apt install *, /usr/bin/apt update

# Restrict to specific hosts
bob     webserver01=(ALL) ALL

# Allow running commands as specific user
%developers ALL=(www-data) /usr/bin/php, /bin/cat /var/log/apache2/*
```

### sudo Security Best Practices:

```bash
# Always use visudo to edit sudoers
sudo visudo

# Test sudoers syntax
sudo visudo -c

# Use groups instead of individual users when possible
%sudo   ALL=(ALL:ALL) ALL

# Be specific with command paths
alice   ALL=(ALL) /usr/bin/systemctl

# Use NOPASSWD sparingly and only for safe commands
%backup ALL=(ALL) NOPASSWD: /usr/bin/rsync
```
---

## Navigation

**Next:** [→ User Environment And Configuration](06-user-environment-and-configuration.md)  
**Previous:** [← Group Management](04-group-management.md)  
**Lesson Home:** [↑ Lesson 9: Users & Groups](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
