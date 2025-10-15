# 8. Essential File Commands


### File and Directory Operations:

```bash
# List directory contents
ls
ls -l        # Long format with details
ls -la       # Include hidden files
ls -lh       # Human readable sizes

# Change directory
cd /path/to/directory
cd ..        # Go up one level
cd -         # Go to previous directory

# Print working directory
pwd

# Create files
touch filename.txt
touch file1.txt file2.txt file3.txt

# Create directories
mkdir dirname
mkdir -p path/to/nested/directories

# Copy files and directories
cp file1.txt file2.txt
cp -r directory1 directory2

# Move/rename files and directories
mv oldname.txt newname.txt
mv file.txt /path/to/destination/

# Remove files
rm filename.txt
rm -i filename.txt    # Interactive (ask before deleting)

# Remove directories
rmdir empty_directory
rm -r directory_with_contents
rm -rf directory      # Force remove (be careful!)
```

### File Content Commands:

```bash
# View file contents
cat filename.txt      # Display entire file
less filename.txt     # View with paging
more filename.txt     # View with paging (older)
head filename.txt     # First 10 lines
tail filename.txt     # Last 10 lines
head -20 filename.txt # First 20 lines
tail -20 filename.txt # Last 20 lines

# Edit files
nano filename.txt     # Simple text editor
```

### Download Files:

```bash
# Download files from internet
wget https://example.com/file.txt
wget -O newname.txt https://example.com/file.txt
```
---

## Navigation

**Previous:** [← Essential Navigation Commands](09-essential-navigation-commands.md)  
**Next:** [→ Getting Help With Commands](11-getting-help-with-commands.md)  
**Lesson Home:** [↑ Lesson 02: Shell](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
