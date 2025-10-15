# Solutions for Lesson 6: File Operations, Globbing, and Archiving

## Lab 6.1: File Globbing Practice (15 minutes)

### Solution:

```bash
# 1. Create test files
mkdir globbing_test && cd globbing_test

# Create numbered text files:
touch file{1..10}.txt           # Creates file1.txt through file10.txt

# Create log files:
touch {backup,config,temp}.log  # Creates backup.log, config.log, temp.log

# Create data files:
touch test{a..e}.dat           # Creates testa.dat through teste.dat

# Create image files:
touch image{01..05}.{jpg,png}   # Creates image01.jpg, image01.png, etc.

# Verify file creation:
ls -la
# Should show 40+ files total

# 2. Practice basic globbing
ls *.txt                        # Lists all .txt files
# Output: file1.txt file2.txt ... file10.txt

ls *.log                        # Lists all .log files
# Output: backup.log config.log temp.log

ls file?.txt                    # Single character wildcard
# Output: file1.txt file2.txt ... file9.txt (not file10.txt - two digits)

ls file[1-5].txt               # Character range
# Output: file1.txt file2.txt file3.txt file4.txt file5.txt

ls *{1,5}*                     # Files containing 1 OR 5
# Output: file1.txt file10.txt file5.txt image01.jpg image01.png image05.jpg image05.png

# 3. Advanced patterns
ls *.{txt,log}                 # Multiple extensions
# Output: All .txt and .log files

ls file[!1-3].txt              # NOT in range 1-3
# Output: file4.txt file5.txt file6.txt file7.txt file8.txt file9.txt

ls *{jpg,png}                  # All jpg and png files
# Output: All image01-05.jpg and image01-05.png files

# Additional globbing examples:
ls file[13579].txt             # Odd numbers only
ls *[0-9][0-9]*                # Files with two consecutive digits
ls ???.txt                     # Exactly 3 characters before .txt

# 4. Directory patterns
mkdir {src,docs,test}           # Create directories

# Create files in subdirectories:
touch src/main.c src/utils.c src/header.h
touch docs/readme.md docs/manual.pdf docs/changelog.txt
touch test/test1.py test/test2.py test/runner.sh

ls */*.c                       # All .c files in subdirectories
# Output: src/main.c src/utils.c

ls */*.{md,txt}                # Multiple extensions in subdirs
# Output: docs/readme.md docs/changelog.txt

# If globstar is enabled (bash 4+):
shopt -s globstar              # Enable recursive globbing
ls **/*                        # All files recursively
ls **/*.py                     # All Python files recursively

cd ..                          # Return to parent directory

# Clean up:
rm -rf globbing_test
```

## Lab 6.2: Archiving Practice (20 minutes)

### Solution:

```bash
# 1. Create test data
mkdir archive_test && cd archive_test

# Create project structure:
mkdir -p project/{src,docs,config}

# Add content to files:
echo "Source code" > project/src/main.c
echo "Header file" > project/src/main.h
echo "Documentation" > project/docs/README.md
echo "User manual" > project/docs/manual.txt
echo "settings" > project/config/app.conf
echo "database_url=localhost" > project/config/db.conf

# Create some additional test files:
echo "Build script" > project/build.sh
echo "Project metadata" > project/project.info

# Verify structure:
tree project/ || find project/ -type f | sort
# Should show organized directory structure

# 2. Create archives
tar -cf project.tar project/            # Uncompressed tar
tar -czf project.tar.gz project/        # Gzip compressed
tar -cjf project.tar.bz2 project/       # Bzip2 compressed
zip -r project.zip project/             # ZIP format

# Verbose archive creation:
tar -cvf project_verbose.tar project/   # Show files being added

# 3. Compare sizes
ls -lh project.tar*
ls -lh project.zip
# Expected output showing size differences:
# project.tar.bz2 is smallest
# project.tar.gz is medium
# project.tar is largest
# project.zip varies

# Detailed size comparison:
du -h project.tar*
echo "Original directory size:"
du -sh project/

# 4. Test extraction
mkdir restore_test                      # Create extraction directory
tar -xzf project.tar.gz -C restore_test/  # Extract to specific directory

# Verify extraction:
diff -r project/ restore_test/project/   # Should show no differences
tree restore_test/ || find restore_test/ -type f | sort

# Test other extraction methods:
mkdir extract_tar && tar -xf project.tar -C extract_tar/
mkdir extract_bz2 && tar -xjf project.tar.bz2 -C extract_bz2/
mkdir extract_zip && unzip project.zip -d extract_zip/

# 5. Archive with exclusions
echo "temp" > project/temp.tmp          # Create temporary file
echo "backup" > project/backup~         # Create backup file

# Create clean archive excluding temp files:
tar -czf clean_project.tar.gz --exclude='*.tmp' --exclude='*~' project/

# Verify exclusion worked:
tar -tzf clean_project.tar.gz | grep -E '\.(tmp|~)$'  # Should show nothing

# More exclusion examples:
tar -czf source_only.tar.gz --exclude='docs' --exclude='config' project/
tar -czf docs_only.tar.gz project/docs/

cd ..                                   # Return to parent
```

