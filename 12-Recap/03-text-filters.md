# 3. Text Filters

---

## 3.1 Vertical Filtering: `head`, `tail`, `grep`

These commands select **which lines** to keep.

### `head` and `tail`

```bash
head -n 5 file.txt          # first 5 lines
tail -n 5 file.txt          # last 5 lines
tail -n +3 file.txt         # all lines starting from line 3 (skip first 2)
```

### `grep`

```bash
grep "error" file.txt       # lines containing "error"
grep -i "error" file.txt    # case-insensitive
grep -v "error" file.txt    # lines NOT containing "error"
grep -c "error" file.txt    # count of matching lines
grep -m 1 "error" file.txt  # stop after first match
grep -n "error" file.txt    # show line numbers
```

### Exercises

**3.1** Show the last 3 lines of `/var/log/dpkg.log`.

**3.2** Search `/var/log/dpkg.log` for the **first** occurrence of the word "error" (case-insensitive). Display only that one line.

**3.3** Show only the lines in `/etc/passwd` that do **not** contain the word `nologin`.

---

## 3.2 Horizontal Filtering: `cut`

`cut` extracts **columns or character ranges** from each line.

```bash
cut -d':' -f1 /etc/passwd      # field 1, delimiter ':'  → username
cut -d':' -f1,6 /etc/passwd    # fields 1 and 6
cut -d',' -f2- data.csv        # field 2 to end, comma delimiter
cut -c1-5 file.txt             # characters 1 to 5
```

### Exercises

**3.4** From `/etc/passwd`, extract **only the username** (field 1, delimiter `:`).

**3.5** From `/etc/passwd`, extract the **username and home directory** (fields 1 and 6, delimiter `:`).

---

## 3.3 Combining Vertical and Horizontal Filtering

Chain `grep` (select rows) with `cut` (select columns) using a pipe.

```bash
grep "bash" /etc/passwd | cut -d':' -f1     # usernames that use bash
grep "^root" /etc/passwd | cut -d':' -f6   # home dir of root
```

### Exercises

**3.6** From `/etc/passwd`, show the **home directory** (field 6) of every user whose shell is `/bin/bash`.

**3.7** From `/etc/group`, show the **group name** (field 1) of all groups that have at least one member (field 4 is not empty). Use `grep` then `cut`.

---

## 3.4 `tr` — Character Translation and Deletion

`tr` works on **characters**, not words or lines. It always reads from stdin.

```bash
tr 'a-z' 'A-Z' < file.txt          # convert lowercase to uppercase
tr ' ' '_' < file.txt              # replace spaces with underscores
tr -d '\n' < file.txt              # delete all newlines (join all lines)
tr -d ' ' < file.txt              # delete all spaces
tr -s ' ' < file.txt              # squeeze multiple spaces into one
```

> Common exam pattern: **join all lines into one AND replace spaces**  
> `tr -d '\n' < file1 | tr ' ' '_' > output.txt`  
> Order matters: delete newlines first, then replace spaces.

### Exercises

**3.8** Convert all text in `names.txt` to **uppercase** and save to `upper.txt`.

**3.9** Replace all spaces in `sentence.txt` with underscores **and** join all lines into a single line. Write the result to `oneline.txt`.

**3.10** From `data.txt`, remove all occurrences of the colon character `:` and display the result on screen.

---

## 3.5 `sort` and `uniq -c`

### `sort`

```bash
sort file.txt                # alphabetical
sort -n file.txt             # numeric sort
sort -r file.txt             # reverse
sort -nr file.txt            # reverse numeric (largest first)
sort -k2 file.txt            # sort by column 2 (whitespace-delimited)
sort -t':' -k3 -n file.txt  # sort by field 3, using ':' as delimiter
```

### `uniq`

`uniq` collapses **consecutive** identical lines. Always `sort` first!

```bash
sort file.txt | uniq           # remove duplicate lines
sort file.txt | uniq -c        # count occurrences (count is first column)
sort file.txt | uniq -c | sort -nr   # most frequent first
```

### Exercises

**3.11** From the file `words.txt`, show each unique word and how many times it appears, sorted from **most to least frequent**.

