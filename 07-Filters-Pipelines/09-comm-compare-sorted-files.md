# 9. comm: Compare Sorted Files

`comm` compares two **sorted** files and shows the result in **three columns**:

```
 a.txt     b.txt
 ─────     ─────
 alice     bob
 bob       carol
 dave      dave

 $ comm a.txt b.txt
 col 1      col 2      col 3
 alice                             ← only in a.txt
                       bob         ← in both
            carol                  ← only in b.txt
                       dave        ← in both
```

The columns are separated by TABs, and the lines stay in sorted order.

Think of it as a Venn diagram:

```
      a.txt              b.txt
   ┌─────────┬───────┬─────────┐
   │ column 1│column3│ column 2│
   │  alice  │  bob  │  carol  │
   │         │  dave │         │
   └─────────┴───────┴─────────┘
    only in a  both   only in b
```

### Hiding Columns

The options **hide** a column. You say which columns you do **not** want to see:

| Command | Hides | Shows |
|---------|-------|-------|
| `comm -12 a b` | columns 1 and 2 | only lines in **both** files |
| `comm -23 a b` | columns 2 and 3 | only lines **only in a** |
| `comm -13 a b` | columns 1 and 3 | only lines **only in b** |

```bash
# Both input files MUST be sorted
sort file1.txt > sorted1.txt
sort file2.txt > sorted2.txt

comm sorted1.txt sorted2.txt         # all 3 columns
comm -12 sorted1.txt sorted2.txt     # common lines
comm -23 sorted1.txt sorted2.txt     # only in the first file
comm -13 sorted1.txt sorted2.txt     # only in the second file
```

> ⚠️ If a file is not sorted, `comm` gives wrong results (and usually a warning: `file 1 is not in sorted order`).

---

## Navigation

**Next:** [→ Sed Stream Editor](10-sed-stream-editor.md)  
**Previous:** [← Uniq Identify Unique And Duplicate Lines](08-uniq-identify-unique-and-duplicate-lines.md)  
**Lesson Home:** [↑ Lesson 7: Text Processing and Filters](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
