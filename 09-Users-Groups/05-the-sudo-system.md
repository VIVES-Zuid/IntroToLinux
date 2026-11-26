# 5. The sudo System

Regular users can't change system files. For administrative work there are two tools:

| | `sudo` | `su` |
|---|---|---|
| Full name | "**s**uperuser **do**" | "**s**witch **u**ser" |
| What it does | run **one** command as root | become another user (default: root) |
| Whose password? | **your own** password | the password of the **target** user |
| Logged? | yes, every command is logged | no, only the switch |
| Typical use | `sudo apt update` | `su - alice` |

```
 sudo apt update
   │
   ├─ 1. Are you allowed? (member of group "sudo" / rule in /etc/sudoers)
   ├─ 2. Type YOUR password (remembered for 15 minutes)
   └─ 3. apt update runs as root ──▶ back to your normal user
```

> 💡 On Ubuntu the root account has **no password**, so you can't log in as root directly. Users in the group `sudo` can use `sudo`. That's the safer way: you only have root rights when you really need them.

### Using sudo

```bash
sudo command               # run one command as root
sudo -u alice command      # run a command as user alice
sudo !!                    # repeat the previous command, now with sudo
sudo -l                    # which commands may I run with sudo?

sudo -i                    # open a root shell (as if root logged in)
exit                       # ... and go back to your own user
```

> ⚠️ In a root shell (`sudo -i`) the prompt ends with `#` instead of `$`. Every command is now executed as root: be careful, and `exit` as soon as you're done.

### Using su

```bash
su - alice                 # become alice (asks alice's password)
whoami                     # alice
exit                       # back to your own user

sudo su -                  # become root using YOUR password (Ubuntu)
```

> 💡 The `-` in `su -` starts a **login shell**: you get the target user's environment (home directory, PATH ...). Without `-` you keep your own environment, which can cause confusing behaviour.

### Who May Use sudo?

On Ubuntu: every member of the group `sudo`.

```bash
sudo usermod -aG sudo alice      # give alice sudo rights (she must log in again)
getent group sudo                # who is in the sudo group?
```

### ℹ️ Advanced: The sudoers File

> ℹ️ **Advanced** – optional on a first read.

The rules live in `/etc/sudoers` and in files in `/etc/sudoers.d/`. **Always** edit them with `visudo`: it checks the syntax before saving. A typo in a normal editor can lock everyone out of `sudo`.

```bash
sudo visudo                              # edit /etc/sudoers
sudo visudo -f /etc/sudoers.d/charlie    # better: one extra file per rule
sudo visudo -c                           # check the syntax
```

A rule has this structure:

```
  alice   ALL = (ALL:ALL)   /usr/bin/apt, /usr/bin/systemctl
    │      │       │                     │
    │      │       │                     └─ which commands (full path!) – ALL = everything
    │      │       └─ as which (user:group) – ALL = anyone, including root
    │      └─ on which host – ALL = on every machine
    └─ user (or %group)
```

```
# The default rules on Ubuntu
root    ALL=(ALL:ALL) ALL
%sudo   ALL=(ALL:ALL) ALL                # % = a group: members of sudo may do everything

# alice may only run apt and systemctl as root
alice   ALL=(ALL) /usr/bin/apt, /usr/bin/systemctl

# charlie may check service status without typing a password
charlie ALL=(ALL) NOPASSWD: /usr/bin/systemctl status *
```

Best practices for sudoers:
- Give rights to **groups** (`%webadmins`) instead of individual users
- Always use the **full path** of commands (`/usr/bin/systemctl`, find it with `which systemctl`)
- Use `NOPASSWD` rarely, and only for harmless commands

---

## Navigation

**Next:** [→ User Environment And Configuration](06-user-environment-and-configuration.md)  
**Previous:** [← Group Management](04-group-management.md)  
**Lesson Home:** [↑ Lesson 9: Users and Groups](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
