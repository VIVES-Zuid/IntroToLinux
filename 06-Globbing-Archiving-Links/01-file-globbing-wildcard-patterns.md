# 1. File Globbing (Wildcard Patterns)


File globbing allows you to select multiple files using pattern matching. It's processed by the shell before the command runs.

### Basic Globbing Patterns:

#### Asterisk (*) - Match Any Characters
```bash
# Match all files
ls *

# Match files with specific extension
ls *.txt
ls *.pdf
ls *.jpg

# Match files starting with specific text
ls test*
ls backup*

# Match files with pattern in middle
ls *config*
ls *2024*
```

#### Question Mark (?) - Match Single Character
```bash
# Match files with single character variation
ls file?.txt          # file1.txt, file2.txt, fileA.txt
ls test?              # test1, test2, testA
ls ?.conf             # a.conf, 1.conf, x.conf

# Multiple question marks
ls file??.txt         # file01.txt, file23.txt, fileAB.txt
```

#### Square Brackets ([]) - Match Character Range
```bash
# Match specific characters
ls file[123].txt      # file1.txt, file2.txt, file3.txt
ls [abc]*             # Files starting with a, b, or c

# Match character ranges
ls file[0-9].txt      # file0.txt through file9.txt
ls [A-Z]*             # Files starting with uppercase letters
ls [a-z]*             # Files starting with lowercase letters
ls [0-9a-f]*          # Hexadecimal patterns

# Negate patterns
ls [!0-9]*            # Files NOT starting with digits
ls file[!a-c].txt     # file excluding filea.txt, fileb.txt, filec.txt
```

#### Brace Expansion ({}) - Generate Multiple Patterns
```bash
# Create multiple files
touch file{1,2,3}.txt                    # file1.txt, file2.txt, file3.txt
touch {backup,config,log}.txt            # backup.txt, config.txt, log.txt

# Numeric ranges
touch file{1..10}.txt                    # file1.txt through file10.txt
touch test{001..100}.log                 # test001.log through test100.log

# Character ranges
touch file{a..z}.txt                     # filea.txt through filez.txt

# Nested patterns
touch {backup,restore}_{2023,2024}.txt   # 4 files total
mkdir -p project/{src,docs,tests}/{2023,2024}
```

### Advanced Globbing:

#### Extended Globbing (bash)
Enable with `shopt -s extglob`:

```bash
# Enable extended globbing
shopt -s extglob

# Match one of several patterns
ls +(*.txt|*.doc)     # .txt OR .doc files
ls @(*.jpg|*.png)     # .jpg OR .png files

# Match zero or more occurrences
ls *(backup)          # Files containing zero or more "backup"

# Match zero or one occurrence
ls ?(test)*.txt       # test1.txt, 1.txt (optional "test")

# Match anything except pattern
ls !(*.tmp)           # All files except .tmp files
```

#### Globbing with Directories:
```bash
# Match files in subdirectories
ls */*.txt            # .txt files in immediate subdirectories
ls **/*.txt           # .txt files in all subdirectories (with globstar)

# Enable recursive globbing
shopt -s globstar
ls **/*.py            # All .py files recursively
```

### Practical Globbing Examples:

```bash
# Backup specific file types
cp *.{txt,doc,pdf} ~/backup/

# Remove temporary files
rm *.tmp *~ *.bak

# List files by date pattern
ls *2024*
ls *-01-*             # Files with "-01-" (January files)

# Find configuration files
ls *.conf
ls *config*
ls /etc/*.conf

# Work with image files
ls *.{jpg,jpeg,png,gif}
cp *.jpg ~/Pictures/

# Find log files
ls *.log
ls /var/log/*.log
tail -f /var/log/*.log
```
---

## Navigation

**Previous:** [← Learning Objectives](00-learning-objectives.md)  
**Next:** [→ Working With Blocks The Dd Command](02-working-with-blocks-the-dd-command.md)  
**Lesson Home:** [↑ Lesson 06: Globbing Archiving Links](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
