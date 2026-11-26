# 4. cut: Extract Fields and Characters

`cut` keeps only certain **columns** of every line. It works in two ways:

- **by field** (`-f`): split each line on a separator (`-d`) and keep some pieces
- **by character** (`-c`): keep characters at certain positions

### Cut by Field: `-d` (delimiter) and `-f` (field)

```
 cut -d: -f1,6 /etc/passwd

          -d:  = split on ":"
   ┌──────┬─┬─┬─┬────┬───────┬─────────┐
   │ root │x│0│0│root│ /root │/bin/bash│
   └──────┴─┴─┴─┴────┴───────┴─────────┘
  field:  1  2 3 4  5     6        7
          ▲                ▲
          └──── -f1,6 ─────┘

 Result:  root:/root
```

```bash
# Custom delimiter
cut -d: -f1 /etc/passwd        # usernames (field 1)
cut -d: -f1,3 /etc/passwd      # username and UID (fields 1 and 3)
cut -d: -f1-3 /etc/passwd      # fields 1 up to 3
cut -d: -f6- /etc/passwd       # field 6 until the end
cut -d, -f2 data.csv           # 2nd column of a CSV file

# Space as delimiter (quote it!)
cut -d' ' -f1 file.txt

# Without -d the default delimiter is TAB
cut -f1 file.txt
```

> 💡 The output keeps the same delimiter: `cut -d: -f1,3` gives `root:0`.

### Cut by Character Position: `-c`

```
 echo "2024-01-15" | cut -c1-4

  position: 1 2 3 4 5 6 7 8 9 10
            2 0 2 4 - 0 1 - 1 5
            └──┬──┘
            -c1-4  →  2024
```

```bash
cut -c1-5 file.txt             # characters 1 to 5
cut -c1,3,5 file.txt           # characters 1, 3 and 5
cut -c10- file.txt             # from character 10 to the end
cut -c-10 file.txt             # the first 10 characters

echo "Hello World" | cut -c1-5  # Hello
echo "2024-01-15" | cut -c1-4   # 2024
```

### ⚠️ The Multiple-Spaces Trap

`cut` treats **every** delimiter as a new field. If there are several spaces after each other (like in `ls -l`, `ps` or `df` output), you get empty fields:

```
 "a␣␣␣b"  with  cut -d' '   →  field1="a"  field2=""  field3=""  field4="b"
```

The fix: **squeeze** the spaces first with `tr -s ' '` (see next section):

```bash
ls -l /etc/passwd | tr -s ' ' | cut -d' ' -f5,9     # size and filename
```

### Practical cut Examples

```bash
# Usernames from /etc/passwd
cut -d: -f1 /etc/passwd

# Domain from a URL  (https: / "" / www.example.com / path)
echo "https://www.example.com/path" | cut -d/ -f3

# CSV data
cut -d, -f1,3 data.csv         # first and third column
cut -d, -f2- data.csv          # all columns except the first

# Date parts
echo "2024-01-15" | cut -d- -f1 # year: 2024
```

---

## Navigation

**Next:** [→ Tr Character Translation And Deletion](05-tr-character-translation-and-deletion.md)  
**Previous:** [← Grep Pattern Matching Master](03-grep-pattern-matching-master.md)  
**Lesson Home:** [↑ Lesson 7: Text Processing and Filters](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
