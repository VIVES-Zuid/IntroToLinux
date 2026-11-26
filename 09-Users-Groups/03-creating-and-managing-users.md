# 3. Creating and Managing Users

All commands in this section change system files, so they need `sudo`.

```mermaid
graph LR
    A["➕ Create<br/>useradd / adduser"] --> B["🔑 Password<br/>passwd"]
    B --> C["✏️ Modify<br/>usermod"]
    C --> D["🔒 Lock<br/>passwd -l / usermod -L"]
    D --> E["🗑️ Delete<br/>userdel"]
```

### Adding Users: `adduser` or `useradd`?

| | `adduser` | `useradd` |
|---|---|---|
| Style | interactive: asks questions | everything via options |
| Home directory | created automatically | only with `-m` |
| Password | asked immediately | set afterwards with `passwd` |
| Default shell | `/bin/bash` | `/bin/sh` (so add `-s /bin/bash`) |
| Available on | Debian / Ubuntu | every Linux distribution |

#### adduser (interactive, Debian/Ubuntu)

```bash
sudo adduser newuser
# asks for: password, full name, room number, phones ... (just press Enter to skip)
```

#### useradd (works everywhere, great for scripts)

```bash
# The usual way: with home directory (-m) and bash as shell (-s)
sudo useradd -m -s /bin/bash newuser
sudo passwd newuser                       # then set a password

# With a full name (comment)
sudo useradd -m -s /bin/bash -c "Alice Johnson" alice

# With secondary groups
sudo useradd -m -s /bin/bash -G sudo,developers newuser
```

| Option | Meaning |
|--------|---------|
| `-m` | create the home directory |
| `-s shell` | login shell |
| `-c "text"` | comment / full name |
| `-G groups` | secondary groups (comma-separated, no spaces) |
| `-g group` | primary group |
| `-u UID` | a specific UID |
| `-e YYYY-MM-DD` | account expiration date |

> ℹ️ **Advanced** – `sudo useradd -r serviceuser` creates a system account (UID < 1000, no home directory), used for services. `sudo useradd -m -e 2027-06-30 tempuser` creates an account that stops working after 30 June 2027.

### Setting Passwords: `passwd`

```bash
passwd                      # change your OWN password
sudo passwd username        # set the password of another user

sudo passwd -l username     # lock the account (login with password impossible)
sudo passwd -u username     # unlock it again
sudo passwd -S username     # show status: P = password set, L = locked
sudo passwd -e username     # force a new password at the next login
```

### Modifying Users: `usermod`

```bash
sudo usermod -aG developers username   # ADD to a secondary group  (-a = append!)
sudo usermod -c "New Name" username    # change the full name
sudo usermod -s /bin/zsh username      # change the login shell
sudo usermod -L username               # lock the account
sudo usermod -U username               # unlock the account
```

### ⚠️ The Most Common Mistake: `-G` Without `-a`

```
 Before:   alice is in: alice, sudo, developers

 sudo usermod -aG docker alice   ✅  →  alice, sudo, developers, docker   (added)
 sudo usermod -G docker alice    ❌  →  alice, docker                      (sudo and developers are GONE!)
```

Without `-a` (append), `-G` **replaces** all secondary groups. If that removes you from `sudo`, you can no longer use `sudo` to fix it!

> 💡 **Remember:** `-aG` = **a**dd to **G**roup.

### ℹ️ Advanced usermod Options

```bash
sudo usermod -l newname oldname        # rename the user (login name)
sudo usermod -d /new/home -m username  # new home directory, move the files (-m)
sudo usermod -g newgroup username      # change the primary group
sudo usermod -e 2027-06-30 username    # set an expiration date

chsh -s /bin/zsh                       # change your own shell (see: cat /etc/shells)
chfn                                   # change your own full name / info
```

### Deleting Users: `userdel`

```bash
sudo userdel username        # delete the user, KEEP the home directory
sudo userdel -r username     # delete the user AND the home directory + mail
```

> ⚠️ `userdel -r` cannot be undone: make a backup of the home directory first if you might need the files.
>
> ℹ️ `userdel -f` (force) deletes the account even when the user is still logged in. It does **not** delete other files the user owns elsewhere on the system. Find those with `find / -user username` (lesson 10).

Ubuntu also has the friendlier `sudo deluser username` and `sudo deluser --remove-home username`.

---

## Navigation

**Next:** [→ Group Management](04-group-management.md)  
**Previous:** [← User Information Files](02-user-information-files.md)  
**Lesson Home:** [↑ Lesson 9: Users and Groups](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
