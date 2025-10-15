# 3. Archiving and Compression


### tar (Tape ARchive) Command

tar is the standard Unix archiving tool. It bundles files into a single archive file.

#### Basic tar Operations:

```bash
# Create archive (no compression)
tar -cf archive.tar file1 file2 directory/

# Create compressed archive with gzip
tar -czf archive.tar.gz file1 file2 directory/
tar -czf archive.tgz file1 file2 directory/     # .tgz is same as .tar.gz

# Create compressed archive with bzip2 (better compression)
tar -cjf archive.tar.bz2 file1 file2 directory/

# Create compressed archive with xz (best compression)
tar -cJf archive.tar.xz file1 file2 directory/

# List archive contents
tar -tf archive.tar
tar -tzf archive.tar.gz

# Extract archive
tar -xf archive.tar
tar -xzf archive.tar.gz

# Extract to specific directory
tar -xzf archive.tar.gz -C /path/to/destination/

# Extract specific files
tar -xzf archive.tar.gz file1.txt directory/file2.txt
```

#### tar Options Explained:
- `-c` - Create archive
- `-x` - Extract archive
- `-t` - List contents
- `-f` - Specify filename
- `-z` - Use gzip compression
- `-j` - Use bzip2 compression
- `-J` - Use xz compression
- `-v` - Verbose output
- `-p` - Preserve permissions

#### Practical tar Examples:

```bash
# Backup home directory
tar -czf home_backup_$(date +%Y%m%d).tar.gz ~

# Backup with exclusions
tar -czf backup.tar.gz --exclude='*.tmp' --exclude='*.log' /home/user/

# Extract and view progress
tar -xzf large_archive.tar.gz -v

# Create archive with verification
tar -czf archive.tar.gz files/ && tar -tzf archive.tar.gz

# Split large archives
tar -czf - large_directory/ | split -b 1G - archive.tar.gz.part_
```

### Compression Tools

#### gzip/gunzip:
```bash
# Compress file (replaces original)
gzip file.txt                    # Creates file.txt.gz

# Decompress file
gunzip file.txt.gz               # Restores file.txt

# Keep original file
gzip -k file.txt                 # Keep original
gunzip -k file.txt.gz            # Keep compressed

# Compress multiple files
gzip *.txt

# View compressed file content
zcat file.txt.gz
zless file.txt.gz
zgrep pattern file.txt.gz
```

#### bzip2/bunzip2:
```bash
# Compress (better compression than gzip)
bzip2 file.txt                   # Creates file.txt.bz2

# Decompress
bunzip2 file.txt.bz2             # Restores file.txt

# Keep original
bzip2 -k file.txt

# View compressed content
bzcat file.txt.bz2
bzless file.txt.bz2
bzgrep pattern file.txt.bz2
```

#### zip/unzip:
```bash
# Create zip archive
zip archive.zip file1 file2 directory/

# Create zip with compression level
zip -9 archive.zip file1 file2   # Maximum compression

# Add to existing zip
zip archive.zip newfile.txt

# Extract zip
unzip archive.zip

# Extract to directory
unzip archive.zip -d destination/

# List zip contents
unzip -l archive.zip

# Test zip integrity
unzip -t archive.zip
```
---

## Navigation

**Previous:** [← Working With Blocks The Dd Command](02-working-with-blocks-the-dd-command.md)  
**Next:** [→ Understanding Links](04-understanding-links.md)  
**Lesson Home:** [↑ Lesson 06: Globbing Archiving Links](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
