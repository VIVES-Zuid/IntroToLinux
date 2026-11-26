# 9. Security Best Practices

### ✅ The Principle of Least Privilege

Give every user and every program **only the permissions it really needs**, nothing more.

| ✅ Do | ❌ Don't |
|------|---------|
| `chmod 600` for private files (keys, passwords, config with secrets) | `chmod 777` "because then it works" |
| `chmod 755` / `644` for things others must use or read | make files writable for others |
| Share via a **group** (ℹ️ advanced: + SGID directory `2775`) | give everyone access to everything |
| Use `sudo` for one command | stay logged in as root |
| Check `ls -l` after every `chmod -R` | `chmod -R` blindly on a big directory |

### Typical Permissions on Ubuntu

```
 File / directory              Permissions        Why
 ────────────────────────────  ─────────────────  ─────────────────────────────────────
 /etc/passwd                   644  rw-r--r--     everyone may read the user list
 /etc/shadow                   640  rw-r-----     only root (and group shadow): password hashes!
 /etc/sudoers                  440  r--r-----     read-only, even for root (edit via visudo)
 /home/alice                   750  rwxr-x---     private home directory
 /tmp                         1777  rwxrwxrwt     everyone writes, sticky bit protects your files (ℹ️)
 ~/.ssh/                       700  rwx------     private
 ~/.ssh/id_ed25519             600  rw-------     private key: SSH refuses it if others can read it
 ~/.ssh/id_ed25519.pub         644  rw-r--r--     public key may be read
 ~/.ssh/authorized_keys        600  rw-------
 shared project directory     2775  rwxrwsr-x     SGID: new files get the project group (ℹ️)
```

ℹ️ The rows with 4 digits (`1777`, `2775`) use the special bits of section 4 (advanced).

```bash
# Check them yourself
ls -l /etc/passwd /etc/shadow /etc/sudoers
ls -ld /home/* /tmp
```

### ℹ️ Advanced: A Small Security Audit Script

> ℹ️ **Advanced** – optional. Combines `find` (section 6) with a script (lesson 8). Run it with `sudo` to avoid permission errors.

```bash
#!/bin/bash
# Script: security-audit.sh
# Usage:  sudo ./security-audit.sh

echo "=== Security Audit Report: $(date) ==="

echo "=== SUID files in /usr ==="
find /usr -perm -4000 -type f 2>/dev/null

echo "=== World-writable files in /home ==="
find /home -perm -002 -type f 2>/dev/null | head -20

echo "=== Files without an existing owner ==="
find /home -nouser 2>/dev/null

echo "=== Large files (>100MB) in /home ==="
find /home -size +100M -exec ls -lh {} \; 2>/dev/null

echo "=== System files changed in the last 24 hours ==="
find /etc -mtime -1 -type f 2>/dev/null

echo "=== Audit complete ==="
```

---

## Navigation

**Next:** [→ Review Questions](10-review-questions.md)  
**Previous:** [← Practical Labs](08-practical-labs.md)  
**Lesson Home:** [↑ Lesson 10: File Permissions and Tools](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
