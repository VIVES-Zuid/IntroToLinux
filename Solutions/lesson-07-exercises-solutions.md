# Solutions: Lesson 7 Exercises – Filters & Pipelines

Solutions for [07-Filters-Pipelines/17-exercises.md](../07-Filters-Pipelines/17-exercises.md). Often more than one correct answer exists; alternatives are shown where useful.

## grep

```bash
# 1. The line of user root (^ anchors to the start of the line,
#    the : avoids matching e.g. a user called "rootkit")
grep '^root:' /etc/passwd

# 2. Count accounts with bash as login shell ($ = end of line)
grep -c '/bin/bash$' /etc/passwd

# 3. Lines NOT containing nologin
grep -v 'nologin' /etc/passwd

# 4. Case-insensitive search with line numbers
grep -in 'ubuntu' /etc/os-release
```

## cut

```bash
# 5. First field, delimiter :
cut -d: -f1 /etc/passwd | head -5

# 6. Fields 1 and 6 (the delimiter is kept in the output)
cut -d: -f1,6 /etc/passwd | head -3

# 7. Lines starting with VERSION, then everything after the =
grep '^VERSION' /etc/os-release | cut -d= -f2

# 8. Characters 1 to 3 of every username
cut -d: -f1 /etc/passwd | cut -c1-3 | head -5
```

## wc

```bash
# 9. Number only: read via stdin, so wc does not know the filename
wc -l < /etc/passwd
# alternative:
cat /etc/passwd | wc -l

# 10. Lines, words and bytes (that is the default of wc)
wc /etc/os-release
# same as:
wc -l -w -c /etc/os-release
```

## tr

```bash
# 11. Lowercase to uppercase
cut -d: -f1 /etc/passwd | head -5 | tr 'a-z' 'A-Z'
# alternative:
cut -d: -f1 /etc/passwd | head -5 | tr '[:lower:]' '[:upper:]'

# 12. Replace every : by a newline, then count the lines
echo $PATH | tr ':' '\n'
echo $PATH | tr ':' '\n' | wc -l

# 13. Delete the dashes
date +%F | tr -d '-'

# 14. Squeeze repeated spaces into one, then cut field 5 (size) and 9 (name)
ls -l /etc/passwd /etc/group /etc/hostname | tr -s ' ' | cut -d' ' -f5,9
```

## sort

```bash
# 15. Reverse alphabetical order
cut -d: -f1 /etc/passwd | sort -r

# 16. Numeric sort on field 3, delimiter :
sort -t: -k3 -n /etc/passwd | cut -d: -f1,3 | head -5
```

**Without `-n`** the UIDs are sorted as text, character by character: `0, 1, 10, 100, 1000, 13, 2, 33, ...`. So `10` comes before `2` because the character `1` comes before `2`. With `-n` the values are compared as numbers.

## uniq

```bash
# 17. uniq only merges ADJACENT identical lines, so always sort first.
#     The second sort -nr puts the highest count on top.
cut -d: -f7 /etc/passwd | sort | uniq -c | sort -nr

# Extra: number of different shells
cut -d: -f7 /etc/passwd | sort -u | wc -l
# alternative:
cut -d: -f7 /etc/passwd | sort | uniq | wc -l

# 18. -d shows only lines that occur more than once
cut -d: -f4 /etc/passwd | sort | uniq -d
```

GID `65534` (group `nogroup`) is shared by `sync`, `_apt` and `nobody` (and `sshd` when OpenSSH server is installed).

## comm

```bash
# Preparation: comm needs SORTED input
cut -d: -f1 /etc/passwd | sort > ~/users.txt
cut -d: -f1 /etc/group  | sort > ~/groups.txt

# 19. Hide column 1 (only in users) and column 2 (only in groups)
#     -> only column 3 remains: lines in both files
comm -12 ~/users.txt ~/groups.txt

# 20. Hide column 2 and 3 -> only lines unique to users.txt
comm -23 ~/users.txt ~/groups.txt
comm -23 ~/users.txt ~/groups.txt | wc -l

# Clean up
rm ~/users.txt ~/groups.txt
```

**Reminder:** `comm` shows 3 columns: (1) only in file 1, (2) only in file 2, (3) in both. The options `-1`, `-2`, `-3` **hide** the corresponding column.

---

## Navigation

**Exercises:** [← Lesson 7 Exercises](../07-Filters-Pipelines/17-exercises.md)  
**Solutions Home:** [↑ Solutions](README.md)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
