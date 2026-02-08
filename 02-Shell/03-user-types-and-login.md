# 3. User Types and Login

---

## 👥 User Categories

<table>
<tr>
<th width="50%">🔴 Administrator (Privileged User)</th>
<th width="50%">💚 Regular User (Unprivileged User)</th>
</tr>
<tr>
<td valign="top">

**Username:** `root`

✅ Full system access  
✅ Can modify any file or setting  
✅ Can install software system-wide  
🔴 Prompt ends with `#`

</td>
<td valign="top">

**Username:** varies (`alice`, `bob`, etc.)

⚠️ Limited system access  
✅ Can modify files in home directory  
❌ Cannot change system settings  
💚 Prompt ends with `$`

</td>
</tr>
</table>

---

## 🔄 Switching Users

### 📋 Quick Reference

| Command           | Description                               | Password Required |
| ----------------- | ----------------------------------------- | ----------------- |
| `su - <username>` | Switch to another user                    | User's password   |
| `su -`            | Switch to root                            | Root password     |
| `sudo su`         | Switch to root                            | Your password     |
| `sudo <command>`  | Run single command as root                | Your password     |
| `sudo -i`         | Interactive root shell (root environment) | Your password     |
| `sudo -s`         | Interactive root shell (user environment) | Your password     |

### 💻 Command Examples

```bash
# Switch to another regular user (- can be omitted)
su - alice

# Switch to root account (prompts for Root password)
su -

# Switch to root account (prompts for User password)
sudo su

# Run single command as another user (root or administrator in that case)
sudo apt update

# Run command as specific user
sudo -u postgres psql

# Get interactive root shell
# Switches to Root environment (home)
sudo -i

# or (Remains in user environment)
sudo -s
```

---

## 🔍 Who's Logged In?

### 👤 User Information Commands

```bash
# Show all logged in users
who
┌──────────────────────────────────┐
│ alice    tty1  2024-02-08 10:30  │
│ bob      pts/0 2024-02-08 11:15  │
└──────────────────────────────────┘

# Show information about current session
who am i

# Show current username
whoami
alice

# Show detailed user activity
w
┌───────────────────────────────────────────────────┐
│ USER   TTY    LOGIN@   IDLE   WHAT                │
│ alice  pts/0  10:30    0.00s  w                   │
│ bob    pts/1  11:15    5:30   vim document.txt    │
└───────────────────────────────────────────────────┘
```

---

## Navigation

**Next:** [→ Linux File System Structure](04-linux-file-system-structure.md)  
**Previous:** [← The Shell Your Command Interface](02-the-shell-your-command-interface.md)  
**Lesson Home:** [↑ Lesson 2: The Shell](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
