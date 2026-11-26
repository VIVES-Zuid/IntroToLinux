# 3. grep: Pattern Matching Master

`grep` (**G**lobal **R**egular **E**xpression **P**rint) keeps only the lines that contain a pattern.

```
 sample.log                          grep "ERROR" sample.log
┌──────────────────────────────┐    ┌──────────────────────────────┐
│10:30 INFO  User login        │    │                              │
│10:31 ERROR Database down     │───▶│10:31 ERROR Database down     │
│10:32 WARNING Low disk        │    │                              │
│10:33 ERROR Timeout           │    │10:33 ERROR Timeout           │
└──────────────────────────────┘    └──────────────────────────────┘
        every line                    only lines containing ERROR
```

### Basic grep Usage

```bash
# Basic pattern search
grep "pattern" file.txt
grep "error" /var/log/syslog

# Search in the output of another command
ls -l /etc | grep "conf"
```

### The Most Important Options

| Option | Meaning | Example |
|--------|---------|---------|
| `-i` | **i**gnore case (`error` = `ERROR`) | `grep -i "error" log.txt` |
| `-v` | in**v**ert: lines that do **NOT** match | `grep -v "^#" config.txt` |
| `-n` | show line **n**umbers | `grep -n "TODO" script.sh` |
| `-c` | **c**ount matching lines (instead of showing them) | `grep -c "404" access.log` |
| `-w` | match whole **w**ords only (`cat` but not `category`) | `grep -w "cat" file.txt` |
| `-r` | **r**ecursive: search all files in a directory | `grep -r "TODO" ~/code/` |
| `-l` | only show fi**l**enames that contain a match | `grep -l "error" *.log` |
| `-o` | only show the matching part, not the whole line | `grep -o "[0-9]*" file.txt` |

```bash
# Examples
grep -i "error" logfile.txt        # error, Error, ERROR ...
grep -v "^#" config.txt            # remove comment lines
grep -c "bash" /etc/passwd         # how many lines contain bash?
grep -n "root" /etc/passwd         # show line number in front
```

> ⚠️ `grep -c` counts **lines**, not words. A line with the pattern three times counts as 1.

### Context and Multiple Patterns

```bash
# Several patterns: lines with pattern1 OR pattern2
grep -e "ERROR" -e "WARNING" file.txt

# Context lines around a match
grep -A 3 "pattern" file.txt   # 3 lines After
grep -B 3 "pattern" file.txt   # 3 lines Before
grep -C 3 "pattern" file.txt   # 3 lines before and after (Context)

# Fixed string: dots, stars ... are NOT special
grep -F "192.168.1.1" file.txt
```

### Basic Regular Expressions

A **regular expression** (regex) is a pattern that describes text. You only need a few symbols to get started:

| Symbol | Meaning | Example | Matches |
|--------|---------|---------|---------|
| `^` | start of the line | `^root` | lines that **start** with root |
| `$` | end of the line | `bash$` | lines that **end** with bash |
| `.` | exactly one character (any) | `c.t` | cat, cut, c9t |
| `[abc]` | one character from the list | `[Cc]at` | Cat, cat |
| `[0-9]` | one digit (a range) | `file[0-9]` | file1, file7 |
| `*` | the previous character 0 or more times | `go*d` | gd, god, good |

```
 Pattern:  ^root
           │└──┴── the text "root"
           └────── must be at the START of the line

 root:x:0:0:root:/root:/bin/bash      ✅ match
 sync:x:4:65534:sync:/bin:/bin/sync   ❌ no match ("root" not at start)
```

```bash
grep "^start" file.txt         # lines starting with "start"
grep "end$" file.txt           # lines ending with "end"
grep "^$" file.txt             # empty lines (start immediately followed by end)
grep "[0-9]" file.txt          # lines that contain a digit
grep "[A-Z]" file.txt          # lines that contain an uppercase letter
```

> ⚠️ Always put the pattern between **quotes**. Without quotes the shell may treat `*`, `[ ]` or `$` as its own special characters before `grep` even sees them.

### ℹ️ Advanced: Extended Regular Expressions (`-E`)

> ℹ️ **Advanced** – optional on a first read.

With `grep -E` you get extra symbols without needing backslashes:

| Symbol | Meaning | Example | Matches |
|--------|---------|---------|---------|
| `?` | previous character 0 or 1 time | `colou?r` | color, colour |
| `+` | previous character 1 or more times | `go+d` | god, good (not gd) |
| `\|` | OR | `cat\|dog` | cat or dog |
| `( )` | group | `(ab)+` | ab, abab |
| `{n,m}` | between n and m times | `[0-9]{1,3}` | 1 to 3 digits |

```bash
grep -E "colou?r" file.txt          # color or colour
grep -E "(ERROR|WARNING)" file.txt  # ERROR or WARNING
grep -E "^[0-9]{1,3}\." file.txt    # lines starting with 1-3 digits and a dot (start of an IP address)

# The same without -E needs backslashes:
grep "colou\?r" file.txt
grep "ERROR\|WARNING" file.txt
```

### Practical grep Examples

```bash
# System administration
grep "Failed password" /var/log/auth.log
grep -i "error" /var/log/syslog

# Code searching
grep -rn "TODO" ~/code/
grep -n -e "TODO" -e "FIXME" *.py

# Configuration files: show only the real settings (no comments, no empty lines)
grep -v "^#" /etc/ssh/sshd_config | grep -v "^$"
```

---

## Navigation

**Next:** [→ Cut Extract Fields And Characters](04-cut-extract-fields-and-characters.md)  
**Previous:** [← The Cat Command And Tee Utility](02-the-cat-command-and-tee-utility.md)  
**Lesson Home:** [↑ Lesson 7: Text Processing and Filters](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
