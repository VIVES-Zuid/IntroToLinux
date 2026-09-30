# 4. Links and Permissions

---

## 4.1 Hard Links and Soft (Symbolic) Links

### Hard Links

A hard link is an **additional name** for the same file on disk (same inode).

```bash
ln original.txt hardlink.txt    # create a hard link
```

- Deleting the original does **not** affect the hard link — the data survives as long as any hard link exists.
- Hard links cannot span file systems or point to directories.
- Both names share the same inode number (check with `ls -i`).

### Soft (Symbolic) Links

A soft link is a **pointer** to another file's path (different inode).

```bash
ln -s /bin/bash ~/myShell       # create a symlink named myShell
ln -s $(which bash) ~/myShell   # same, using which to find bash
```

- If the original is deleted or moved, the symlink **breaks**.
- Can cross file systems and point to directories.
- `ls -l` shows them with `->`: `myShell -> /bin/bash`

### Quick Comparison

| | Hard link | Soft link |
|--|-----------|-----------|
| Same inode? | Yes | No |
| Survives original deletion? | Yes | No |
| Can cross file systems? | No | Yes |
| Points to directories? | No | Yes |

### Exercises

**4.1** Create a **soft link** named `myShell` in your home directory that points to `/bin/bash`. The link must break if `/bin/bash` is deleted.

**4.2** Create a **hard link** named `backup.conf` pointing to `/etc/hostname` inside your home directory.

**4.3** Use `ls -li` to confirm that `backup.conf` and `/etc/hostname` share the same inode number.

**4.4** What type of link should you use if you want the link to remain valid even after the original file is deleted?

---

## 4.2 Permissions: Symbolic Notation

Linux permissions have three groups: **u** (user/owner), **g** (group), **o** (others), **a** (all three).

Each group can have: **r** (read=4), **w** (write=2), **x** (execute=1).

### Symbolic `chmod` syntax

```bash
chmod who[+-=]permission[,who[+-=]permission] file
```

| Operator | Meaning |
|----------|---------|
| `+` | Add permission |
| `-` | Remove permission |
| `=` | Set exactly (clears others first) |

```bash
chmod u+x script.sh             # add execute for owner
chmod g-w file.txt              # remove write from group
chmod o= file.txt               # remove all permissions for others
chmod u=rw,g=r,o= file.txt      # owner: rw, group: r, others: none
chmod u=,g=r,o=rx script.sh     # owner: none, group: r, others: rx
chmod a+x script.sh             # add execute for everyone
```

> ⚠️ `chmod u=rwx,g=r,o=rx file` sets ALL three groups in **one command** with commas.  
> No semicolons (`;`), `&&`, or `||` needed — commas do the job.

### Exercises

**4.5** Using **symbolic notation**, set the permissions of `examenscript.sh` so that:
- Owner: no access at all
- Group: read only
- Others: read and execute

Do this in **one** `chmod` command without using `;`, `&&`, or `||`.

**4.6** Using symbolic notation, **add execute** permission for the owner on all `.sh` files in the current directory.

**4.7** What symbolic `chmod` command removes write from the owner and sets others to read-only, in one command?

---

## 4.3 Permissions: Octal Notation

Each permission group is represented as a single digit: **r=4, w=2, x=1**. Add the values.

| Binary | Octal | Permissions |
|--------|-------|-------------|
| 111 | 7 | rwx |
| 110 | 6 | rw- |
| 101 | 5 | r-x |
| 100 | 4 | r-- |
| 011 | 3 | -wx |
| 010 | 2 | -w- |
| 001 | 1 | --x |
| 000 | 0 | --- |

```bash
chmod 755 script.sh    # rwxr-xr-x
chmod 644 file.txt     # rw-r--r--
chmod 600 private.key  # rw-------
chmod 502 file.txt     # r-x---w-   (owner=5, group=0, others=2)
```

### Exercises

**4.8** Using **octal notation**, set permissions on `report.txt` so that:
- Owner: read and execute
- Group: no access
- Others: write only

**4.9** What octal value represents: owner=rwx, group=r-x, others=r--?

**4.10** What are the permissions for a file with octal mode `634`?

---

## 4.4 Permissions on Directory Contents (from Outside)

To change permissions of everything **inside** a directory without changing the directory itself, use a glob pattern or `find`:

```bash
chmod 644 ~/mydir/*                          # all files directly in mydir
chmod 634 ~/executables/*                    # octal on all contents
find ~/mydir -type f -exec chmod 644 {} \;   # recursive, files only
```

> `chmod 644 ~/mydir` changes the **directory** itself.  
> `chmod 644 ~/mydir/*` changes the **contents** of the directory.

### Exercises

**4.11** Using **octal notation**, set all files inside `~/executables/` to:
- Owner: read and write
- Group: write and execute
- Others: read only

Change only the **contents**, not the directory itself.

**4.12** Using `find`, recursively set all **regular files** in `~/project/` to permission `644`.

---

## Navigation

**Next:** [→ Alias and Scripting](05-alias-and-scripting.md)
**Previous:** [← Text Filters](03-text-filters.md)
**Lesson Home:** [↑ Lesson 12: Exam Preparation Recap](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
