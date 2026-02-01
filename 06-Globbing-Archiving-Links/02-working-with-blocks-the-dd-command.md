# 2. Working with Blocks: The dd Command


The `dd` command is a powerful tool for copying and converting blocks of data. It's often called "disk destroyer" due to its power - use carefully!

### Basic dd Syntax:
```bash
dd if=input_file of=output_file [options]
```

### Common dd Operations:

#### Create Files of Specific Size:
```bash
# Create 1MB file filled with zeros
dd if=/dev/zero of=test.img bs=1M count=1

# Create 100MB file
dd if=/dev/zero of=large.file bs=1M count=100

# Create file with random data
dd if=/dev/urandom of=random.dat bs=1K count=10
```

#### Backup and Restore:
```bash
# Backup entire disk (DANGEROUS - be very careful!)
dd if=/dev/sda of=disk_backup.img bs=4M

# Backup partition
dd if=/dev/sda1 of=partition_backup.img bs=4M

# Restore from backup
dd if=disk_backup.img of=/dev/sda bs=4M

# Backup with progress (using pv if available)
dd if=/dev/sda bs=4M | pv | dd of=backup.img bs=4M
```

#### Create Bootable USB:
```bash
# Create bootable USB from ISO (DANGEROUS - verify device!)
dd if=linux.iso of=/dev/sdX bs=4M status=progress

# Always sync after dd operations
sync
```

#### Performance Testing:
```bash
# Test write speed
dd if=/dev/zero of=test.img bs=1M count=1000 oflag=direct

# Test read speed  
dd if=test.img of=/dev/null bs=1M iflag=direct
```

### dd Options:
- `bs=SIZE` - Block size (e.g., 1K, 1M, 1G)
- `count=N` - Number of blocks to copy
- `skip=N` - Skip N blocks at start of input
- `seek=N` - Skip N blocks at start of output
- `status=progress` - Show progress
- `conv=noerror` - Continue on errors
---

## Navigation

**Next:** [→ Archiving And Compression](03-archiving-and-compression.md)  
**Previous:** [← File Globbing Wildcard Patterns](01-file-globbing-wildcard-patterns.md)  
**Lesson Home:** [↑ Lesson 6: Globbing & Archiving](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
