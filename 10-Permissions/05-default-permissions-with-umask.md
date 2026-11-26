# 5. ℹ️ Default Permissions with umask

When you create a new file or directory, which permissions does it get? That's decided by the **umask**: a list of permissions that are **taken away** from the maximum.

### How It Works

```
                     new FILE          new DIRECTORY
 maximum             rw-rw-rw- (666)   rwxrwxrwx (777)
 umask 022 removes   ----w--w-         ----w--w-
                     ─────────         ─────────
 result              rw-r--r-- (644)   rwxr-xr-x (755)
```

> 💡 Files start from `666`, not `777`: Linux **never** makes a new file executable automatically. You always do that yourself with `chmod +x`.

### Common umask Values

| umask | New file | New directory | Use |
|-------|----------|---------------|-----|
| `022` | `644` rw-r--r-- | `755` rwxr-xr-x | group and others may only read (root's default on Ubuntu) |
| `002` | `664` rw-rw-r-- | `775` rwxrwxr-x | the group may also write (default for regular users on Ubuntu) |
| `077` | `600` rw------- | `700` rwx------ | private: nobody else gets access |

```bash
umask                 # show the current umask, e.g. 0002
umask -S              # show it as letters: u=rwx,g=rwx,o=rx

# Try it
umask 022
touch test_file
mkdir test_dir
ls -ld test_file test_dir
# -rw-r--r-- ... test_file
# drwxr-xr-x ... test_dir
```

> ⚠️ "666 − 022 = 644" works for the common values, but the umask is **not** really a subtraction: it **removes bits**. Example: umask `033` gives `644` for a file (and not 633), because there was no `x` to remove in the first place.

### Setting umask

```bash
umask 077                          # only for the current shell session
echo "umask 077" >> ~/.bashrc      # permanently, for your own user
```

> ℹ️ **Advanced** – The system-wide default is set in `/etc/login.defs` (`UMASK`). A shared project directory usually combines `umask 002` with an SGID directory (section 4), so that new files are automatically group-writable **and** belong to the project group.

---

## Navigation

**Next:** [→ The Find Command Advanced File Search](06-the-find-command-advanced-file-search.md)  
**Previous:** [← Special Permission Bits](04-special-permission-bits.md)  
**Lesson Home:** [↑ Lesson 10: File Permissions and Tools](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
