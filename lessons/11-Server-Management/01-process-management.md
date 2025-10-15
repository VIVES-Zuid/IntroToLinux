# 1. Process Management


### Understanding Processes
Every running program in Linux is a process. Processes have:
- **PID (Process ID)**: Unique identifier
- **PPID (Parent Process ID)**: Parent process that started it
- **UID/GID**: User and group running the process
- **Memory usage**: RAM and virtual memory consumption
- **CPU usage**: Processing time consumption
- **State**: Running, sleeping, stopped, or zombie

### Viewing Processes:

```bash
# Show all processes
ps aux
ps -ef

# Show process tree
ps aux --forest
pstree

# Interactive process monitor
top
htop                 # Enhanced version (install with apt)

# Show processes for specific user
ps -u username
ps aux | grep username

# Show processes by name
pgrep firefox
pgrep -l ssh         # Show name and PID
```

### Process States:
- **R**: Running or runnable
- **S**: Sleeping (interruptible)
- **D**: Sleeping (uninterruptible, usually I/O)
- **T**: Stopped (by signal or debugger)
- **Z**: Zombie (finished but not reaped by parent)

### Managing Processes:

```bash
# Send signals to processes
kill PID                    # Send TERM signal (graceful shutdown)
kill -9 PID                 # Send KILL signal (force kill)
kill -STOP PID              # Pause process
kill -CONT PID              # Resume process

# Kill by name
killall firefox
pkill firefox

# Kill all processes by user
sudo pkill -u username

# Background and foreground jobs
command &                   # Run in background
jobs                       # List background jobs
fg %1                      # Bring job 1 to foreground
bg %1                      # Send job 1 to background
nohup command &            # Run immune to hangups
```

### Process Priorities:

```bash
# Run with different priority (nice values: -20 to 19)
nice -n 10 command         # Lower priority
nice -n -5 command         # Higher priority (requires privileges)

# Change running process priority
renice 5 PID               # Set nice value to 5
renice -5 -u username      # Set for all user's processes
```

### System Resource Monitoring:

```bash
# CPU and memory usage
top
htop
vmstat 1               # Virtual memory statistics every second
iostat 1               # I/O statistics
sar 1 10               # System activity report

# Memory usage
free -h                # Human readable
cat /proc/meminfo      # Detailed memory information

# Disk usage
df -h                  # Filesystem usage
du -sh directory/      # Directory size
du -h --max-depth=1    # Size of subdirectories

# System load
uptime                 # Load average and uptime
w                      # Who's logged in and what they're doing
```
---

## Navigation

**Previous:** [← Learning Objectives](00-learning-objectives.md)  
**Next:** [→ System Services And Systemd](02-system-services-and-systemd.md)  
**Lesson Home:** [↑ Lesson 11: Server Management](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
