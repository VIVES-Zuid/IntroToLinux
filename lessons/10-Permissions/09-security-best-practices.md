# 9. Security Best Practices


### File Permission Guidelines:

```bash
# System files
/etc/passwd         644 (readable by all, writable by root)
/etc/shadow         640 (readable by root and shadow group only)
/etc/sudoers        440 (readable by root only)

# SSH keys
~/.ssh/             700 (private directory)
~/.ssh/id_rsa       600 (private key)
~/.ssh/id_rsa.pub   644 (public key)
~/.ssh/authorized_keys  600 (authorized keys)

# Web files
HTML files          644 (readable by web server)
CGI scripts         755 (executable by web server)
Configuration       600 (private configuration)

# Shared directories
Project directories 2775 (SGID for group inheritance)
Temporary sharing   1777 (sticky bit for /tmp-like behavior)
```

### Regular Security Audits:

```bash
#!/bin/bash
# Script: security-audit.sh

echo "=== Security Audit Report ==="
echo "Date: $(date)"
echo

echo "=== SUID Files ==="
find /usr -perm -4000 -type f 2>/dev/null

echo "=== World-Writable Files ==="
find /home -perm -002 -type f 2>/dev/null | head -20

echo "=== Files with no Owner ==="
find /home -nouser 2>/dev/null

echo "=== Large Files (>100MB) ==="
find /home -size +100M -exec ls -lh {} \; 2>/dev/null

echo "=== Recently Modified System Files ==="
find /etc -mtime -1 -type f 2>/dev/null

echo "=== Audit Complete ==="
```
---

## Navigation

**Previous:** [← Practical Labs](08-practical-labs.md)  
**Next:** [→ Review Questions](10-review-questions.md)  
**Lesson Home:** [↑ Lesson 10: Permissions](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
