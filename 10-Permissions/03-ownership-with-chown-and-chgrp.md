# 3. Ownership with chown and chgrp

Every file has a **user owner** and a **group owner**:

```
 -rw-r--r-- 1  alice  developers  1024 Jan 15 10:30 report.txt
               ─┬───  ────┬─────
                │         └── group owner   → change with chgrp (or chown :group)
                └──────────── user owner    → change with chown
```

> ⚠️ Only **root** can give a file to another user, so you almost always need `sudo chown`. A regular user who tries gets `Operation not permitted`.

### chown: Change the Owner (and Group)

```
 sudo chown  alice:developers  report.txt
             ──┬── ────┬─────
               │       └── new group  (optional)
               └────────── new owner
```

```bash
sudo chown alice file.txt              # new owner only
sudo chown alice:developers file.txt   # new owner AND new group
sudo chown :developers file.txt        # new group only (same as chgrp)
sudo chown alice: file.txt             # owner alice, group = alice's primary group
sudo chown -R alice:alice /home/alice  # recursive: the directory and everything inside
```

### chgrp: Change Only the Group

```bash
chgrp developers file.txt         # you may do this yourself if you own the file AND are in that group
sudo chgrp -R developers project/ # recursive
```

### Practical Ownership Examples

```bash
# Web server files belong to the web server user
sudo chown -R www-data:www-data /var/www/html/

# Fix the ownership of a user's home directory
sudo chown -R alice:alice /home/alice/

# Shared project directory for the group developers
sudo chown -R :developers /opt/project/
sudo chmod -R g+w /opt/project/
```

> ℹ️ **Advanced** – `chown --from=olduser newuser file` only changes files currently owned by `olduser`. `chgrp --reference=ref.txt target.txt` copies the group of another file.

---

## Navigation

**Next:** [→ Special Permission Bits](04-special-permission-bits.md)  
**Previous:** [← The Chmod Command](02-the-chmod-command.md)  
**Lesson Home:** [↑ Lesson 10: File Permissions and Tools](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
