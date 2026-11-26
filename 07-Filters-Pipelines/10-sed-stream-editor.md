# 10. sed: Stream Editor

`sed` (**s**tream **ed**itor) edits text while it flows through a pipeline. You will mostly use it for **search and replace**.

### The Substitute Command: `s/old/new/`

```
 sed 's/cat/dog/g'
      │ │   │   │
      │ │   │   └── g = global: replace ALL matches on the line (not only the first)
      │ │   └────── replacement text
      │ └────────── text (pattern) to search for
      └──────────── s = substitute
```

```
 Input:  "the cat sat on the cat mat"

 sed 's/cat/dog/'    →  "the dog sat on the cat mat"   (only the 1st per line)
 sed 's/cat/dog/g'   →  "the dog sat on the dog mat"   (all of them)
```

```bash
sed 's/old/new/' file.txt         # replace first occurrence on each line
sed 's/old/new/g' file.txt        # replace all occurrences
sed 's/old/new/2' file.txt        # replace only the 2nd occurrence on each line
sed 's/old/new/gi' file.txt       # all occurrences, ignore case
```

> 💡 `sed` does **not** change the file by default. It prints the result on the screen. Use `> newfile.txt` to save it, or `-i` (see below).

### Delete and Print Lines

```bash
# Delete lines
sed '/pattern/d' file.txt         # delete lines that contain pattern
sed '1d' file.txt                 # delete line 1 (e.g. a header)
sed '$d' file.txt                 # delete the last line
sed '1,5d' file.txt               # delete lines 1 to 5

# Print only certain lines (-n = don't print everything automatically)
sed -n '1,5p' file.txt            # print lines 1 to 5
sed -n '/pattern/p' file.txt      # print lines matching pattern (like grep)
```

### Editing a File In-Place: `-i`

```bash
sed -i 's/old/new/g' file.txt     # ⚠️ changes the file directly, no undo!
sed -i.bak 's/old/new/g' file.txt # safer: keeps the original as file.txt.bak
```

### ℹ️ Advanced sed

> ℹ️ **Advanced** – optional on a first read.

```bash
# Several substitutions in one go
sed -e 's/old1/new1/g' -e 's/old2/new2/g' file.txt

# Another delimiter: handy when the text contains slashes
sed 's|/old/path|/new/path|g' file.txt

# Insert / append lines
sed '1i\New first line' file.txt  # insert before line 1
sed '$a\New last line' file.txt   # append after the last line

# Capture groups: \( \) remembers a part, \1 puts it back
echo "User: john@example.com" | sed 's/User: \([^@]*\)@.*/Username: \1/'   # Username: john
```

---

## Navigation

**Next:** [→ Building Powerful Pipelines](11-building-powerful-pipelines.md)  
**Previous:** [← Comm Compare Sorted Files](09-comm-compare-sorted-files.md)  
**Lesson Home:** [↑ Lesson 7: Text Processing and Filters](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