## Lab 6.3: Links Practice (15 minutes)

### Solution:

```bash
# 1. Create test file
mkdir links_test && cd links_test
echo "Original content" > original.txt

# Verify initial state:
ls -li original.txt                     # Note the inode number
cat original.txt

# 2. Create links
ln original.txt hardlink.txt            # Hard link
ln -s original.txt softlink.txt         # Soft/symbolic link
ln -s ../links_test ../test_link        # Link to directory

# Create additional test links:
ln -s /etc/passwd system_passwd         # Absolute path link
ln -s nonexistent.txt broken_link       # Broken link

# 3. Examine links
ls -li                                  # Long listing with inodes
# Note: hardlink.txt has same inode as original.txt
# Note: softlink.txt shows -> original.txt

stat original.txt                       # Detailed file information
stat hardlink.txt                      # Should show same inode
stat softlink.txt                      # Shows link information

file softlink.txt                      # Identifies as symbolic link
file hardlink.txt                      # Identifies as regular file

# Check link targets:
readlink softlink.txt                  # Shows: original.txt
readlink ../test_link                  # Shows: ../links_test

# 4. Test link behavior
echo "Additional content" >> hardlink.txt  # Modify via hard link
cat original.txt                           # Shows modification
cat softlink.txt                          # Also shows modification

# Check file sizes and modification times:
ls -li original.txt hardlink.txt          # Same size, same inode
stat original.txt hardlink.txt | grep Size

# Add more content via soft link:
echo "Via soft link" >> softlink.txt
cat original.txt                          # Shows all modifications

# 5. Test deletion
cp original.txt backup.txt               # Create backup first
rm original.txt                         # Remove original file

cat hardlink.txt                        # Still works (data preserved)
# Output: Original content
#         Additional content
#         Via soft link

cat softlink.txt                        # Broken link - error
# Error: cat: softlink.txt: No such file or directory

ls -la softlink.txt                     # Shows broken link (red in color terminals)
file softlink.txt                      # Shows "broken symbolic link"

# Test directory link:
ls -la ../test_link                     # Should show directory contents
cd ../test_link                         # Should work
pwd                                     # Shows actual directory path
cd - && cd links_test                  # Return to test directory

# Restore original file to fix soft link:
cp backup.txt original.txt
cat softlink.txt                       # Now works again

cd ..                                  # Return to parent
```

## Lab 6.4: dd and Block Operations (10 minutes)

### Solution:

```bash
# 1. Create test files
mkdir dd_test && cd dd_test

# 2. Create files of specific sizes
dd if=/dev/zero of=1MB.file bs=1M count=1
# Output: 1+0 records in, 1+0 records out, 1048576 bytes (1.0 MB) copied

dd if=/dev/zero of=10MB.file bs=1M count=10
# Output: 10+0 records in, 10+0 records out, 10485760 bytes (10 MB) copied

# Create file with different block sizes:
dd if=/dev/zero of=1MB_4K.file bs=4K count=256  # 256 * 4K = 1MB

# 3. Check file sizes
ls -lh *.file
# Expected output:
# -rw-r--r-- 1 user user 1.0M Oct 15 10:30 1MB.file
# -rw-r--r-- 1 user user 1.0M Oct 15 10:30 1MB_4K.file
# -rw-r--r-- 1 user user  10M Oct 15 10:30 10MB.file

# Verify exact sizes:
stat -c%s *.file                        # Show exact byte counts
du -b *.file                           # Show disk usage in bytes

# 4. Copy with dd
dd if=1MB.file of=copy.file bs=4K
# Shows block-level copying statistics

# Compare original and copy:
diff 1MB.file copy.file                 # Should show no difference
md5sum 1MB.file copy.file              # Should show identical checksums

# Test different block sizes for performance:
time dd if=1MB.file of=copy_1K.file bs=1K
time dd if=1MB.file of=copy_64K.file bs=64K
# Larger block sizes are typically faster

# 5. Create random data
dd if=/dev/urandom of=random.dat bs=1K count=5
# Creates 5KB of random data

# Create sparse file (efficient for large files with holes):
dd if=/dev/zero of=sparse.file bs=1M seek=100 count=1
# Creates file that appears to be 101MB but uses minimal disk space

# 6. Verify results
ls -lh                                  # List all files with sizes
file *.file *.dat                      # Check file types
# Output should show:
# *.file: data
# random.dat: data

# Test file contents:
hexdump -C random.dat | head -5        # Show random data in hex
head -c 100 1MB.file | hexdump -C      # Show zeros in hex

# Advanced dd examples:
dd if=/dev/zero of=test.img bs=1M count=50  # Create disk image
dd if=test.img of=backup.img bs=64K         # Copy disk image

# Performance monitoring:
dd if=/dev/zero of=performance.test bs=1M count=100 oflag=dsync
# Test with synchronous writes (slower but more realistic)

cd ..                                   # Return to parent
```

