# 3. Input Redirection

## Input Redirection Overview

```
Default Input:                 Redirected Input:
┌─────────┐                   ┌─────────┐
│Keyboard │ ──── stdin ────▶  │Command  │
└─────────┘                   └─────────┘

         VS

┌─────────┐                   ┌─────────┐
│Input    │ ──── stdin ────▶  │Command  │
│File     │                   └─────────┘
└─────────┘
```

### Basic Input Redirection:

```bash
# Redirect stdin from file
command < file.txt
sort < unsorted.txt
wc < textfile.txt

# Here document (multiline input)
command << DELIMITER
line 1
line 2
DELIMITER

# Here string (single line input)
command <<< "input string"

# Examples
cat << EOF > poem.txt
Roses are red
Violets are blue
Linux is great
And so are you!
EOF

cat poem.txt
```

## Here Document Visualization

```
Here Document Flow:

cat << EOF > poem.txt
Roses are red        ┌─────────────────┐
Violets are blue ───▶│ Temporary       │───▶ poem.txt
Linux is great       │ Input Buffer    │
And so are you!      │ (in memory)     │
EOF                  └─────────────────┘

The << operator creates a temporary input stream until it sees "EOF"
```

## Input Redirection Types

```
Type          │ Syntax        │ Description
──────────────┼───────────────┼─────────────────────────
File Input    │ cmd < file    │ Read from file
Here Document │ cmd << EOF    │ Multiline input
              │ text          │ until EOF marker
              │ EOF           │
Here String   │ cmd <<< "str" │ Single string input
Pipe Input    │ cmd1 | cmd2   │ Output from cmd1

Visual Flow:
┌─────────┐     ┌─────────┐     ┌─────────┐
│ Input   │────▶│ stdin   │────▶│ Command │
│ Source  │     │ Stream  │     │ Process │
└─────────┘     └─────────┘     └─────────┘
```

### Practical Input Examples:

```bash
# Sort a list of names
cat << EOF > names.txt
Charlie
Alice
Bob
David
EOF

sort < names.txt
sort < names.txt > sorted_names.txt

# Count words in multiline input
wc -w << EOF
This is a test
of word counting
with here documents
EOF
```

## Complete Redirection Example

```
Complex Example: sort < input.txt > output.txt 2> errors.txt

┌─────────┐    stdin     ┌─────────┐    stdout    ┌─────────┐
│input.txt│ ───────────▶ │  sort   │ ───────────▶ │output   │
└─────────┘              │ command │              │.txt     │
                         │         │    stderr    └─────────┘
                         └─────────┘ ───────────▶ ┌─────────┐
                                                  │errors   │
                                                  │.txt     │
                                                  └─────────┘

Flow: File → Process → File (with error handling)
```

## Practical Use Cases

```bash
# Email-like input for scripts
mail user@example.com << EMAIL
Subject: System Report

The system is running normally.
Disk usage: 45%
Memory usage: 67%

Regards,
System Admin
EMAIL

# Configuration file creation
cat << CONFIG > app.conf
[database]
host=localhost
port=5432
name=myapp

[logging]
level=debug
file=/var/log/app.log
CONFIG

# SQL script execution
mysql -u user -p database << SQL
CREATE TABLE users (
    id INT PRIMARY KEY,
    name VARCHAR(50),
    email VARCHAR(100)
);

INSERT INTO users VALUES (1, 'John', 'john@example.com');
SQL
```

---

## Navigation

**Next:** [→ Pipes Chaining Commands](04-pipes-chaining-commands.md)  
**Previous:** [← Output Redirection](02-output-redirection.md)  
**Lesson Home:** [↑ Lesson 4: Redirects & Pipes](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
