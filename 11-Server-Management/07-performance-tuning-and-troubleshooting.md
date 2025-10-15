# 7. Performance Tuning and Troubleshooting


### System Performance Analysis:

```bash
# CPU performance
top
htop
vmstat 1 10           # CPU, memory, I/O stats
sar -u 1 10           # CPU utilization

# Memory analysis
free -h
cat /proc/meminfo
vmstat -m             # Memory slabs

# Disk I/O analysis
iostat -x 1 10        # Extended I/O statistics
iotop                 # I/O usage by process

# Network performance
iftop                 # Network usage by connection
nethogs               # Network usage by process
```

### Common Troubleshooting:

```bash
# High CPU usage
top -o %CPU           # Sort by CPU usage
ps aux --sort=-%cpu | head -10

# High memory usage
ps aux --sort=-%mem | head -10
smem -r               # Shared memory analysis

# Disk space issues
df -h                 # Check filesystem usage
du -sh /* | sort -hr  # Find large directories
lsof | grep deleted   # Find deleted but open files

# Process troubleshooting
lsof -p PID           # Files opened by process
strace -p PID         # System calls made by process
```
---

## Navigation

**Previous:** [← Backup And Recovery Strategies](06-backup-and-recovery-strategies.md)  
**Next:** [→ Advanced System Integration Project](08-advanced-system-integration-project.md)  
**Lesson Home:** [↑ Lesson 11: Server Management](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
