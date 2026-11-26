# 5. System Maintenance and Monitoring

> ℹ️ **Advanced – not part of the exam.** Chapter 11 is a look ahead for students who want to go further. You do **not** need to know this material for the *Introduction to Linux* exam. The follow-up course **Linux Server Management** covers these topics in depth.


### Log File Management:

```bash
# System logs location
/var/log/syslog        # General system messages
/var/log/auth.log      # Authentication logs
/var/log/kern.log      # Kernel messages
/var/log/dpkg.log      # Package management (Debian/Ubuntu)

# View logs
sudo tail -f /var/log/syslog           # Follow log in real-time
sudo journalctl                        # systemd journal
sudo journalctl -u ssh                 # Logs for specific service
sudo journalctl -f                     # Follow journal
sudo journalctl --since "1 hour ago"   # Recent logs

# Log rotation
sudo logrotate -f /etc/logrotate.conf  # Force log rotation
```

### Package Management:

```bash
# Debian/Ubuntu (apt)
sudo apt update                # Update package lists
sudo apt upgrade               # Upgrade installed packages
sudo apt install package      # Install package
sudo apt remove package       # Remove package
sudo apt autoremove           # Remove unused dependencies
sudo apt search keyword       # Search for packages

# RedHat/CentOS (yum/dnf)
sudo yum update               # RHEL 7 and earlier
sudo dnf update               # RHEL 8+ and Fedora
sudo dnf install package
sudo dnf remove package
```

### System Updates and Security:

```bash
# Check for security updates (Ubuntu)
sudo unattended-upgrade --dry-run

# Update system
sudo apt update && sudo apt upgrade -y

# Check system version
cat /etc/os-release
lsb_release -a
uname -a

# Check installed packages
dpkg -l                        # Debian/Ubuntu
rpm -qa                        # RedHat/CentOS
```
---

## Navigation

**Next:** [→ Backup And Recovery Strategies](06-backup-and-recovery-strategies.md)  
**Previous:** [← Network Basics](04-network-basics.md)  
**Lesson Home:** [↑ Lesson 11: System Administration and Advanced Topics](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
