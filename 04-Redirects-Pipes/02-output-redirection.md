# 2. Output Redirection

## Redirection Overview

```
┌─────────┐                    ┌─────────┐
│Command  │ ──── stdout ────▶  │Terminal │  (default)
│         │ ──── stderr ────▶  │Screen   │
└─────────┘                    └─────────┘

         VS

┌─────────┐                    ┌─────────┐
│Command  │ ──── stdout ────▶  │Output   │  (redirected)
│         │                    │File     │
│         │ ──── stderr ────▶  └─────────┘
└─────────┘                    ┌─────────┐
                               │Error    │
                               │File     │
                               └─────────┘
```

### Basic Output Redirection:

```bash
# Redirect stdout to file (overwrite)
command > file.txt
ls > directory_listing.txt
date > current_time.txt

# Redirect stdout to file (append)
command >> file.txt
echo "New line" >> file.txt
date >> log.txt

# Examples
echo "Hello World" > greeting.txt
cat greeting.txt

ps aux > processes.txt
wc -l processes.txt
```

## Redirection Operators Visual Guide

```
Operator │ Description          │ Visual
─────────┼─────────────────────┼──────────────────────
>        │ Redirect stdout     │ cmd ───▶ file (new)
>>       │ Append stdout       │ cmd ───▶ file (add)
2>       │ Redirect stderr     │ cmd ╫══▶ file (err)
2>>      │ Append stderr       │ cmd ╫══▶ file (err+)
&>       │ Both stdout+stderr  │ cmd ═══▶ file (all)
2>&1     │ stderr to stdout    │ cmd ╫─┐
         │                     │      ├─▶ file
         │                     │ cmd ──┘
```

### Error Redirection:

```bash
# Redirect stderr to file
command 2> error.txt
ls /nonexistent 2> errors.txt

# Redirect stderr to append
command 2>> error.txt

# Redirect both stdout and stderr to same file
command > output.txt 2>&1
command &> output.txt        # Shorter syntax (bash)

# Redirect stdout and stderr to different files
command > output.txt 2> error.txt

# Examples
find / -name "*.txt" > found.txt 2> errors.txt
cat found.txt
cat errors.txt
```

## Complex Redirection Example

```
┌──────────────────────────────────────────────────────────┐
│ find /usr -name "*.txt" > found.txt 2> errors.txt       │
└──────────────────────────────────────────────────────────┘
                    │                      │
                    ▼                      ▼
            ┌─────────────┐        ┌─────────────┐
            │ found.txt   │        │ errors.txt  │
            │             │        │             │
            │/usr/share/  │        │find: /usr/  │
            │doc/file.txt │        │private:     │
            │/usr/lib/    │        │Permission   │
            │help.txt     │        │denied       │
            └─────────────┘        └─────────────┘
```

### Discarding Output:

```bash
# Discard stdout (send to null device)
command > /dev/null
ls > /dev/null

# Discard stderr
command 2> /dev/null
ls /nonexistent 2> /dev/null

# Discard both stdout and stderr
command > /dev/null 2>&1
command &> /dev/null
```

## The Black Hole: /dev/null

```
┌─────────┐                    ┌─────────┐
│Command  │ ──── output ────▶  │/dev/null│ ──▶ ∅ (vanishes)
│         │                    │"BitBucket"│
└─────────┘                    └─────────┘

Why use /dev/null?
• Suppress unwanted output
• Hide error messages
• Performance (no disk I/O)
• Clean scripts
```

---

## Navigation

**Next:** [→ Input Redirection](03-input-redirection.md)  
**Previous:** [← Understanding Standard Streams](01-understanding-standard-streams.md)  
**Lesson Home:** [↑ Lesson 4: Redirects & Pipes](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
