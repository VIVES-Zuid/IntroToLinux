# 9. Troubleshooting User Issues


### Common Problems and Solutions:

#### User Can't Login:
```bash
# Check if account is locked
sudo passwd -S username

# Check account expiration
sudo chage -l username

# Check shell validity
grep username /etc/passwd
which /bin/bash

# Check home directory permissions
ls -ld /home/username
```

#### Permission Denied Issues:
```bash
# Check user's groups
groups username

# Check file permissions
ls -la problematic_file

# Check sudo access
sudo -l -U username

# Check if user is in correct groups
getent group groupname
```

#### Password Issues:
```bash
# Reset password
sudo passwd username

# Check password policy
sudo grep pwquality /etc/pam.d/common-password

# Check for account lock
sudo pam_tally2 --user username

# Unlock account
sudo pam_tally2 --user username --reset
```
---

## Navigation

**Next:** [→ Review Questions](10-review-questions.md)  
**Previous:** [← User Security Best Practices](08-user-security-best-practices.md)  
**Lesson Home:** [↑ Lesson 9: Users & Groups](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
