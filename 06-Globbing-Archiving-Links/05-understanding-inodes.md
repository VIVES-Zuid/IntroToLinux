# 5. Understanding Inodes


![Inodes](../images/inodes.png)

### What is an Inode?
An inode (index node) is a data structure that stores metadata about a file or directory.

### Inode Information:
- File type and permissions
- Owner and group
- File size
- Timestamps (access, modify, change)
- Link count
- Block locations on disk

### Working with Inodes:

```bash
# Show inode numbers
ls -i file.txt
ls -li directory/

# Detailed inode information
stat file.txt

# Find files by inode
find . -inum 123456

# Show inode usage
df -i                            # Inode usage per filesystem

# Check file system inode limits
tune2fs -l /dev/sda1 | grep -i inode
```

### Practical Inode Concepts:

```bash
# Hard links share inodes
echo "test content" > original.txt
ln original.txt hardlink.txt
ls -li original.txt hardlink.txt    # Same inode number

# Soft links have different inodes
ln -s original.txt softlink.txt
ls -li original.txt softlink.txt    # Different inode numbers

# Monitor link counts
stat original.txt                   # Shows link count
rm hardlink.txt
stat original.txt                   # Link count decreased
```
---

## Navigation

**Previous:** [← Understanding Links](04-understanding-links.md)  
**Next:** [→ Practical Labs](06-practical-labs.md)  
**Lesson Home:** [↑ Lesson 06: Globbing Archiving Links](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
