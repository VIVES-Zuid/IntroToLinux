# Solutions for Lesson 2: The Linux Console and Terminal

## Exercise 1: File System Exploration

### Solution:

```bash
# 1. Start in your home directory
cd ~
pwd  # Should show /home/username

# 2. Navigate to the root directory
cd /
pwd  # Should show /

# 3. List the contents and identify key directories
ls -la
# Key directories you should see:
# /bin - Essential command binaries
# /etc - Configuration files
# /home - User home directories
# /usr - User programs and data
# /var - Variable data files
# /tmp - Temporary files

# 4. Go to /usr/bin and see the available programs
cd /usr/bin
ls | head -20  # Show first 20 programs
ls | wc -l     # Count total programs available

# 5. Return to your home directory
cd ~
# Alternative: cd (without arguments)
# Alternative: cd $HOME
pwd  # Verify you're back home
```

## Exercise 2: File Operations

### Solution:

```bash
# 1. Create a directory called linux_practice
mkdir linux_practice
ls -la  # Verify directory was created

# 2. Navigate into this directory
cd linux_practice
pwd  # Should show /home/username/linux_practice

# 3. Create three files: notes.txt, commands.txt, practice.txt
touch notes.txt commands.txt practice.txt
# Alternative method:
# touch notes.txt
# touch commands.txt
# touch practice.txt

# 4. List the files with detailed information
ls -la
# Should show:
# -rw-rw-r-- 1 username username 0 date time notes.txt
# -rw-rw-r-- 1 username username 0 date time commands.txt
# -rw-rw-r-- 1 username username 0 date time practice.txt

# 5. Create a backup directory and copy the files there
mkdir backup
cp *.txt backup/
# Alternative:
# cp notes.txt commands.txt practice.txt backup/

# Verify the backup
ls -la backup/
```

## Exercise 3: Command Help

### Solution:

```bash
# 1. Use man to learn about the ls command
man ls
# Key sections to read:
# - SYNOPSIS: Shows command syntax
# - DESCRIPTION: Explains what the command does
# - OPTIONS: Lists all available options

# 2. Find at least 3 different options for ls
# Here are 3 useful options:
# -l : long format (detailed information)
# -a : show all files (including hidden)
# -h : human-readable sizes

# 3. Try each option and observe the differences
ls          # Basic listing
ls -l       # Long format with permissions, size, date
ls -a       # Shows hidden files (starting with .)
ls -la      # Combination: long format + all files
ls -lah     # Long format + all files + human-readable sizes

# 4. Use man to learn about the cp command
man cp
# Key points from cp manual:
# - cp SOURCE DEST: Copy file to new location
# - cp SOURCE... DIRECTORY: Copy multiple files to directory
# - -r : recursive (for directories)
# - -i : interactive (ask before overwriting)
# - -v : verbose (show what's being copied)

# Test cp options:
cp -v notes.txt notes_copy.txt     # Verbose copy
cp -i notes.txt commands.txt       # Interactive (asks before overwrite)
mkdir test_dir
cp -r backup/ test_dir/            # Recursive copy of directory
```

## Additional Practice Commands:

```bash
# Navigation shortcuts
pwd          # Print working directory
cd ~         # Go to home directory
cd ..        # Go up one directory
cd -         # Go to previous directory

# File information
file filename.txt    # Show file type
stat filename.txt    # Detailed file information
which ls            # Show location of command

# Getting help
man command         # Manual page
command --help      # Quick help
info command        # Info documentation
whatis command      # Brief description
```

## Verification Commands:

```bash
# Check if exercises were completed correctly:
echo "=== Exercise 1 Verification ==="
cd /
ls -la | grep -E "(bin|etc|home|usr|var|tmp)"

echo "=== Exercise 2 Verification ==="
cd ~/linux_practice
ls -la
ls backup/

echo "=== Exercise 3 Verification ==="
ls -la
ls -lah
```

## Common Mistakes to Avoid:

1. **Case sensitivity**: Linux is case-sensitive. `File.txt` and `file.txt` are different.
2. **Spaces in filenames**: Use quotes or escape spaces: `"my file.txt"` or `my\ file.txt`
3. **Permissions**: If you get "Permission denied", check file permissions with `ls -la`
4. **Current directory**: Always verify where you are with `pwd`
5. **Overwriting files**: Use `-i` option with `cp` to avoid accidentally overwriting files

## Key Learning Points:

1. **Navigation**: Master directory traversal and file system exploration
2. **File Operations**: Create, copy, move, and delete files and directories efficiently
3. **Help System**: Use man pages and --help options to learn about commands
4. **Command Structure**: Understand syntax, options, and arguments
5. **Safety**: Always verify location with `pwd` and use tab completion

---

## Navigation

**Next:** [→ Lesson 3: Shell Environment and Variables](lesson-03-lab-solutions.md)  
**Home:** [↑ Solutions Index](README.md)