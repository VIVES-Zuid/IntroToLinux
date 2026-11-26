# 7. sort: Sorting Text

`sort` puts lines in order. By default it sorts **alphabetically**, on the whole line.

### Alphabetical vs Numeric: Why `-n` Matters

```
 Input      sort          sort -n
 ─────      ─────         ───────
  10         1             1
  2          10   ◀── !    2
  1          2             10
```

Without `-n`, `sort` compares **text** character by character: `"10"` starts with `"1"`, so it comes before `"2"`.
With `-n` it compares the **numbers**.

### Most Used Options

| Option | Meaning |
|--------|---------|
| `-n` | **n**umeric sort |
| `-r` | **r**everse (Z→A, big→small) |
| `-f` | ignore upper/lowercase (**f**old case) |
| `-u` | **u**nique: remove duplicate lines |
| `-t X` | use `X` as field separator (like `cut -d`) |
| `-k N` | sort on field (**k**ey) N |
| `-h` | **h**uman sizes: understands `2K < 1M < 3G` |

```bash
sort file.txt                  # alphabetical
sort -r file.txt               # reverse
sort -n numbers.txt            # numeric
sort -nr numbers.txt           # numeric, biggest first
sort -u file.txt               # sorted and without duplicates
sort -f names.txt              # ignore case
```

### Sorting on a Field: `-t` and `-k`

```
 sort -t: -k3 -n /etc/passwd

  -t:   split on ":"
  -k3   sort on field 3 (the UID)
  -n    compare as numbers

  root:x:0:...       ← UID 0
  daemon:x:1:...     ← UID 1
  bin:x:2:...        ← UID 2
  ...
  student:x:1000:... ← UID 1000
```

```bash
sort -t: -k3 -n /etc/passwd    # sort users by UID
sort -t, -k2 data.csv          # sort a CSV on the second column
sort -k2 file.txt              # sort on 2nd field (default separator: spaces)
```

> 💡 `-k2` means "from field 2 **to the end of the line**". `-k2,2` means "only field 2". Usually both give the same result; use `-k2,2` when you want to be precise.

### Human-Readable Sizes: `-h`

```bash
du -sh * | sort -h             # smallest first: 12K, 340K, 2.1M, 1.5G
du -sh * | sort -hr | head -5  # 5 biggest items
```

### Practical sort Examples

```bash
# Biggest files (field 5 of ls -l is the size)
ls -l | sort -k5 -nr | head -5

# Processes using the most memory (field 4 of ps aux is %MEM)
ps aux | sort -k4 -nr | head -5

# Log lines starting with an ISO date (2024-01-15 ...) sort correctly alphabetically
sort logfile.txt
```

---

## Navigation

**Next:** [→ Uniq Identify Unique And Duplicate Lines](08-uniq-identify-unique-and-duplicate-lines.md)  
**Previous:** [← Wc Word Line And Character Counting](06-wc-word-line-and-character-counting.md)  
**Lesson Home:** [↑ Lesson 7: Text Processing and Filters](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
