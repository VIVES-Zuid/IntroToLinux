# 6. wc: Word, Line, and Character Counting

`wc` (**w**ord **c**ount) counts lines, words and bytes.

```
 $ wc /etc/hostname
      1       1      9 /etc/hostname
      │       │      │       │
    lines   words  bytes  filename
```

| Option | Counts |
|--------|--------|
| `-l` | **l**ines |
| `-w` | **w**ords |
| `-c` | bytes |
| `-m` | characters |

> 💡 `-c` (bytes) and `-m` (characters) are the same for plain English text. They differ for characters such as `é` or emoji, which take more than one byte: `printf 'héllo' | wc -c` gives 6, `printf 'héllo' | wc -m` gives 5.

```bash
# Lines, words and bytes
wc file.txt

# Only lines / words / bytes / characters
wc -l file.txt
wc -w file.txt
wc -c file.txt
wc -m file.txt

# Several files: one line per file + a total
wc -l *.txt
```

### Only the Number, Without the Filename

```bash
wc -l /etc/passwd        # 27 /etc/passwd
wc -l < /etc/passwd      # 27            (wc reads stdin, so it doesn't know a filename)
```

### wc at the End of a Pipeline

`wc -l` is the classic last step to answer "**how many** ...?":

```bash
ps aux | wc -l                      # how many processes (+1 for the header line)
grep "error" log.txt | wc -l        # how many error lines
ls /etc | wc -l                     # how many items in /etc
```

---

## Navigation

**Next:** [→ Sort Sorting Text](07-sort-sorting-text.md)  
**Previous:** [← Tr Character Translation And Deletion](05-tr-character-translation-and-deletion.md)  
**Lesson Home:** [↑ Lesson 7: Text Processing and Filters](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
