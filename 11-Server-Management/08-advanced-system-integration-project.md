# 8. Advanced System Integration Project


### Project: Complete System Setup

Let's create a comprehensive script that demonstrates integration of all concepts learned:

```bash
#!/bin/bash
#
# Script: system-setup.sh
# Purpose: Complete Linux system setup and monitoring
# Integrates: Users, permissions, services, monitoring, backups
#

# Colors for output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

# Functions
log_info() {
    echo -e "${GREEN}[INFO]${NC} $1"
}

log_warn() {
    echo -e "${YELLOW}[WARN]${NC} $1"
}

log_error() {
    echo -e "${RED}[ERROR]${NC} $1"
}

# Check if running as root
check_root() {
    if [ "$EUID" -ne 0 ]; then
        log_error "This script must be run as root"
        exit 1
    fi
}

# Create project users and groups
setup_users() {
    log_info "Setting up users and groups..."
    
    # Create groups
    groupadd -f developers
    groupadd -f webadmin
    
    # Create users
    for user in alice bob charlie; do
        if ! id "$user" &>/dev/null; then
            useradd -m -s /bin/bash -G developers "$user"
            echo "$user:password123" | chpasswd
            log_info "Created user: $user"
        fi
    done
    
    # Create web admin user
    if ! id "webadmin" &>/dev/null; then
        useradd -m -s /bin/bash -G webadmin webadmin
        echo "webadmin:webpass123" | chpasswd
        usermod -aG sudo webadmin
        log_info "Created webadmin user"
    fi
}

# Setup shared project directory
setup_project_directory() {
    log_info "Setting up project directory..."
    
    PROJECT_DIR="/opt/shared_project"
    mkdir -p "$PROJECT_DIR"
    chgrp developers "$PROJECT_DIR"
    chmod 2775 "$PROJECT_DIR"
    
    # Create subdirectories
    mkdir -p "$PROJECT_DIR"/{src,docs,config,logs}
    chgrp -R developers "$PROJECT_DIR"
    chmod -R 2775 "$PROJECT_DIR"
    
    log_info "Project directory created: $PROJECT_DIR"
}

# Install and configure web server
setup_web_server() {
    log_info "Setting up web server..."
    
    # Install nginx
    apt update
    apt install -y nginx
    
    # Configure basic site
    cat > /var/www/html/index.html << 'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>Linux Course Server</title>
</head>
<body>
    <h1>Welcome to Linux Administration</h1>
    <p>This server demonstrates Linux concepts learned in class.</p>
    <p>Server time: <script>document.write(new Date());</script></p>
</body>
</html>
EOF

    # Set proper permissions
    chown -R www-data:www-data /var/www/html
    chmod -R 644 /var/www/html
    find /var/www/html -type d -exec chmod 755 {} \;
    
    # Enable and start nginx
    systemctl enable nginx
    systemctl start nginx
    
    log_info "Web server configured and started"
}

# Setup monitoring
setup_monitoring() {
    log_info "Setting up system monitoring..."
    
    # Create monitoring script
    cat > /usr/local/bin/system-monitor.sh << 'EOF'
#!/bin/bash
# System monitoring script

LOG_FILE="/var/log/system-monitor.log"
DATE=$(date '+%Y-%m-%d %H:%M:%S')

{
    echo "=== System Status - $DATE ==="
    echo "Uptime: $(uptime)"
    echo "Memory: $(free -h | grep '^Mem:' | awk '{print $3"/"$2}')"
    echo "Disk: $(df -h / | tail -1 | awk '{print $5}')"
    echo "Load: $(uptime | awk -F'load average:' '{print $2}')"
    echo "Active users: $(who | wc -l)"
    echo "Top processes:"
    ps aux --sort=-%cpu | head -5
    echo "==========================================="
} >> "$LOG_FILE"

# Alert if disk usage > 80%
DISK_USAGE=$(df / | tail -1 | awk '{print $5}' | sed 's/%//')
if [ "$DISK_USAGE" -gt 80 ]; then
    echo "WARNING: Disk usage is ${DISK_USAGE}%" | mail -s "Disk Alert" root
fi
EOF

    chmod +x /usr/local/bin/system-monitor.sh
    
    # Add to cron
    echo "*/5 * * * * /usr/local/bin/system-monitor.sh" | crontab -
    
    log_info "Monitoring setup complete"
}

# Setup backup system
setup_backup() {
    log_info "Setting up backup system..."
    
    BACKUP_DIR="/backups"
    mkdir -p "$BACKUP_DIR"
    
    # Create backup script
    cat > /usr/local/bin/daily-backup.sh << 'EOF'
#!/bin/bash
# Daily backup script

BACKUP_DIR="/backups"
DATE=$(date +%Y%m%d_%H%M%S)
LOG_FILE="/var/log/backup.log"

log_message() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" >> "$LOG_FILE"
}

# Backup important directories
BACKUP_DIRS="/etc /home /opt/shared_project"

for dir in $BACKUP_DIRS; do
    if [ -d "$dir" ]; then
        BACKUP_NAME="$(basename $dir)_$DATE.tar.gz"
        if tar -czf "$BACKUP_DIR/$BACKUP_NAME" "$dir" 2>/dev/null; then
            log_message "Backup successful: $BACKUP_NAME"
        else
            log_message "Backup failed: $dir"
        fi
    fi
done

# Cleanup old backups (keep 7 days)
find "$BACKUP_DIR" -name "*.tar.gz" -mtime +7 -delete
log_message "Backup cleanup completed"
EOF

    chmod +x /usr/local/bin/daily-backup.sh
    
    # Schedule daily backup at 2 AM
    echo "0 2 * * * /usr/local/bin/daily-backup.sh" >> /var/spool/cron/crontabs/root
    
    log_info "Backup system configured"
}

# Security hardening
apply_security() {
    log_info "Applying security measures..."
    
    # Update system
    apt update && apt upgrade -y
    
    # Install security tools
    apt install -y fail2ban ufw
    
    # Configure firewall
    ufw --force enable
    ufw allow ssh
    ufw allow http
    ufw allow https
    
    # Configure fail2ban
    systemctl enable fail2ban
    systemctl start fail2ban
    
    # Set secure permissions on important files
    chmod 644 /etc/passwd
    chmod 600 /etc/shadow
    chmod 644 /etc/group
    
    log_info "Security hardening completed"
}

# Generate system report
generate_report() {
    log_info "Generating system report..."
    
    REPORT_FILE="/tmp/system-setup-report.txt"
    
    {
        echo "=== Linux System Setup Report ==="
        echo "Generated: $(date)"
        echo
        echo "=== System Information ==="
        uname -a
        cat /etc/os-release | head -5
        echo
        echo "=== Users and Groups ==="
        echo "Regular users:"
        awk -F: '$3 >= 1000 {print $1}' /etc/passwd
        echo "Groups:"
        getent group developers webadmin
        echo
        echo "=== Services Status ==="
        systemctl is-active nginx ssh cron
        echo
        echo "=== Network Information ==="
        ip addr show | grep "inet " | head -3
        echo
        echo "=== Disk Usage ==="
        df -h | head -5
        echo
        echo "=== Memory Usage ==="
        free -h
        echo
        echo "=== Setup Complete ==="
    } > "$REPORT_FILE"
    
    log_info "Report generated: $REPORT_FILE"
    cat "$REPORT_FILE"
}

# Main execution
main() {
    log_info "Starting Linux system setup..."
    
    check_root
    setup_users
    setup_project_directory
    setup_web_server
    setup_monitoring
    setup_backup
    apply_security
    generate_report
    
    log_info "System setup completed successfully!"
}

# Run main function
main "$@"
```
---

## Navigation

**Next:** [→ Final Project Labs](09-final-project-labs.md)  
**Previous:** [← Performance Tuning And Troubleshooting](07-performance-tuning-and-troubleshooting.md)  
**Lesson Home:** [↑ Lesson 11: Server Management](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
