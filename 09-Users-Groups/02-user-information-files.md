# 2. User Information Files

Three text files hold all information about users and groups:

| File | Contains | Who can read it? |
|------|----------|------------------|
| `/etc/passwd` | the user accounts | everyone |
| `/etc/shadow` | the password **hashes** and password rules | only root |
| `/etc/group` | the groups and their members | everyone |

> ⚠️ Don't edit these files by hand. Use commands like `useradd`, `usermod` and `passwd` (section 3); they make sure the files stay consistent.

### ⚠️ /etc/passwd: User Accounts

```bash
cat /etc/passwd
```

Every line is one user, with 7 fields separated by `:`

```
  john : x : 1000 : 1000 : John Doe : /home/john : /bin/bash
   │     │     │      │       │           │            │
   1     2     3      4       5           6            7
   │     │     │      │       │           │            └─ login shell
   │     │     │      │       │           └─ home directory
   │     │     │      │       └─ comment (full name)
   │     │     │      └─ GID of the primary group
   │     │     └─ UID
   │     └─ password: "x" = stored in /etc/shadow
   └─ username
```

```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
john:x:1000:1000:John Doe,,,:/home/john:/bin/bash
```

> 💡 Login shell `/usr/sbin/nologin` means: this account can't log in. That's normal for system users.

### /etc/shadow: Passwords

```bash
sudo cat /etc/shadow       # only root may read it
```

```
  john : $y$j9T$...hash... : 19500 : 0 : 99999 : 7 : : :
   │            │             │      │     │     │
   1            2             3      4     5     6     (7, 8, 9 empty)
   │            │             │      │     │     └─ warn: days before expiry to warn
   │            │             │      │     └─ max: days a password stays valid
   │            │             │      └─ min: days before it may be changed again
   │            │             └─ last change: days since 1 Jan 1970
   │            └─ password hash ($y$ = yescrypt, the Ubuntu default)
   └─ username
```

The password itself is **never** stored, only a **hash**: a one-way "fingerprint". At login, Linux hashes what you type and compares the two hashes.

| Value in field 2 | Meaning |
|------------------|---------|
| `$y$...` or `$6$...` | a normal password hash |
| `!` or `*` | no password set / login with password not possible |
| `!$y$...` | the account is **locked** (`!` placed in front of the hash) |

> ℹ️ **Advanced** – Fields 7, 8 and 9 are: inactive days after expiry, account expiration date (in days since 1 Jan 1970) and a reserved field. You view and change these with `chage` (section 8).

### /etc/group: Groups

```bash
cat /etc/group
```

```
  sudo : x : 27 : john,alice
    │    │    │       │
    1    2    3       4
    │    │    │       └─ members (secondary group members, comma-separated)
    │    │    └─ GID
    │    └─ group password (not used)
    └─ group name
```

```
root:x:0:
sudo:x:27:john,alice
john:x:1000:
```

> 💡 The group `john` shows no members, yet `john` is in it: it is his **primary** group (via the GID in `/etc/passwd`). The 4th field only lists **secondary** members.

### Commands to Look Up User Information

```bash
whoami                  # who am I?
id                      # my UID, GID and all my groups
id alice                # ... of another user
groups                  # my groups (names only)
groups alice
getent passwd alice     # alice's line from /etc/passwd
getent group sudo       # the sudo group and its members

who                     # who is logged in right now?
w                       # ... and what are they doing?
last                    # login history
```

---

## Navigation

**Next:** [→ Creating And Managing Users](03-creating-and-managing-users.md)  
**Previous:** [← Introduction To Users And Groups](01-introduction-to-users-and-groups.md)  
**Lesson Home:** [↑ Lesson 9: Users and Groups](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
