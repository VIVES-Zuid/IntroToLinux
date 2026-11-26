# 4. Group Management

Groups make it easy to give **several users** the same access, e.g. to a shared project directory (lesson 10).

### The Basic Commands

| Task | Command |
|------|---------|
| create a group | `sudo groupadd developers` |
| add a user to a group | `sudo usermod -aG developers alice` |
| remove a user from a group | `sudo gpasswd -d alice developers` |
| show the members | `getent group developers` |
| show the groups of a user | `groups alice` |
| delete a group | `sudo groupdel developers` |

```bash
# Create a group and add two users
sudo groupadd developers
sudo usermod -aG developers alice
sudo usermod -aG developers bob

# Check
getent group developers        # developers:x:1003:alice,bob
groups alice                   # alice : alice developers
```

> 💡 `sudo gpasswd -a alice developers` also adds a user to a group. It does the same as `usermod -aG`, so pick one and stick with it.

### ⚠️ New Group Membership: Log Out and In Again!

A user only gets the rights of a new group after a **new login**. The shell that is already running still has the old list of groups.

```
 $ sudo usermod -aG developers student
 $ id                          ← still the old groups (no "developers")
 ... log out and log in again ...
 $ id                          ← now "developers" is in the list ✅
```

> 💡 Quick test without logging out: `su - student` starts a new login shell with the new groups.

### Adding Several Users at Once

A small `for` loop from lesson 8:

```bash
for user in alice bob charlie; do
    sudo usermod -aG developers "$user"
done
```

### ℹ️ Advanced Group Commands

> ℹ️ **Advanced** – optional on a first read.

```bash
sudo groupadd -g 2000 projectx          # create a group with a specific GID
sudo groupadd -r servicegroup           # create a system group
sudo groupmod -n newname oldname        # rename a group
sudo groupmod -g 2500 groupname         # change the GID
sudo gpasswd -A alice developers        # make alice a group administrator
sudo delgroup groupname                 # Debian/Ubuntu alternative for groupdel
getent group                            # list all groups
```

---

## Navigation

**Next:** [→ The Sudo System](05-the-sudo-system.md)  
**Previous:** [← Creating And Managing Users](03-creating-and-managing-users.md)  
**Lesson Home:** [↑ Lesson 9: Users and Groups](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
