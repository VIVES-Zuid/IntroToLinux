# 9. Final Project Labs


### Lab 11.1: System Administration Dashboard (30 minutes)

Create a comprehensive system monitoring script:

```bash
#!/bin/bash
# System Administration Dashboard

clear
echo "=================================="
echo "    LINUX ADMINISTRATION DASHBOARD"
echo "=================================="
echo

# System Information
echo "=== SYSTEM INFORMATION ==="
echo "Hostname: $(hostname)"
echo "OS: $(cat /etc/os-release | grep PRETTY_NAME | cut -d'"' -f2)"
echo "Kernel: $(uname -r)"
echo "Uptime: $(uptime -p)"
echo

# Resource Usage
echo "=== RESOURCE USAGE ==="
echo "CPU Usage:"
top -bn1 | grep "Cpu(s)" | awk '{print $2}' | sed 's/%us,//'
echo
echo "Memory Usage:"
free -h | grep '^Mem:' | awk '{printf "Used: %s/%s (%.1f%%)\n", $3, $2, ($3/$2)*100}'
echo
echo "Disk Usage:"
df -h / | tail -1 | awk '{printf "Used: %s/%s (%s)\n", $3, $2, $5}'
echo

# Network Information
echo "=== NETWORK ==="
echo "IP Addresses:"
ip addr show | grep "inet " | awk '{print $2}' | head -3
echo
echo "Active Connections:"
ss -tuln | wc -l
echo

# User Activity
echo "=== USER ACTIVITY ==="
echo "Logged in users: $(who | wc -l)"
who
echo

# Service Status
echo "=== SERVICE STATUS ==="
services=("ssh" "nginx" "cron")
for service in "${services[@]}"; do
    if systemctl is-active --quiet "$service"; then
        echo "$service: ✓ Running"
    else
        echo "$service: ✗ Stopped"
    fi
done
echo

# Recent System Events
echo "=== RECENT SYSTEM EVENTS ==="
echo "Last 5 logins:"
last -n 5
echo

echo "Dashboard updated: $(date)"
```

### Lab 11.2: Automated System Maintenance (25 minutes)

Create a comprehensive maintenance script:

```bash
#!/bin/bash
# Automated System Maintenance Script

LOG_FILE="/var/log/maintenance.log"
BACKUP_DIR="/maintenance_backups"

log() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" | tee -a "$LOG_FILE"
}

# System update
log "Starting system maintenance"
apt update && apt upgrade -y
log "System updates completed"

# Cleanup temporary files
log "Cleaning temporary files"
find /tmp -type f -atime +7 -delete
find /var/tmp -type f -atime +7 -delete
log "Temporary file cleanup completed"

# Log rotation
log "Rotating logs"
logrotate -f /etc/logrotate.conf
log "Log rotation completed"

# Security scan
log "Running security scan"
find /home -perm -002 -type f > /tmp/world_writable_files.txt
if [ -s /tmp/world_writable_files.txt ]; then
    log "WARNING: World-writable files found"
    cat /tmp/world_writable_files.txt >> "$LOG_FILE"
fi

# Backup important configurations
log "Backing up configurations"
mkdir -p "$BACKUP_DIR"
tar -czf "$BACKUP_DIR/config_backup_$(date +%Y%m%d).tar.gz" /etc
log "Configuration backup completed"

# System health check
log "Performing system health check"
df -h | grep -E '(9[0-9]%|100%)' && log "WARNING: High disk usage detected"
free | awk 'NR==2{printf "%.2f%%\n", $3*100/$2 }' | grep -E '(9[0-9]|100)' && log "WARNING: High memory usage"

log "Maintenance completed"
```

### Lab 11.3: User Environment Setup (20 minutes)

Create a script to set up a complete user environment:

```bash
#!/bin/bash
# User Environment Setup Script

setup_user_environment() {
    local username="$1"
    local fullname="$2"
    
    if [ -z "$username" ]; then
        echo "Usage: setup_user_environment <username> [full_name]"
        return 1
    fi
    
    # Create user
    if ! id "$username" &>/dev/null; then
        useradd -m -s /bin/bash -c "$fullname" "$username"
        echo "User $username created"
    fi
    
    # Set up home directory structure
    sudo -u "$username" bash << EOF
cd /home/$username

# Create useful directories
mkdir -p {bin,scripts,projects,documents,downloads,tmp}

# Create .bashrc with useful aliases
cat > .bashrc << 'BASHRC'
# User-specific aliases and functions
alias ll='ls -la'
alias la='ls -la'
alias l='ls -CF'
alias grep='grep --color=auto'
alias ..='cd ..'
alias ...='cd ../..'

# Environment variables
export EDITOR=nano
export HISTSIZE=10000
export HISTFILESIZE=20000

# Custom prompt
PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

# Add personal bin to PATH
if [ -d "\$HOME/bin" ]; then
    PATH="\$HOME/bin:\$PATH"
fi
BASHRC

# Create useful scripts
cat > bin/backup-home.sh << 'BACKUP'
#!/bin/bash
# Personal backup script
BACKUP_DIR="$HOME/backups"
mkdir -p "$BACKUP_DIR"
tar -czf "$BACKUP_DIR/home_backup_$(date +%Y%m%d_%H%M%S).tar.gz" \
    --exclude="$BACKUP_DIR" \
    --exclude=".cache" \
    --exclude="tmp" \
    "$HOME"
echo "Backup completed: $BACKUP_DIR/home_backup_$(date +%Y%m%d_%H%M%S).tar.gz"
BACKUP

chmod +x bin/backup-home.sh

# Create a sample project
mkdir -p projects/sample_project/{src,docs,tests}
echo "# Sample Project" > projects/sample_project/README.md
echo "print('Hello, World!')" > projects/sample_project/src/hello.py

# Create .vimrc for vim users
cat > .vimrc << 'VIMRC'
syntax on
set number
set tabstop=4
set shiftwidth=4
set expandtab
VIMRC

echo "User environment setup completed for $username"
EOF
}

# Main execution
if [ $# -eq 0 ]; then
    echo "Usage: $0 <username> [full_name]"
    echo "Example: $0 john 'John Doe'"
    exit 1
fi

setup_user_environment "$1" "$2"
```
---

## Navigation

**Previous:** [← Advanced System Integration Project](08-advanced-system-integration-project.md)  
**Next:** [→ Course Review And Next Steps](10-course-review-and-next-steps.md)  
**Lesson Home:** [↑ Lesson 11: Server Management](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
