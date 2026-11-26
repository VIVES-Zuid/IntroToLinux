# 11. Building Powerful Pipelines

A pipeline is a chain of small filters. Almost every pipeline follows the same pattern:

```mermaid
graph LR
    A["📄 Input<br/>cat / a command"] --> B["🔍 Select lines<br/>grep"]
    B --> C["✂️ Select columns<br/>cut, tr"]
    C --> D["🔤 Sort<br/>sort"]
    D --> E["🔢 Count / summarize<br/>uniq -c, wc -l"]
    E --> F["🏆 Show result<br/>sort -nr, head"]
```

### Step by Step: Which IP Addresses Cause the Most 404 Errors?

Our (simplified) web server log `access.log`:

```
192.168.1.10 200
192.168.1.20 404
192.168.1.10 404
192.168.1.30 404
192.168.1.10 404
192.168.1.40 200
192.168.1.30 404
192.168.1.10 404
```

The full pipeline:

```bash
grep "404" access.log | cut -d' ' -f1 | sort | uniq -c | sort -nr | head -3
```

What happens at every step:

```
 Step 1: grep "404"     Step 2: cut -d' ' -f1   Step 3: sort
┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐
│192.168.1.20 404  │   │192.168.1.20      │   │192.168.1.10      │
│192.168.1.10 404  │──▶│192.168.1.10      │──▶│192.168.1.10      │
│192.168.1.30 404  │   │192.168.1.30      │   │192.168.1.10      │
│192.168.1.10 404  │   │192.168.1.10      │   │192.168.1.20      │
│192.168.1.30 404  │   │192.168.1.30      │   │192.168.1.30      │
│192.168.1.10 404  │   │192.168.1.10      │   │192.168.1.30      │
└──────────────────┘   └──────────────────┘   └──────────────────┘
  only the 404 lines      only the IP address     same IPs together
                                                          │
          ┌───────────────────────────────────────────────┘
          ▼
 Step 4: uniq -c        Step 5: sort -nr        Step 6: head -3
┌──────────────────┐   ┌──────────────────┐   ┌──────────────────┐
│ 3 192.168.1.10   │   │ 3 192.168.1.10   │   │ 3 192.168.1.10   │
│ 1 192.168.1.20   │──▶│ 2 192.168.1.30   │──▶│ 2 192.168.1.30   │
│ 2 192.168.1.30   │   │ 1 192.168.1.20   │   │ 1 192.168.1.20   │
└──────────────────┘   └──────────────────┘   └──────────────────┘
  count per IP           biggest count first     top 3 only
```

### 🛠️ How to Build a Pipeline Yourself

Never write a long pipeline in one go. Add **one command at a time** and look at the output after each step:

```bash
grep "404" access.log                                   # 1. does the filter work?
grep "404" access.log | cut -d' ' -f1                   # 2. is this the right column?
grep "404" access.log | cut -d' ' -f1 | sort            # 3. sorted?
grep "404" access.log | cut -d' ' -f1 | sort | uniq -c  # 4. counted?
# ... and so on. Use the ↑ arrow key to recall the previous command and add one step.
```

### Long Pipelines Over Several Lines

When a line ends with `|`, the shell knows the command continues on the next line. You can even put a comment after the pipe:

```bash
grep -v "^#" data.csv |   # remove comment lines
    cut -d, -f2 |         # take column 2
    grep -v "^$" |        # remove empty lines
    sort |                # sort
    uniq -c |             # count occurrences
    sort -nr |            # most frequent first
    head -10              # top 10
```

> ⚠️ **Common mistake:** `sort | \   # comment`. A backslash `\` only continues the line when it is the **very last character**. With spaces and a comment after it, the pipeline silently breaks. End the line with `|` instead and you don't need a backslash.

### Example: Word Frequency of a Text

```bash
cat document.txt |
    tr '[:upper:]' '[:lower:]' |   # everything lowercase
    tr -d '[:punct:]' |            # remove punctuation
    tr ' ' '\n' |                  # one word per line
    grep -v "^$" |                 # remove empty lines
    sort |                         # same words together
    uniq -c |                      # count every word
    sort -nr |                     # most frequent first
    head -20 > word_frequency.txt  # save the top 20
```

### Example: Users and Their Shells

```bash
# Which users can log in with bash, alphabetically?
grep "/bin/bash$" /etc/passwd | cut -d: -f1 | sort

# How many accounts use each shell?
cut -d: -f7 /etc/passwd | sort | uniq -c | sort -nr
```

### ℹ️ Advanced: Pipelines on Command Output

> ℹ️ **Advanced** – optional on a first read. Command output such as `ps` or `df` uses multiple spaces between columns, so squeeze them with `tr -s ' '` before using `cut`.

```bash
# Top 5 processes by memory: %MEM (field 4) and command (field 11)
ps aux | sort -k4 -nr | head -5 | tr -s ' ' | cut -d' ' -f4,11

# Today's errors in syslog (Ubuntu 24.04 log lines start with an ISO date: 2024-01-15T10:30:45...)
grep "^$(date +%F)" /var/log/syslog | grep -i "error" | wc -l
```

---

## Navigation

**Next:** [→ Practical Labs](12-practical-labs.md)  
**Previous:** [← Sed Stream Editor](10-sed-stream-editor.md)  
**Lesson Home:** [↑ Lesson 7: Text Processing and Filters](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
