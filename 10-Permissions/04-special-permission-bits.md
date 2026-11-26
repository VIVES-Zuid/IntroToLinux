# 4. ℹ️ Special Permission Bits

> ℹ️ **Advanced** – this whole section is optional and **not** part of the exam. The basic permissions `rwx` (sections 1–3) are enough for this course. Read on if you are curious how `passwd`, `/tmp` and shared project folders work.

Besides `rwx` there are three special bits.

| Bit | Value | Symbolic | Shows as | On a file | On a directory |
|-----|-------|----------|----------|-----------|----------------|
| **SUID** | 4000 | `u+s` | `rws------` | runs as the **owner** of the file | (not used) |
| **SGID** | 2000 | `g+s` | `---rws---` | runs as the **group** of the file | new files get the **directory's group** |
| **Sticky** | 1000 | `+t` | `------rwt` | (not used) | only the **owner of a file** may delete it |

The special bit is written as a 4th digit **in front**: `chmod 2775` = SGID + `775`.

```
 chmod 2775 project/

    2      7     7     5
    │      │     │     │
  SGID   user  group  others    →  drwxrwsr-x
```

### SUID (4000): Run as the Owner

```bash
ls -l /usr/bin/passwd
# -rwsr-xr-x 1 root root ... /usr/bin/passwd
#     ↑ s = SUID: passwd always runs as root
```

```
 student runs passwd  ──▶  passwd runs as ROOT  ──▶  may write /etc/shadow ✅
```

`/etc/shadow` is only writable by root, yet every user can change their own password. The `passwd` program has SUID, so it runs with the rights of its owner, root.

> ⚠️ SUID programs owned by root are a security risk: a bug in such a program can give an attacker root rights. That's why administrators regularly check which SUID files exist (section 6).

### SGID (2000) on a Directory: Shared Project Folders

This is the **most useful** special bit in practice:

```
 Without SGID:                          With SGID (chmod 2775):

 /opt/project   (group developers)      /opt/project   (group developers)
 └── alice.txt  (group alice) ❌        └── alice.txt  (group developers) ✅
     the group can't edit it                the whole team can edit it
```

```bash
sudo mkdir /opt/project
sudo chgrp developers /opt/project
sudo chmod 2775 /opt/project           # or: sudo chmod g+s /opt/project
ls -ld /opt/project
# drwxrwsr-x 2 root developers ... /opt/project
#       ↑ s = SGID
```

### Sticky Bit (1000): Only Delete Your Own Files

```bash
ls -ld /tmp
# drwxrwxrwt 10 root root ... /tmp
#          ↑ t = sticky bit
```

Everyone may create files in `/tmp` (`rwx` for others), but thanks to the sticky bit you can only delete **your own** files, even when a file of someone else has `777` permissions.

```bash
chmod 1777 shared_dir/                 # or: chmod +t shared_dir/
```

### Lowercase or Uppercase?

| Shows | Means |
|-------|-------|
| `s` / `t` | special bit **and** `x` are set (normal) |
| `S` / `T` | special bit is set, but `x` is **not**, usually a mistake |

```
 chmod u+s file      →  -rwSr--r--   (S: SUID without execute, has no effect)
 chmod u+x file      →  -rwsr--r--   (s: now it works)
```

### ℹ️ Advanced: Finding Files with Special Bits

```bash
find /usr -perm -4000 2>/dev/null             # SUID files
find /usr -perm -2000 2>/dev/null             # SGID files
find / -type d -perm -1000 2>/dev/null        # directories with sticky bit
```

---

## Navigation

**Next:** [→ Default Permissions With Umask](05-default-permissions-with-umask.md)  
**Previous:** [← Ownership With Chown And Chgrp](03-ownership-with-chown-and-chgrp.md)  
**Lesson Home:** [↑ Lesson 10: File Permissions and Tools](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
