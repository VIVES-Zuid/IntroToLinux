# 2. The chmod Command

`chmod` (**ch**ange **mod**e) changes the permissions. Only the **owner** of the file (or root) may do that. There are two ways to write it:

### Symbolic Mode: Who, What, Which

```
 chmod  u  +  x  script.sh
        │  │  │
        │  │  └── which permission:  r  w  x
        │  └───── what:   + add    - remove    = set exactly
        └──────── who:    u user   g group   o others   a all
```

```bash
# Add permissions
chmod u+x script.sh       # user may execute
chmod g+w file.txt        # group may write
chmod a+r file.txt        # all may read
chmod +x script.sh        # without "who": the same as a+x

# Remove permissions
chmod o-r file.txt        # others may no longer read
chmod g-w file.txt        # group may no longer write

# Set exactly (everything else for that "who" is removed)
chmod u=rw file.txt       # user: read + write, no execute
chmod o= file.txt         # others: nothing

# Combine with commas (no spaces!)
chmod u=rwx,g=rx,o= script.sh
```

### Numeric Mode: Set All at Once

```bash
chmod 644 file.txt        # rw-r--r--
chmod 755 script.sh       # rwxr-xr-x
chmod 600 private.txt     # rw-------
chmod 700 private_dir/    # rwx------
```

### Symbolic or Numeric?

| | Symbolic (`u+x`) | Numeric (`755`) |
|---|---|---|
| Changes | only what you mention | **everything** at once |
| Handy for | "add execute permission" | "make it exactly rwxr-xr-x" |

```
 before:  rw-r-----
 chmod o+r      →  rw-r--r--     (only "others" changed)
 chmod 644      →  rw-r--r--     (same result here, but 644 overwrites ALL nine bits)
```

### Recursive: `-R`

```bash
chmod -R g+w project/     # the directory and EVERYTHING inside it
```

> ⚠️ Be careful with `chmod -R 755 dir/`: that makes **every file** inside executable too.
>
> ℹ️ **Advanced** – A capital `X` adds execute only to directories (and files that are already executable): `chmod -R u=rwX,go=rX dir/` gives directories `755` and normal files `644`.

### Practical chmod Examples

```bash
# Make a script executable
chmod +x script.sh

# Secure SSH keys (SSH refuses keys that others can read!)
chmod 700 ~/.ssh/
chmod 600 ~/.ssh/id_ed25519          # private key
chmod 644 ~/.ssh/id_ed25519.pub      # public key

# Read-only for everyone, also for yourself (protect against accidents)
chmod 444 backup.tar.gz

# Shared file: owner and group may change it, others may read it
chmod 664 shared.txt
```

---

## Navigation

**Next:** [→ Ownership With Chown And Chgrp](03-ownership-with-chown-and-chgrp.md)  
**Previous:** [← Understanding Linux File Permissions](01-understanding-linux-file-permissions.md)  
**Lesson Home:** [↑ Lesson 10: File Permissions and Tools](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
