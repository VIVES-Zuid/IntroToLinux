# 6. The find Command: Advanced File Search

`find` searches **live** through the directory tree and finds files based on name, type, size, age, owner, permissions ...

### The Structure of a find Command

```
 find  /home  -type f  -name "*.txt"  -exec ls -l {} \;
       ──┬──  ────────┬────────────   ─────────┬───────
         │            │                        │
     where to    tests: which files?      action: what to do with them
     search      (all must be true)       (default: print the path)
```

```bash
find /home -name "notes.txt"     # find notes.txt anywhere under /home
find . -name "*.txt"             # . = start in the current directory
```

> 💡 Searching in system directories gives lots of `Permission denied` errors. Hide them with `2>/dev/null` (lesson 4): `find / -name "*.conf" 2>/dev/null`

### By Name: `-name` and `-iname`

```bash
find /home -name "file.txt"      # exact name
find /home -iname "FILE.TXT"     # ignore upper/lowercase
find /home -name "*.txt"         # wildcards: put the pattern in "quotes"!
find /home -name "test?.log"     # ? = exactly one character
find /home /tmp -name "*.conf"   # search in several directories
```

> ⚠️ Without quotes the shell expands `*.txt` **before** `find` runs (globbing, lesson 6), and you get strange results or errors.

### By Type: `-type`

| Test | Finds |
|------|-------|
| `-type f` | regular **f**iles |
| `-type d` | **d**irectories |
| `-type l` | symbolic **l**inks |

```bash
find /var/log -type f -name "*.log"   # only files ending in .log
find ~ -type d -name "backup*"        # only directories starting with backup
```

### By Size: `-size`

```
 -size +10M    larger than 10 MB
 -size -10M    smaller than 10 MB
 -size 10M     (about) 10 MB

 units:  c = bytes   k = KB   M = MB   G = GB
```

```bash
find /home -size +100M                 # larger than 100 MB
find /home -size +10M -size -100M      # between 10 and 100 MB (both tests must be true)
find /home -size -1024c                # smaller than 1024 bytes
find /tmp -empty                       # empty files and directories
```

> ⚠️ `find` rounds sizes **up** to whole units. `-size -1k` therefore only finds **empty** files (a 500-byte file counts as "1k", which is not less than 1k). For small sizes, use bytes: `-size -1024c`.

### By Time: `-mtime` and `-mmin`

```
            -mtime +7            │   -mtime -7
   ◀──── more than 7 days ago ───┼── within the last 7 days ────▶ now
```

```bash
find /home -mtime -7      # modified in the last 7 days
find /home -mtime +30     # modified more than 30 days ago
find /tmp -mmin -60       # modified in the last 60 minutes
```

> ℹ️ **Advanced** – `-mtime` counts in whole periods of 24 hours: `-mtime 1` means "between 24 and 48 hours ago". `-atime` uses the last **a**ccess time, `-ctime` the last **c**hange of the metadata (permissions, owner).

### By Owner: `-user` and `-group`

```bash
find /home -user alice           # files owned by alice
find /opt -group developers      # files belonging to group developers
find / -nouser 2>/dev/null       # files whose owner no longer exists (deleted user)
```

### Combining Tests

Tests after each other must **all** be true (AND):

```bash
find /var/log -type f -name "*.log" -size +1M      # .log files larger than 1 MB
find ~ -type f -mtime -1 -name "*.sh"              # scripts changed since yesterday
```

### ℹ️ Advanced: By Permissions: `-perm`

> ℹ️ **Advanced** – optional on a first read.

| Test | Finds files with ... |
|------|----------------------|
| `-perm 644` | **exactly** `644` |
| `-perm -644` | **at least** these bits (`644`, `755`, `777` ... all match) |
| `-perm /222` | **any** of these bits (writable by someone) |

```bash
find /home -perm -002 -type f            # world-writable files (security risk!)
find /usr -perm -4000 -type f            # SUID files
find ~ -executable -type f               # files you may execute
```

### ℹ️ Advanced: Doing Something With the Results

> ℹ️ **Advanced** – optional on a first read. These actions change or delete files: **always** run the `find` without the action first and check the list!

```
 find /tmp -name "*.tmp" -exec rm {} \;
                               ── ─┬─
                               │   └── \; = end of the -exec command
                               └────── {} = replaced by every file found
```

```bash
find /tmp -name "*.tmp" -delete                  # ⚠️ delete the files found
find /home -name "*.bak" -exec rm {} \;          # run rm for every file found
find /tmp -name "*.tmp" -ok rm {} \;             # ask y/n before every file
find /home -name "*.txt" -exec ls -l {} +        # + = run ls ONCE with all files (faster)
find /var/log -name "*.log" -mtime +30 -delete   # ⚠️ clean up logs older than 30 days
find /home -name "*.conf" -printf "%p %s bytes\n" # custom output: path and size
```

---

## Navigation

**Next:** [→ System Tools](07-system-tools.md)  
**Previous:** [← Default Permissions With Umask](05-default-permissions-with-umask.md)  
**Lesson Home:** [↑ Lesson 10: File Permissions and Tools](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
