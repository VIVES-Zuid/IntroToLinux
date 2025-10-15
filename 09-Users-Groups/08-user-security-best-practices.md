# 8. User Security Best Practices


### Password Policies:

```bash
# Install password quality checking
sudo apt install libpam-pwquality

# Configure password policy in /etc/pam.d/common-password
# Add: password requisite pam_pwquality.so retry=3 minlen=8 difok=3

# Set password aging
sudo chage -M 90 -m 7 -W 7 username

# Check password aging
sudo chage -l username

# Force password change
sudo chage -d 0 username
```

### Account Security:

```bash
# Lock account after failed attempts
# Configure in /etc/pam.d/common-auth:
# auth required pam_tally2.so deny=5 unlock_time=900

# Set account expiration
sudo usermod -e 2024-12-31 tempuser

# Disable account (without deleting)
sudo usermod -L -s /usr/sbin/nologin username

# Monitor user activity
last username
lastlog
who
w
```

### File Permission Security:

```bash
# Set proper home directory permissions
sudo chmod 750 /home/username

# Secure user files
sudo find /home/username -type f -exec chmod 640 {} \;
sudo find /home/username -type d -exec chmod 750 {} \;

# Check for world-writable files
find /home -type f -perm -002 -ls

# Check for SUID/SGID files
find /home -type f \( -perm -4000 -o -perm -2000 \) -ls
```
---

## Navigation

**Previous:** [← Practical User Management Scenarios](07-practical-user-management-scenarios.md)  
**Next:** [→ Troubleshooting User Issues](09-troubleshooting-user-issues.md)  
**Lesson Home:** [↑ Lesson 09: Users Groups](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
