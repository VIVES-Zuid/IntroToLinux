# 7. Practical User Management Scenarios


### Lab 7.1: User Creation and Management (20 minutes)

```bash
# 1. Create test users
sudo useradd -m -s /bin/bash alice
sudo useradd -m -s /bin/bash bob
sudo useradd -m -s /bin/bash charlie

# 2. Set passwords
sudo passwd alice
sudo passwd bob
sudo passwd charlie

# 3. Check user information
id alice
getent passwd alice
groups alice

# 4. Modify user properties
sudo usermod -c "Alice Johnson" alice
sudo usermod -aG sudo bob

# 5. Verify changes
getent passwd alice
groups bob
```

### Lab 7.2: Group Management (15 minutes)

```bash
# 1. Create project groups
sudo groupadd developers
sudo groupadd testers
sudo groupadd project-alpha

# 2. Add users to groups
sudo usermod -aG developers alice
sudo usermod -aG developers bob
sudo usermod -aG testers charlie
sudo usermod -aG project-alpha alice
sudo usermod -aG project-alpha charlie

# 3. Verify group membership
groups alice
groups bob
groups charlie
getent group developers
```

### Lab 7.3: sudo Configuration (15 minutes)

```bash
# 1. Check current sudo privileges
sudo -l

# 2. Add user to sudo group
sudo usermod -aG sudo alice

# 3. Test sudo access
sudo -u alice sudo whoami

# 4. Create custom sudo rule
sudo visudo
# Add line: charlie ALL=(ALL) NOPASSWD: /usr/bin/systemctl status *

# 5. Test custom rule
sudo -u charlie sudo systemctl status ssh
```

### Lab 7.4: User Security Audit (10 minutes)

```bash
# 1. Check users with shells
grep "/bin/bash\|/bin/sh" /etc/passwd

# 2. Find users with UID 0 (should only be root)
awk -F: '$3 == 0 {print $1}' /etc/passwd

# 3. Check for empty passwords
sudo awk -F: '$2 == "" {print $1}' /etc/shadow

# 4. Check sudo group members
getent group sudo

# 5. Check recent logins
last | head -10

# 6. Check failed login attempts
sudo grep "Failed password" /var/log/auth.log | tail -10
```
---

## Navigation

**Previous:** [← User Environment And Configuration](06-user-environment-and-configuration.md)  
**Next:** [→ User Security Best Practices](08-user-security-best-practices.md)  
**Lesson Home:** [↑ Lesson 09: Users Groups](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
