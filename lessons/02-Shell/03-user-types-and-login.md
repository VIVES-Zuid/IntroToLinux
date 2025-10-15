# 3. User Types and Login


### User Categories

#### Administrator (Privileged User)
- Username: **root**
- Full system access
- Can modify any file or setting
- Prompt ends with `#`

#### Regular User (Unprivileged User)
- Limited system access
- Can only modify files in home directory
- Prompt ends with `$`

### Switching Users

```bash
# Switch to another regular user
su - <username>

# Switch to root account
su -

# Run single command as another user
sudo <command>

# Run command as specific user
sudo -u <username> <command>

# Get interactive root shell
sudo -i
# or
sudo -s
```

### Who's Logged In?

```bash
# Show all logged in users
who

# Show information about current session
who am i

# Show current username
whoami

# Show detailed user activity
w
```
---

## Navigation

**Previous:** [← The Shell Your Command Interface](02-the-shell-your-command-interface.md)  
**Next:** [→ Linux File System Structure](04-linux-file-system-structure.md)  
**Lesson Home:** [↑ Lesson 02: Shell](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
