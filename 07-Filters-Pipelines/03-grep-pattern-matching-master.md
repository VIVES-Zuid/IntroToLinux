# 3. grep: Pattern Matching Master


grep (Global Regular Expression Print) is one of the most powerful text searching tools.

### Basic grep Usage:

```bash
# Basic pattern search
grep "pattern" file.txt
grep "error" /var/log/syslog

# Case-insensitive search
grep -i "pattern" file.txt
grep -i "error" logfile.txt

# Show line numbers
grep -n "pattern" file.txt

# Count matches
grep -c "pattern" file.txt

# Show only filenames with matches
grep -l "pattern" *.txt

# Show filenames without matches
grep -L "pattern" *.txt
```

### Advanced grep Options:

```bash
# Invert match (lines NOT containing pattern)
grep -v "pattern" file.txt
grep -v "^#" config.txt        # Remove comment lines

# Recursive search
grep -r "pattern" directory/
grep -r "TODO" ~/code/

# Search multiple patterns
grep -e "pattern1" -e "pattern2" file.txt
grep "pattern1\|pattern2" file.txt

# Context lines
grep -A 3 "pattern" file.txt   # 3 lines after match
grep -B 3 "pattern" file.txt   # 3 lines before match  
grep -C 3 "pattern" file.txt   # 3 lines before and after

# Whole word matching
grep -w "word" file.txt

# Fixed string (no regex)
grep -F "literal.string" file.txt
```

### grep with Regular Expressions:

```bash
# Beginning of line
grep "^start" file.txt

# End of line
grep "end$" file.txt

# Any character
grep "c.t" file.txt            # cat, cut, cot, c@t, etc.

# Character classes
grep "[0-9]" file.txt          # Lines with digits
grep "[A-Z]" file.txt          # Lines with uppercase
grep "[aeiou]" file.txt        # Lines with vowels

# Repetition
grep "colou\?r" file.txt       # color or colour
grep "go*d" file.txt           # gd, god, good, goood, etc.
grep "go\+d" file.txt          # god, good, goood, etc.

# Extended regex (with -E)
grep -E "colou?r" file.txt     # Same as above, easier syntax
grep -E "(cat|dog)" file.txt   # cat OR dog
grep -E "^[0-9]{1,3}\." file.txt # IP address pattern
```

### Practical grep Examples:

```bash
# System administration
grep "Failed" /var/log/auth.log
grep -i "error" /var/log/syslog
ps aux | grep -v grep | grep firefox

# Log analysis
grep "$(date '+%Y-%m-%d')" /var/log/syslog
grep -c "404" /var/log/apache2/access.log

# Code searching
grep -r "function.*login" ~/code/
grep -n "TODO\|FIXME\|BUG" *.py

# Configuration files
grep -v "^#" /etc/ssh/sshd_config | grep -v "^$"
grep "Port" /etc/ssh/sshd_config
```
---

## Navigation

**Next:** [→ Cut Extract Fields And Characters](04-cut-extract-fields-and-characters.md)  
**Previous:** [← The Cat Command And Tee Utility](02-the-cat-command-and-tee-utility.md)  
**Lesson Home:** [↑ Lesson 7: Filters & Pipelines](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