## Advanced Examples and Real-World Usage:

```bash
# Complex globbing for file management:
find . -name "*.log" -mtime +7 -exec rm {} \;  # Remove old log files
ls **/*.{py,sh} 2>/dev/null | wc -l             # Count script files

# Professional archiving with metadata:
tar -czf backup_$(date +%Y%m%d_%H%M%S).tar.gz \
    --exclude-vcs \
    --exclude='*.tmp' \
    --exclude='*.log' \
    ~/important_project/

# Advanced link management:
find /usr/bin -type l -exec ls -la {} \; | head -10  # Find symbolic links
find . -type f -links +1                            # Find files with multiple hard links

# Disk imaging and forensics:
dd if=/dev/sda1 of=disk_image.img bs=4M status=progress  # Backup partition
dd if=disk_image.img of=/dev/sdb1 bs=4M                  # Restore partition
```

## Troubleshooting Common Issues:

```bash
# Issue 1: Globbing not expanding
echo *.txt                             # Shows literal *.txt if no matches
ls *.txt 2>/dev/null || echo "No .txt files found"

# Issue 2: Archive extraction fails
tar -tzf archive.tar.gz                # Test archive integrity first
tar -xzf archive.tar.gz --keep-old-files  # Don't overwrite existing files

# Issue 3: Broken symbolic links
find . -type l -exec test ! -e {} \; -print  # Find broken links
find . -type l -exec sh -c 'test -e "$1" || echo "Broken: $1"' _ {} \;

# Issue 4: dd operation too slow
dd if=/dev/zero of=test bs=1M count=100 status=progress  # Show progress
# Use larger block sizes for better performance

# Issue 5: Permission denied with hard links
# Hard links can't cross filesystem boundaries
# Hard links can't be created for directories (except by root)
```

## Verification Commands:

```bash
# Verify all lab exercises:
echo "=== Globbing Test ==="
mkdir temp_test && cd temp_test
touch file{1..5}.{txt,log}
ls *.txt | wc -l                        # Should show 5
ls file[1-3].* | wc -l                  # Should show 6
cd .. && rm -rf temp_test

echo "=== Archive Test ==="
mkdir temp_archive && cd temp_archive
echo "test" > test.txt
tar -czf test.tar.gz test.txt
tar -tzf test.tar.gz                    # Should list test.txt
cd .. && rm -rf temp_archive

echo "=== Links Test ==="
mkdir temp_links && cd temp_links
echo "content" > orig.txt
ln orig.txt hard.txt
ln -s orig.txt soft.txt
ls -li                                  # Compare inodes
cd .. && rm -rf temp_links

echo "=== dd Test ==="
dd if=/dev/zero of=test.dat bs=1K count=1 2>/dev/null
ls -lh test.dat                         # Should show 1.0K
rm test.dat
```

## Key Learning Points:

1. **Globbing**: Use wildcards to match multiple files efficiently
2. **Archives**: tar for Unix systems, zip for cross-platform compatibility
3. **Links**: Hard links share inodes, soft links are pointers to paths
4. **dd**: Powerful tool for block-level operations and disk imaging
5. **File Management**: Combine tools for efficient file organization

---

## Navigation

**Previous:** [← Lesson 5: Advanced Commands and Control Operators](lesson-05-lab-solutions.md)  
**Next:** [→ Lesson 7: Text Processing and Filters](lesson-07-lab-solutions.md)  
**Home:** [↑ Solutions Index](README.md)