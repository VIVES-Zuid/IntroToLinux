# 5. tr: Character Translation and Deletion

`tr` (**tr**anslate) works on single **characters**, not on words. It can do three things:

| Mode | Syntax | What it does |
|------|--------|--------------|
| translate | `tr 'SET1' 'SET2'` | replace each character of SET1 by the matching one in SET2 |
| delete | `tr -d 'SET'` | delete every character in SET |
| squeeze | `tr -s 'SET'` | turn repeated characters into one |

> ⚠️ `tr` only reads from **stdin**. You can't give it a filename: use `tr ... < file.txt` or `cat file.txt | tr ...`.

### Character Translation

The characters are matched **by position**:

```
 tr 'abc' 'xyz'

   a ──▶ x
   b ──▶ y
   c ──▶ z

 "cabbage" ──▶ "zxyyxge"
```

```bash
# Lowercase to uppercase
echo "hello world" | tr 'a-z' 'A-Z'               # HELLO WORLD
echo "hello world" | tr '[:lower:]' '[:upper:]'   # same, also works for other alphabets

# Uppercase to lowercase
echo "HELLO WORLD" | tr 'A-Z' 'a-z'

# Replace one character by another
echo "hello world" | tr ' ' '_'    # hello_world
echo "hello,world" | tr ',' ' '    # hello world

# Put every item of PATH on its own line
echo "$PATH" | tr ':' '\n'
```

> ⚠️ `tr '123' 'abc'` does **not** replace the word "123" by "abc". It replaces every `1` by `a`, every `2` by `b` and every `3` by `c`. For replacing words use `sed` (section 10).

### Character Deletion: `-d`

```bash
echo "hello world" | tr -d ' '               # helloworld
echo "abc123def" | tr -d '0-9'               # abcdef
echo "Hello123World!" | tr -d '[:digit:]'    # HelloWorld!
echo "Hello, World!" | tr -d '[:punct:]'     # Hello World
```

### Character Squeezing: `-s`

```
 tr -s ' '

 "hello␣␣␣␣world"  ──▶  "hello␣world"
```

```bash
echo "hello    world" | tr -s ' '   # hello world
echo "aaaabbbbcccc" | tr -s 'abc'   # abc

# Very common: clean up columns before using cut
df -h | tr -s ' ' | cut -d' ' -f1,5
```

### Character Classes

| Class | Means |
|-------|-------|
| `[:lower:]` | lowercase letters |
| `[:upper:]` | uppercase letters |
| `[:digit:]` | digits 0-9 |
| `[:alpha:]` | letters |
| `[:alnum:]` | letters and digits |
| `[:space:]` | spaces, tabs, newlines |
| `[:punct:]` | punctuation `.,!?;:` ... |

### Practical tr Examples

```bash
# Convert Windows line endings (\r\n) to Linux (\n)
tr -d '\r' < windows.txt > linux.txt

# Tabs to commas (TSV to CSV)
tr '\t' ',' < data.tsv

# Spaces in a filename to underscores
echo "file name with spaces.txt" | tr ' ' '_'   # file_name_with_spaces.txt

# Squeeze blank lines (several \n in a row become one)
tr -s '\n' < file.txt
```

### ℹ️ Advanced tr tricks

> ℹ️ **Advanced** – optional on a first read.

```bash
# -c = complement: work on everything that is NOT in the set
echo "Hello, World! 123" | tr -cd '[:alnum:]'    # HelloWorld123  (delete everything except letters/digits)

# ROT13 "encryption" (every letter shifted 13 places)
echo "hello" | tr 'a-zA-Z' 'n-za-mN-ZA-M'        # uryyb
```

---

## Navigation

**Next:** [→ Wc Word Line And Character Counting](06-wc-word-line-and-character-counting.md)  
**Previous:** [← Cut Extract Fields And Characters](04-cut-extract-fields-and-characters.md)  
**Lesson Home:** [↑ Lesson 7: Text Processing and Filters](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
