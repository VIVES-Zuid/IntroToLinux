# 7. Practical User Management Scenarios

> 💡 Do these labs on your **own VM**, not on a shared server. At the end you can clean up with `sudo userdel -r alice` (etc.) and `sudo groupdel developers` (etc.).

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

# 3. Test sudo access: become alice, then use sudo as alice
su - alice                 # type alice's password
sudo whoami                # type alice's password again → prints: root
exit                       # back to your own user

# 4. ℹ️ Advanced: create a custom sudo rule in its own file
sudo visudo -f /etc/sudoers.d/charlie
# Add this line, save and quit:
# charlie ALL=(ALL) NOPASSWD: /usr/bin/systemctl status *

# 5. Test the custom rule (no password asked for this command)
sudo -u charlie sudo systemctl status ssh
```

### ℹ️ Lab 7.4: User Security Audit (10 minutes)

> ℹ️ **Advanced** – optional. Uses the filters of lesson 7 on the user files.

```bash
# 1. Check users with shells
grep "/bin/bash\|/bin/sh" /etc/passwd

# 2. Find users with UID 0 (should only be root)
cut -d: -f1,3 /etc/passwd | grep ":0$"

# 3. Check for empty passwords (should give no output)
sudo cut -d: -f1,2 /etc/shadow | grep ":$"

# 4. Check sudo group members
getent group sudo

# 5. Check recent logins
last | head -10

# 6. Check failed login attempts
sudo grep "Failed password" /var/log/auth.log | tail -10
```

---

## Navigation

**Next:** [→ User Security Best Practices](08-user-security-best-practices.md)  
**Previous:** [← User Environment And Configuration](06-user-environment-and-configuration.md)  
**Lesson Home:** [↑ Lesson 9: Users and Groups](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
