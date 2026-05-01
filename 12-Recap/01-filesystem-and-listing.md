# 1. File Listing and Navigation

---

## 1.1 `ls` with Common Options

`ls` lists directory contents. The most useful options for the exam:

| Option | Effect |
|--------|--------|
| `-l` | Long format (permissions, owner, size, date) |
| `-h` | Human-readable sizes (KB, MB) — only meaningful with `-l` |
| `-a` | Show hidden files (names starting with `.`) |
| `-i` | Show inode number of each entry |

Options can be combined: `ls -lha`, `ls -li`, `ls -lha /some/path`.

```bash
ls -lha /usr/share        # long format, human sizes, hidden files
ls -li /tmp               # long format + inode numbers
ls -la /etc | sort -k5 -n # long format, sort by size (column 5, numeric)
```

### Exercises

**1.1** List the contents of `/usr/share` in long format, including hidden files, with human-readable sizes.

**1.2** Display the inode number alongside the details of every file in `/etc`. (Hint: combine two options.)

**1.3** From your home directory, list `/usr/share` in detail (including hidden files), save the output to `details.txt`, and immediately after also display the same list sorted by file size (column 5, largest last). Do everything in **one command**.

---

## 1.2 `find` — Searching the File System

`find` searches a directory tree and can filter by name, type, size, inode, and more.

```bash
find <start-path> [options]
```

| Option | Example | Meaning |
|--------|---------|---------|
| `-type f` | `find /etc -type f` | Regular files only |
| `-type d` | `find /etc -type d` | Directories only |
| `-name 'pattern'` | `-name '*.conf'` | Name matches glob pattern (case-sensitive) |
| `-iname 'pattern'` | `-iname '*.txt'` | Case-insensitive name match |
| `-size Nc` | `-size 16c` | Exactly N bytes |
| `-size +Nk` | `-size +10k` | Larger than N kilobytes |
| `-inum N` | `-inum 12345` | Match by inode number |
| `-maxdepth N` | `-maxdepth 1` | Limit recursion depth (1 = current dir only) |
| `2>/dev/null` | (append) | Suppress permission-denied errors |

```bash
# Find all regular files in /etc that are exactly 16 bytes
find /etc -type f -size 16c 2>/dev/null

# Find a file by its inode number
find /etc -inum 98765

# Find files in /var/log larger than 1 MB
find /var/log -type f -size +1M 2>/dev/null
```

### Exercises

**1.4** From your home directory, use `find` to search `/etc` for regular files that are **exactly 16 bytes** in size. Display the path of each file. Suppress any error messages.

**1.5** Find all **regular files** in your home directory whose name starts with an **uppercase letter** and ends in `.txt`. Limit the search to the home directory itself (do **not** recurse into subdirectories).

**1.6** Find all files in `/boot` that **end in `.cfg`**. Suppress permission errors.

**1.7** A file in `/etc` has inode number `98765`. Write the `find` command to locate it.

---

## 1.3 Absolute vs. Relative Paths

- **Absolute path**: starts with `/`. Always refers to the same location regardless of where you are.
  - Example: `/etc/hostname`, `/home/student/report.txt`
- **Relative path**: starts from the current working directory.
  - `.` = current directory, `..` = parent directory
  - Example: `../documents/notes.txt`, `./script.sh`

```bash
# Absolute: works from anywhere
cp /etc/resolvconf/update.d/libc /tmp/

# Relative: only correct if you are in /etc/resolvconf/update.d/
cp libc /tmp/

# Moving up with ..
mv report.txt ../archive/     # move to sibling directory
```

> ⚠️ In scripts, always use **absolute paths** for files that need to be found regardless of where the script is called from.

### Exercises

**1.8** Copy the file `/etc/resolvconf/update.d/libc` to `/tmp/` using its **absolute path**.

**1.9** You are in `/home/student/projects/`. Move `notes.txt` to `/home/student/` using a **relative path** (do not use the full absolute path for the destination).

**1.10** From your home directory, copy `~/documents/report.pdf` to `~/backup/` using only relative paths.

---

## Navigation

**Next:** [→ I/O, Redirects and Pipelines](02-io-redirects-and-pipelines.md)
**Previous:** [← Overview](00-overview.md)
**Lesson Home:** [↑ Lesson 12: Exam Preparation Recap](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
