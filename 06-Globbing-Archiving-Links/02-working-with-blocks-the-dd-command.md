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

### Common Block Devices and Partitions:

#### Listing Block Devices:

```bash
# List all block devices
lsblk                    # Tree view of all block devices

# Show detailed information
lsblk -f                 # Include filesystem type and UUID
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT   # Custom columns

# Alternative methods
fdisk -l                 # List all disks and partitions (requires root)
parted -l                # List partitions with parted (requires root)
ls -l /dev/sd*           # List SATA/SCSI devices
ls -l /dev/nvme*         # List NVMe devices
```

#### SATA/SCSI Drives (Traditional):

```bash
# Whole disks
/dev/sda                 # First SATA/SCSI drive
/dev/sdb                 # Second SATA/SCSI drive
/dev/sdc                 # Third SATA/SCSI drive

# Partitions
/dev/sda1               # First partition on first drive
/dev/sda2               # Second partition on first drive
/dev/sdb1               # First partition on second drive

# Example: Backup entire SATA drive
dd if=/dev/sda of=sda_backup.img bs=4M status=progress

# Example: Clone one drive to another
dd if=/dev/sda of=/dev/sdb bs=4M status=progress
```

#### NVMe Drives (Modern SSDs):

```bash
# Whole NVMe drives
/dev/nvme0n1            # First NVMe drive
/dev/nvme1n1            # Second NVMe drive
/dev/nvme2n1            # Third NVMe drive

# NVMe partitions
/dev/nvme0n1p1          # First partition on first NVMe drive
/dev/nvme0n1p2          # Second partition on first NVMe drive
/dev/nvme1n1p1          # First partition on second NVMe drive

# Example: Backup NVMe drive
dd if=/dev/nvme0n1 of=nvme_backup.img bs=4M status=progress

# Example: Backup NVMe partition
dd if=/dev/nvme0n1p1 of=nvme_partition1.img bs=4M status=progress
```

#### Modern Removable Media:

```bash
# SD cards and eMMC
/dev/mmcblk0            # First SD card/eMMC device
/dev/mmcblk0p1          # First partition on SD card
/dev/mmcblk1            # Second SD card

# USB drives (usually appear as sd*)
/dev/sdd                # USB drive (fourth SCSI device)
/dev/sdd1               # First partition on USB drive

# Example: Create bootable USB from ISO
dd if=ubuntu.iso of=/dev/sdd bs=4M status=progress && sync

# Example: Backup SD card
dd if=/dev/mmcblk0 of=sdcard_backup.img bs=4M status=progress
```

#### Virtual and Loop Devices:

```bash
# Loop devices (for mounting image files)
/dev/loop0              # First loop device
/dev/loop1              # Second loop device

# Virtual disks (in VMs)
/dev/vda                # First virtio disk
/dev/vda1               # First partition on virtio disk
/dev/xvda               # Xen virtual disk

# Example: Create disk image and mount
dd if=/dev/zero of=disk.img bs=1M count=100
losetup /dev/loop0 disk.img
```

#### ⚠️ Safety Tips:
- **Always double-check the device name** before running dd
- Use `lsblk` to verify the correct device
- `if=` is input (source), `of=` is output (destination)
- Wrong device can destroy data permanently
- Consider using `status=progress` to monitor operations
- Always run `sync` after dd operations

---

## Navigation

**Next:** [→ Archiving And Compression](03-archiving-and-compression.md)  
**Previous:** [← File Globbing Wildcard Patterns](01-file-globbing-wildcard-patterns.md)  
**Lesson Home:** [↑ Lesson 6: Globbing & Archiving](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