**3.12** Count the number of files in `/boot` per file extension (e.g., `.conf`, `.img`). Sort from most to least frequent. Suppress errors.

> Hint: `find /boot -type f` gives the filenames. Use `sed` or `cut` to extract the extension, then `sort | uniq -c | sort -nr`.

**3.13** Show all files in `/tmp` sorted by **modification time**, oldest first. (Hint: `ls -ltr`)

---

## 3.6 Sort on Columns (`-k`)

`sort -k N` sorts on the Nth whitespace-delimited field.

```bash
ls -l /tmp | sort -k5 -n        # sort ls output by file size (col 5)
ls -l /tmp | sort -k5 -rn       # largest first
ls -ltr /tmp | tail -n +2        # remove the 'total' header line
```

### Exercises

**3.14** List `/var/log` in long format and sort by file size, **largest first**.

---

## 3.7 `comm` — Lines Common to Two Files

`comm` compares **two sorted** files line by line.

```bash
comm file1.txt file2.txt     # 3 columns: only-in-1, only-in-2, in-both
comm -12 file1.txt file2.txt # suppress col 1 and 2 → only lines in BOTH
comm -23 file1.txt file2.txt # only lines in file1 (not in file2)
comm -13 file1.txt file2.txt # only lines in file2 (not in file1)
```

> ⚠️ Both files **must be sorted** before using `comm`. If they are not, sort inline:  
> `comm -12 <(sort file1.txt) <(sort file2.txt)`

### Exercises

**3.15** Display only the lines that appear in **both** `list_a.txt` and `list_b.txt`. Both files are already sorted.

**3.16** Display lines that are in `list_a.txt` but **not** in `list_b.txt`. Both files are already sorted.

---

## 3.8 `wc` — Counting Lines, Words and Characters

`wc` counts lines, words, or characters. Its real power is as the **last step in a pipeline**.

```bash
wc -l file.txt                        # count lines in a file
wc -w file.txt                        # count words
wc -c file.txt                        # count bytes/characters

# Pipeline usage — the most common exam pattern:
ls /etc | wc -l                       # how many entries in /etc?
grep -i "error" app.log | wc -l       # how many error lines?
ps -e --no-headers | wc -l            # how many running processes?
find /etc -type f 2>/dev/null | wc -l # how many regular files in /etc?
```

> ℹ️ `grep -c "pattern" file` is a shortcut for `grep "pattern" file | wc -l` when you only need a count of matching lines.

### Exercises

**3.19** Count how many lines are in `/etc/passwd`.

**3.20** Count how many processes are currently running (use `ps -e --no-headers`).

**3.21** Count how many **regular files** exist in `/var/log`. Suppress permission errors.

**3.22** Count how many lines in `/var/log/dpkg.log` contain the word "error" (case-insensitive). Give two different solutions: one using `wc -l` in a pipeline, one using a `grep` flag directly.

---

## 3.9 `awk` — Column-Based Processing

`awk` processes text field by field. Each field is `$1`, `$2`, etc. `NR` is the current line number.

```bash
awk '{print $1}' file.txt           # print first field (space-delimited)
awk -F',' '{print $3}' data.csv     # print 3rd field, comma-delimited
awk -F',' '{print NR, $2}' data.csv # line number + 2nd field
awk -F',' '{print "Col 3: (" NR ") " $3 "!"}' data.csv
```

> `awk` is powerful but the exam only tests basic column extraction and formatted output.

### Exercises

**3.23** From `data.csv` (comma-separated), print the **third column** of every row, preceded by `"Column 3: "`, the line number in parentheses, and an exclamation mark at the end.  
Expected format per line: `Column 3: (1) Score!`

**3.24** From `/etc/passwd` (delimiter `:`), use `awk` to print each username (field 1) and their shell (field 7) in the format: `user -> shell`.

---

## Navigation

**Next:** [→ Links and Permissions](04-links-and-permissions.md)
**Previous:** [← I/O, Redirects and Pipelines](02-io-redirects-and-pipelines.md)
**Lesson Home:** [↑ Lesson 12: Exam Preparation Recap](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
