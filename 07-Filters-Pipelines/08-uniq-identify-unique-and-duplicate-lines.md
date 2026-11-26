# 8. uniq: Identify Unique and Duplicate Lines

`uniq` removes or counts **repeated lines**, but only when the repeated lines are **next to each other**.
That's why you almost always use `sort` first.

### Why `sort` Comes First

```
 Without sort:                    With sort first:

 apple                            apple
 banana       uniq                apple        uniq
 apple       ─────▶ apple         banana      ─────▶ apple
                    banana                           banana
                    apple  ◀── still twice!
```

### The Classic Combination: `sort | uniq -c | sort -nr`

This pattern answers "**what occurs most often?**":

```
 fruit.txt     sort         uniq -c         sort -nr
 ─────────     ─────        ────────        ────────
 banana        apple        2 apple         3 banana   ◀── most frequent
 apple    ──▶  apple   ──▶  3 banana   ──▶  2 apple
 banana        banana       1 cherry        1 cherry
 cherry        banana
 apple         banana
 banana        cherry
```

```bash
sort fruit.txt | uniq -c | sort -nr
```

### Options

| Option | Meaning |
|--------|---------|
| *(none)* | show every line once |
| `-c` | **c**ount: put the number of occurrences in front |
| `-d` | only show lines that are **d**uplicated (appear more than once) |
| `-u` | only show lines that are **u**nique (appear exactly once) |
| `-i` | **i**gnore upper/lowercase |

```bash
sort file.txt | uniq         # remove duplicates (same as sort -u)
sort file.txt | uniq -c      # count occurrences
sort file.txt | uniq -d      # only the duplicates
sort file.txt | uniq -u      # only lines that appear once
```

### Practical uniq Examples

```bash
# Which login shells are used, and how often?
cut -d: -f7 /etc/passwd | sort | uniq -c | sort -nr

# How many different login shells are there?
cut -d: -f7 /etc/passwd | sort | uniq | wc -l

# Which GIDs appear more than once?
cut -d: -f4 /etc/passwd | sort | uniq -d

# Count error types in a log
grep "ERROR" log.txt | cut -d' ' -f4 | sort | uniq -c
```

> ℹ️ **Advanced** – `uniq -f1` skips the first field when comparing, `uniq -s5` skips the first 5 characters.

---

## Navigation

**Next:** [→ Comm Compare Sorted Files](09-comm-compare-sorted-files.md)  
**Previous:** [← Sort Sorting Text](07-sort-sorting-text.md)  
**Lesson Home:** [↑ Lesson 7: Text Processing and Filters](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
