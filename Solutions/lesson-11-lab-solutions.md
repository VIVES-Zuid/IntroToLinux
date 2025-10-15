# Solutions for Lesson 11: System Administration and Advanced Topics

## Lab 11.1: System Administration Dashboard (30 minutes)

### Solution:

```bash
# Create system administration dashboard script
cat > system_dashboard.sh << 'EOF'
#!/bin/bash
# System Administration Dashboard

# Color codes for better readability
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m' # No Color

# Function to print colored output
print_header() {
    echo -e "${BLUE}$1${NC}"
}

print_success() {
    echo -e "${GREEN}$1${NC}"
}

print_warning() {
    echo -e "${YELLOW}$1${NC}"
}

print_error() {
    echo -e "${RED}$1${NC}"
}

# Function to get memory usage percentage
get_memory_usage() {
    if command -v free >/dev/null 2>&1; then
        free | grep '^Mem:' | awk '{printf "%.1f", $3/$2 * 100}'
    else
        echo "N/A"
    fi
}

# Function to get disk usage
get_disk_usage() {
    df -h / 2>/dev/null | tail -1 | awk '{print $5}' | sed 's/%//' || echo "N/A"
}

# Function to get CPU usage
get_cpu_usage() {
    if [ -f /proc/stat ]; then
        grep 'cpu ' /proc/stat | awk '{usage=($2+$4)*100/($2+$3+$4+$5)} END {printf "%.1f", usage}'
    else
        top -bn1 | grep "Cpu(s)" | awk '{print $2}' | sed 's/%us,//' || echo "N/A"
    fi
}

# Function to get load average
get_load_average() {
    if [ -f /proc/loadavg ]; then
        cat /proc/loadavg | cut -d' ' -f1-3
    else
        uptime | awk -F'load average:' '{print $2}' | sed 's/^[[:space:]]*//'
    fi
}

# Function to check service status
check_service() {
    local service="$1"
    if command -v systemctl >/dev/null 2>&1; then
        if systemctl is-active --quiet "$service" 2>/dev/null; then
            print_success "$service: ✓ Running"
        else
            print_error "$service: ✗ Stopped"
        fi
    else
        # Fallback for systems without systemd
        if pgrep "$service" >/dev/null 2>&1; then
            print_success "$service: ✓ Running"
        else
            print_error "$service: ✗ Stopped"
        fi
    fi
}

# Main dashboard function
main() {
    clear
    echo "=================================="
    echo "    LINUX ADMINISTRATION DASHBOARD"
    echo "=================================="
    echo
    
    # System Information
    print_header "=== SYSTEM INFORMATION ==="
    echo "Hostname: $(hostname)"
    
    # OS Information
    if [ -f /etc/os-release ]; then
        os_name=$(grep PRETTY_NAME /etc/os-release | cut -d'"' -f2)
        echo "OS: $os_name"
    elif [ -f /etc/redhat-release ]; then
        echo "OS: $(cat /etc/redhat-release)"
    else
        echo "OS: $(uname -s)"
    fi
    
    echo "Kernel: $(uname -r)"
    echo "Architecture: $(uname -m)"
    
    # Uptime
    if command -v uptime >/dev/null 2>&1; then
        uptime_info=$(uptime -p 2>/dev/null || uptime | awk -F'up ' '{print $2}' | awk -F',' '{print $1}')
        echo "Uptime: $uptime_info"
    fi
    echo
    
    # Resource Usage
    print_header "=== RESOURCE USAGE ==="
    
    # CPU Usage
    cpu_usage=$(get_cpu_usage)
    echo "CPU Usage: ${cpu_usage}%"
    
    # Memory Usage
    if command -v free >/dev/null 2>&1; then
        memory_info=$(free -h | grep '^Mem:')
        used=$(echo $memory_info | awk '{print $3}')
        total=$(echo $memory_info | awk '{print $2}')
        mem_percent=$(get_memory_usage)
        echo "Memory Usage: $used/$total (${mem_percent}%)"
        
        # Memory warning
        if (( $(echo "$mem_percent > 80" | bc -l 2>/dev/null || echo 0) )); then
            print_warning "⚠  High memory usage detected!"
        fi
    fi
    
    # Disk Usage
    echo "Disk Usage:"
    df -h | grep -E '^/dev/' | head -5 | while read line; do
        filesystem=$(echo $line | awk '{print $1}')
        used=$(echo $line | awk '{print $3}')
        total=$(echo $line | awk '{print $2}')
        percent=$(echo $line | awk '{print $5}' | sed 's/%//')
        mountpoint=$(echo $line | awk '{print $6}')
        
        if [ "$percent" -gt 80 ]; then
            print_warning "  $mountpoint: $used/$total ($percent%) - High usage!"
        else
            echo "  $mountpoint: $used/$total ($percent%)"
        fi
    done
    
    # Load Average
    load_avg=$(get_load_average)
    echo "Load Average: $load_avg"
    echo
    
    # Network Information
    print_header "=== NETWORK ==="
    echo "IP Addresses:"
    
    # Get IP addresses
    if command -v ip >/dev/null 2>&1; then
        ip addr show | grep "inet " | grep -v 127.0.0.1 | awk '{print $2}' | cut -d'/' -f1 | head -3 | while read ip; do
            echo "  $ip"
        done
    elif command -v ifconfig >/dev/null 2>&1; then
        ifconfig | grep "inet " | grep -v 127.0.0.1 | awk '{print $2}' | head -3 | while read ip; do
            echo "  $ip"
        done
    fi
    
    # Network connections
    if command -v ss >/dev/null 2>&1; then
        active_connections=$(ss -tuln | wc -l)
        echo "Active Network Connections: $active_connections"
    elif command -v netstat >/dev/null 2>&1; then
        active_connections=$(netstat -tuln | wc -l)
        echo "Active Network Connections: $active_connections"
    fi
    echo
    
    # User Activity
    print_header "=== USER ACTIVITY ==="
    logged_users=$(who | wc -l)
    echo "Logged in users: $logged_users"
    
    if [ "$logged_users" -gt 0 ]; then
        echo "Current sessions:"
        who | while read line; do
            echo "  $line"
        done
    fi
    echo
    
    # Service Status
    print_header "=== SERVICE STATUS ==="
    services=("ssh" "sshd" "nginx" "apache2" "httpd" "mysql" "mariadb" "postgresql" "cron" "crond")
    
    for service in "${services[@]}"; do
        # Only check services that might exist
        if command -v systemctl >/dev/null 2>&1; then
            if systemctl list-unit-files | grep -q "^$service"; then
                check_service "$service"
            fi
        else
            # Fallback check
            if which "$service" >/dev/null 2>&1 || [ -f "/etc/init.d/$service" ]; then
                check_service "$service"
            fi
        fi
    done
    echo
    
    # Recent System Events
    print_header "=== RECENT SYSTEM EVENTS ==="
    
    echo "Last 5 logins:"
    if command -v last >/dev/null 2>&1; then
        last -n 5 | head -5
    else
        echo "  last command not available"
    fi
    echo
    
    # System logs (if accessible)
    if [ -r /var/log/syslog ]; then
        echo "Recent system messages:"
        tail -3 /var/log/syslog | while read line; do
            echo "  $line"
        done
    elif [ -r /var/log/messages ]; then
        echo "Recent system messages:"
        tail -3 /var/log/messages | while read line; do
            echo "  $line"
        done
    fi
    echo
    
    # Security Summary
    print_header "=== SECURITY SUMMARY ==="
    
    # Failed login attempts
    if [ -r /var/log/auth.log ]; then
        failed_logins=$(grep "Failed password" /var/log/auth.log | wc -l 2>/dev/null || echo 0)
        if [ "$failed_logins" -gt 0 ]; then
            print_warning "Failed login attempts: $failed_logins"
        else
            print_success "No recent failed login attempts"
        fi
    fi
    
    # Check for updates (Ubuntu/Debian)
    if command -v apt >/dev/null 2>&1; then
        updates=$(apt list --upgradable 2>/dev/null | wc -l)
        if [ "$updates" -gt 1 ]; then
            print_warning "Available updates: $((updates - 1))"
        else
            print_success "System is up to date"
        fi
    fi
    
    echo
    echo "Dashboard updated: $(date)"
    echo "Refresh with: $0"
}

# Check for required commands
check_dependencies() {
    missing_commands=()
    
    for cmd in awk grep sed cut; do
        if ! command -v "$cmd" >/dev/null 2>&1; then
            missing_commands+=("$cmd")
        fi
    done
    
    if [ ${#missing_commands[@]} -gt 0 ]; then
        echo "Warning: Missing commands: ${missing_commands[*]}"
        echo "Some features may not work correctly."
        echo
    fi
}

# Parse command line options
while getopts "hc" opt; do
    case $opt in
        h)
            echo "Usage: $0 [-h] [-c]"
            echo "  -h  Show this help"
            echo "  -c  Continuous monitoring (updates every 5 seconds)"
            exit 0
            ;;
        c)
            echo "Starting continuous monitoring (Ctrl+C to stop)..."
            while true; do
                main
                sleep 5
            done
            ;;
        \?)
            echo "Invalid option: -$OPTARG" >&2
            exit 1
            ;;
    esac
done

# Run the dashboard
check_dependencies
main
EOF

chmod +x system_dashboard.sh

# Test the dashboard
echo "Testing system dashboard..."
./system_dashboard.sh

# Show continuous monitoring option
echo
echo "For continuous monitoring, run: ./system_dashboard.sh -c"
```

## Lab 11.2: Automated System Maintenance (25 minutes)

### Solution:

```bash
# Create automated system maintenance script
cat > system_maintenance.sh << 'EOF'
#!/bin/bash
# Automated System Maintenance Script

# Configuration
LOG_FILE="/var/log/maintenance.log"
BACKUP_DIR="/opt/maintenance_backups"
MAX_LOG_SIZE="100M"
RETENTION_DAYS=30
DEBUG=false

# Color codes
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m'

# Logging function
log() {
    local level="$1"
    shift
    local message="$*"
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')
    
    # Color code based on level
    case "$level" in
        "ERROR")   color="$RED" ;;
        "WARNING") color="$YELLOW" ;;
        "SUCCESS") color="$GREEN" ;;
        "INFO")    color="$BLUE" ;;
        *)         color="$NC" ;;
    esac
    
    # Log to file and console
    echo "[$timestamp] [$level] $message" | sudo tee -a "$LOG_FILE"
    echo -e "${color}[$level]${NC} $message"
}

# Debug logging
debug() {
    if [ "$DEBUG" = true ]; then
        log "DEBUG" "$*"
    fi
}

# Error handling
handle_error() {
    local exit_code=$?
    local line_number=$1
    log "ERROR" "Script failed at line $line_number with exit code $exit_code"
    exit $exit_code
}

# Trap errors
trap 'handle_error $LINENO' ERR

# Function to check if running as root
check_root() {
    if [ "$EUID" -ne 0 ]; then
        log "ERROR" "This script must be run as root (use sudo)"
        exit 1
    fi
}

# Function to create backup directory
create_backup_dir() {
    if [ ! -d "$BACKUP_DIR" ]; then
        mkdir -p "$BACKUP_DIR"
        log "INFO" "Created backup directory: $BACKUP_DIR"
    fi
}

# Function to update system packages
update_system() {
    log "INFO" "Starting system updates..."
    
    if command -v apt >/dev/null 2>&1; then
        # Ubuntu/Debian
        apt update >/dev/null 2>&1
        local updates=$(apt list --upgradable 2>/dev/null | wc -l)
        
        if [ "$updates" -gt 1 ]; then
            log "INFO" "$((updates - 1)) packages available for update"
            apt upgrade -y >/dev/null 2>&1
            log "SUCCESS" "System packages updated successfully"
        else
            log "INFO" "No package updates available"
        fi
        
        # Clean package cache
        apt autoremove -y >/dev/null 2>&1
        apt autoclean >/dev/null 2>&1
        
    elif command -v yum >/dev/null 2>&1; then
        # RHEL/CentOS
        yum update -y >/dev/null 2>&1
        yum clean all >/dev/null 2>&1
        log "SUCCESS" "System updated (YUM)"
        
    elif command -v dnf >/dev/null 2>&1; then
        # Fedora
        dnf update -y >/dev/null 2>&1
        dnf clean all >/dev/null 2>&1
        log "SUCCESS" "System updated (DNF)"
        
    else
        log "WARNING" "No supported package manager found"
    fi
}

# Function to clean temporary files
cleanup_temp_files() {
    log "INFO" "Cleaning temporary files..."
    
    local cleaned=0
    
    # Clean /tmp files older than 7 days
    find /tmp -type f -atime +7 -delete 2>/dev/null && cleaned=$((cleaned + 1))
    find /var/tmp -type f -atime +7 -delete 2>/dev/null && cleaned=$((cleaned + 1))
    
    # Clean user cache directories
    find /home -name ".cache" -type d -exec find {} -type f -atime +30 -delete \; 2>/dev/null
    
    # Clean old log files in /var/log
    find /var/log -name "*.log.*" -type f -mtime +$RETENTION_DAYS -delete 2>/dev/null
    
    # Clean package manager cache
    if command -v apt >/dev/null 2>&1; then
        apt clean >/dev/null 2>&1
    fi
    
    log "SUCCESS" "Temporary file cleanup completed"
}

# Function to rotate logs
rotate_logs() {
    log "INFO" "Rotating logs..."
    
    # Check if logrotate exists
    if command -v logrotate >/dev/null 2>&1; then
        if [ -f /etc/logrotate.conf ]; then
            logrotate -f /etc/logrotate.conf >/dev/null 2>&1
            log "SUCCESS" "Log rotation completed using logrotate"
        else
            log "WARNING" "logrotate.conf not found"
        fi
    else
        # Manual log rotation for common logs
        for logfile in /var/log/syslog /var/log/messages /var/log/auth.log; do
            if [ -f "$logfile" ] && [ $(stat -c%s "$logfile" 2>/dev/null || echo 0) -gt 10485760 ]; then
                # Rotate if larger than 10MB
                cp "$logfile" "${logfile}.old"
                > "$logfile"
                log "INFO" "Manually rotated $logfile"
            fi
        done
    fi
}

# Function to run security scan
security_scan() {
    log "INFO" "Running security scan..."
    
    local security_issues=0
    
    # Check for world-writable files
    local writable_files=$(find /home -type f -perm -002 2>/dev/null | wc -l)
    if [ "$writable_files" -gt 0 ]; then
        log "WARNING" "Found $writable_files world-writable files in /home"
        find /home -type f -perm -002 2>/dev/null | head -10 | while read file; do
            log "WARNING" "World-writable: $file"
        done
        security_issues=$((security_issues + 1))
    fi
    
    # Check for SUID files in unusual locations
    local suid_files=$(find /home /tmp -type f -perm -4000 2>/dev/null | wc -l)
    if [ "$suid_files" -gt 0 ]; then
        log "WARNING" "Found $suid_files SUID files in unusual locations"
        security_issues=$((security_issues + 1))
    fi
    
    # Check for failed login attempts
    if [ -f /var/log/auth.log ]; then
        local failed_logins=$(grep "Failed password" /var/log/auth.log | tail -100 | wc -l)
        if [ "$failed_logins" -gt 10 ]; then
            log "WARNING" "High number of failed login attempts: $failed_logins"
            security_issues=$((security_issues + 1))
        fi
    fi
    
    if [ "$security_issues" -eq 0 ]; then
        log "SUCCESS" "No security issues detected"
    else
        log "WARNING" "Security scan completed with $security_issues issue(s)"
    fi
}

# Function to backup important configurations
backup_configurations() {
    log "INFO" "Backing up configurations..."
    
    local backup_name="config_backup_$(date +%Y%m%d_%H%M%S).tar.gz"
    local backup_path="$BACKUP_DIR/$backup_name"
    
    # Create list of important directories to backup
    local backup_dirs=("/etc" "/var/spool/cron" "/root/.ssh")
    local existing_dirs=()
    
    # Check which directories exist
    for dir in "${backup_dirs[@]}"; do
        if [ -d "$dir" ]; then
            existing_dirs+=("$dir")
        fi
    done
    
    if [ ${#existing_dirs[@]} -gt 0 ]; then
        tar -czf "$backup_path" "${existing_dirs[@]}" 2>/dev/null
        
        if [ $? -eq 0 ]; then
            log "SUCCESS" "Configuration backup created: $backup_name"
            
            # Remove old backups
            find "$BACKUP_DIR" -name "config_backup_*.tar.gz" -mtime +$RETENTION_DAYS -delete 2>/dev/null
            log "INFO" "Old backups cleaned up"
        else
            log "ERROR" "Failed to create configuration backup"
        fi
    else
        log "WARNING" "No configuration directories found to backup"
    fi
}

# Function to perform system health check
system_health_check() {
    log "INFO" "Performing system health check..."
    
    local health_issues=0
    
    # Check disk usage
    df -h | grep -E '^/dev/' | while read line; do
        local usage=$(echo $line | awk '{print $5}' | sed 's/%//')
        local mount=$(echo $line | awk '{print $6}')
        
        if [ "$usage" -gt 90 ]; then
            log "WARNING" "High disk usage: $mount ($usage%)"
            health_issues=$((health_issues + 1))
        elif [ "$usage" -gt 80 ]; then
            log "INFO" "Moderate disk usage: $mount ($usage%)"
        fi
    done
    
    # Check memory usage
    if command -v free >/dev/null 2>&1; then
        local mem_usage=$(free | grep '^Mem:' | awk '{printf "%.0f", $3/$2 * 100}')
        if [ "$mem_usage" -gt 90 ]; then
            log "WARNING" "High memory usage: $mem_usage%"
            health_issues=$((health_issues + 1))
        fi
    fi
    
    # Check load average
    local load=$(uptime | awk -F'load average:' '{print $2}' | awk -F',' '{print $1}' | sed 's/^[[:space:]]*//')
    local cpu_cores=$(nproc 2>/dev/null || echo 1)
    
    if (( $(echo "$load > $cpu_cores" | bc -l 2>/dev/null || echo 0) )); then
        log "WARNING" "High system load: $load (cores: $cpu_cores)"
        health_issues=$((health_issues + 1))
    fi
    
    # Check for zombie processes
    local zombies=$(ps aux | awk '$8 ~ /^Z/ { count++ } END { print count+0 }')
    if [ "$zombies" -gt 0 ]; then
        log "WARNING" "Found $zombies zombie processes"
        health_issues=$((health_issues + 1))
    fi
    
    if [ "$health_issues" -eq 0 ]; then
        log "SUCCESS" "System health check passed"
    else
        log "WARNING" "System health check found $health_issues issue(s)"
    fi
}

# Function to generate maintenance report
generate_report() {
    local report_file="$BACKUP_DIR/maintenance_report_$(date +%Y%m%d_%H%M%S).txt"
    
    cat > "$report_file" << EOF
System Maintenance Report
========================
Date: $(date)
Hostname: $(hostname)
User: $(whoami)
Script: $0

System Information:
- OS: $(cat /etc/os-release 2>/dev/null | grep PRETTY_NAME | cut -d'"' -f2 || uname -s)
- Kernel: $(uname -r)
- Uptime: $(uptime -p 2>/dev/null || uptime)

Maintenance Tasks Completed:
- System package updates
- Temporary file cleanup
- Log rotation
- Security scan
- Configuration backup
- System health check

Current System Status:
- Disk Usage: $(df -h / | tail -1 | awk '{print $5}')
- Memory Usage: $(free | grep '^Mem:' | awk '{printf "%.1f%%", $3/$2 * 100}' 2>/dev/null || echo "N/A")
- Load Average: $(uptime | awk -F'load average:' '{print $2}')

For detailed logs, see: $LOG_FILE
EOF

    log "INFO" "Maintenance report generated: $report_file"
}

# Main maintenance function
main() {
    log "INFO" "Starting automated system maintenance"
    
    # Check prerequisites
    check_root
    create_backup_dir
    
    # Perform maintenance tasks
    update_system
    cleanup_temp_files
    rotate_logs
    security_scan
    backup_configurations
    system_health_check
    
    # Generate report
    generate_report
    
    log "SUCCESS" "Automated maintenance completed successfully"
}

# Parse command line options
while getopts "hdst:" opt; do
    case $opt in
        h)
            echo "Usage: $0 [-h] [-d] [-s] [-t task]"
            echo "  -h        Show this help"
            echo "  -d        Enable debug mode"
            echo "  -s        Run security scan only"
            echo "  -t task   Run specific task only"
            echo "            Tasks: update, cleanup, rotate, security, backup, health"
            exit 0
            ;;
        d)
            DEBUG=true
            log "INFO" "Debug mode enabled"
            ;;
        s)
            check_root
            security_scan
            exit 0
            ;;
        t)
            check_root
            case "$OPTARG" in
                update)   update_system ;;
                cleanup)  cleanup_temp_files ;;
                rotate)   rotate_logs ;;
                security) security_scan ;;
                backup)   create_backup_dir; backup_configurations ;;
                health)   system_health_check ;;
                *)        echo "Invalid task: $OPTARG"; exit 1 ;;
            esac
            exit 0
            ;;
        \?)
            echo "Invalid option: -$OPTARG" >&2
            exit 1
            ;;
    esac
done

# Run main maintenance
main
EOF

chmod +x system_maintenance.sh

# Create a test version that doesn't require sudo
cat > system_maintenance_test.sh << 'EOF'
#!/bin/bash
# Test version of system maintenance script (no sudo required)

LOG_FILE="./maintenance_test.log"
BACKUP_DIR="./maintenance_backups"

log() {
    local level="$1"
    shift
    local message="$*"
    local timestamp=$(date '+%Y-%m-%d %H:%M:%S')
    echo "[$timestamp] [$level] $message" | tee -a "$LOG_FILE"
}

# Create test backup directory
mkdir -p "$BACKUP_DIR"

log "INFO" "Starting test maintenance run"

# Simulate system update
log "INFO" "Simulating system update check..."
log "SUCCESS" "No updates required (test mode)"

# Simulate cleanup
log "INFO" "Simulating temporary file cleanup..."
find /tmp -type f -name "test_*" -mtime +1 2>/dev/null | head -5 | while read file; do
    log "INFO" "Would clean: $file"
done
log "SUCCESS" "Cleanup simulation completed"

# Test backup creation
log "INFO" "Creating test configuration backup..."
backup_name="test_backup_$(date +%Y%m%d_%H%M%S).tar.gz"
tar -czf "$BACKUP_DIR/$backup_name" /etc/hostname /etc/hosts 2>/dev/null
log "SUCCESS" "Test backup created: $backup_name"

# Test health check
log "INFO" "Running test health check..."
disk_usage=$(df -h / | tail -1 | awk '{print $5}' | sed 's/%//')
if [ "$disk_usage" -gt 80 ]; then
    log "WARNING" "Disk usage is high: $disk_usage%"
else
    log "SUCCESS" "Disk usage is acceptable: $disk_usage%"
fi

log "SUCCESS" "Test maintenance completed"
echo "Check $LOG_FILE for detailed log"
EOF

chmod +x system_maintenance_test.sh

echo "Testing maintenance script (safe version)..."
./system_maintenance_test.sh
```

## Lab 11.3: User Environment Setup (20 minutes)

### Solution:

```bash
# Create comprehensive user environment setup script
cat > user_environment_setup.sh << 'EOF'
#!/bin/bash
# User Environment Setup Script

# Configuration
DEFAULT_SHELL="/bin/bash"
SKELETON_DIR="/etc/skel"

# Color codes
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
BLUE='\033[0;34m'
NC='\033[0m'

# Logging function
log() {
    local level="$1"
    shift
    local message="$*"
    
    case "$level" in
        "ERROR")   color="$RED" ;;
        "SUCCESS") color="$GREEN" ;;
        "WARNING") color="$YELLOW" ;;
        "INFO")    color="$BLUE" ;;
        *)         color="$NC" ;;
    esac
    
    echo -e "${color}[$level]${NC} $message"
}

# Function to setup user environment
setup_user_environment() {
    local username="$1"
    local fullname="$2"
    local department="$3"
    
    if [ -z "$username" ]; then
        log "ERROR" "Username is required"
        return 1
    fi
    
    log "INFO" "Setting up environment for user: $username"
    
    # Create user if doesn't exist
    if ! id "$username" &>/dev/null; then
        log "INFO" "Creating user account..."
        
        if [ -n "$fullname" ]; then
            sudo useradd -m -s "$DEFAULT_SHELL" -c "$fullname" "$username"
        else
            sudo useradd -m -s "$DEFAULT_SHELL" "$username"
        fi
        
        if [ $? -eq 0 ]; then
            log "SUCCESS" "User $username created successfully"
        else
            log "ERROR" "Failed to create user $username"
            return 1
        fi
    else
        log "INFO" "User $username already exists, updating environment"
    fi
    
    # Set up home directory structure and configuration
    sudo -u "$username" bash << USEREOF
cd /home/$username

# Create useful directories
log() {
    echo "[INFO] \$*"
}

log "Creating directory structure..."
mkdir -p {bin,scripts,projects,documents,downloads,tmp,backup}
mkdir -p projects/{personal,work,learning}
mkdir -p documents/{templates,archive}

# Create .bashrc with useful aliases and functions
log "Setting up .bashrc..."
cat > .bashrc << 'BASHRC'
# ~/.bashrc: executed by bash(1) for non-login shells.

# If not running interactively, don't do anything
case $- in
    *i*) ;;
      *) return;;
esac

# History settings
HISTCONTROL=ignoreboth
HISTSIZE=10000
HISTFILESIZE=20000
shopt -s histappend

# Check window size after each command
shopt -s checkwinsize

# Enable programmable completion features
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi

# Color support for ls and grep
if [ -x /usr/bin/dircolors ]; then
    test -r ~/.dircolors && eval "$(dircolors -b ~/.dircolors)" || eval "$(dircolors -b)"
    alias ls='ls --color=auto'
    alias grep='grep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias egrep='egrep --color=auto'
fi

# Common aliases
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
alias h='history'
alias c='clear'
alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'
alias ~='cd ~'
alias grep='grep --color=auto'
alias tree='tree -C'

# Safety aliases
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

# Utility aliases
alias df='df -h'
alias du='du -h'
alias free='free -h'
alias ps='ps aux'
alias psg='ps aux | grep'
alias ports='netstat -tuln'

# Git aliases (if git is available)
if command -v git >/dev/null 2>&1; then
    alias gs='git status'
    alias ga='git add'
    alias gc='git commit'
    alias gp='git push'
    alias gl='git pull'
    alias gd='git diff'
    alias gb='git branch'
    alias gco='git checkout'
fi

# Environment variables
export EDITOR=nano
export PAGER=less
export BROWSER=firefox

# Custom prompt with colors
if [ "\$EUID" -eq 0 ]; then
    # Root prompt (red)
    PS1='\[\033[01;31m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]# '
else
    # Regular user prompt (green)
    PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
fi

# Add personal bin to PATH
if [ -d "\$HOME/bin" ]; then
    PATH="\$HOME/bin:\$PATH"
fi

# Load local customizations if they exist
if [ -f ~/.bashrc_local ]; then
    source ~/.bashrc_local
fi

# Welcome message
echo "Welcome to \$(hostname), \$USER!"
echo "Today is \$(date +'%A, %B %d, %Y')"
if command -v fortune >/dev/null 2>&1; then
    echo
    fortune
fi
BASHRC

# Create .bash_profile
log "Setting up .bash_profile..."
cat > .bash_profile << 'PROFILE'
# ~/.bash_profile: executed by bash(1) for login shells.

# Source .bashrc if it exists
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi

# User-specific environment variables
export PATH="\$HOME/bin:\$PATH"
PROFILE

# Create useful scripts in bin directory
log "Creating utility scripts..."

# Backup script
cat > bin/backup-home.sh << 'BACKUP'
#!/bin/bash
# Personal backup script

BACKUP_DIR="\$HOME/backup"
DATE=\$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="\$BACKUP_DIR/home_backup_\$DATE.tar.gz"

echo "Creating backup of home directory..."

# Create backup directory if it doesn't exist
mkdir -p "\$BACKUP_DIR"

# Create backup excluding certain directories
tar -czf "\$BACKUP_FILE" \
    --exclude="\$BACKUP_DIR" \
    --exclude=".cache" \
    --exclude=".tmp" \
    --exclude="tmp" \
    --exclude="*.log" \
    "\$HOME" 2>/dev/null

if [ \$? -eq 0 ]; then
    echo "Backup completed successfully: \$BACKUP_FILE"
    ls -lh "\$BACKUP_FILE"
    
    # Remove backups older than 30 days
    find "\$BACKUP_DIR" -name "home_backup_*.tar.gz" -mtime +30 -delete
    echo "Old backups cleaned up"
else
    echo "Backup failed!"
    exit 1
fi
BACKUP

# System info script
cat > bin/sysinfo.sh << 'SYSINFO'
#!/bin/bash
# Quick system information script

echo "=== System Information ==="
echo "Hostname: \$(hostname)"
echo "Date: \$(date)"
echo "Uptime: \$(uptime -p 2>/dev/null || uptime)"
echo "Kernel: \$(uname -r)"
echo "Shell: \$SHELL"
echo
echo "=== Resource Usage ==="
echo "CPU Load: \$(uptime | awk -F'load average:' '{print \$2}')"
if command -v free >/dev/null 2>&1; then
    echo "Memory: \$(free -h | grep '^Mem:' | awk '{print \$3 "/" \$2}')"
fi
echo "Disk Usage: \$(df -h / | tail -1 | awk '{print \$5}')"
echo
echo "=== Network ==="
if command -v ip >/dev/null 2>&1; then
    ip addr show | grep "inet " | grep -v 127.0.0.1 | awk '{print "IP: " \$2}' | head -3
fi
SYSINFO

# Project initialization script
cat > bin/new-project.sh << 'PROJECT'
#!/bin/bash
# Create new project structure

if [ -z "\$1" ]; then
    echo "Usage: \$0 <project-name>"
    exit 1
fi

PROJECT_NAME="\$1"
PROJECT_DIR="\$HOME/projects/\$PROJECT_NAME"

if [ -d "\$PROJECT_DIR" ]; then
    echo "Project \$PROJECT_NAME already exists!"
    exit 1
fi

echo "Creating project: \$PROJECT_NAME"

mkdir -p "\$PROJECT_DIR"/{src,docs,tests,config,scripts}

# Create basic files
echo "# \$PROJECT_NAME" > "\$PROJECT_DIR/README.md"
echo "A description of the \$PROJECT_NAME project." >> "\$PROJECT_DIR/README.md"

echo "# Changelog for \$PROJECT_NAME" > "\$PROJECT_DIR/CHANGELOG.md"
echo "" >> "\$PROJECT_DIR/CHANGELOG.md"
echo "## [\$(date +%Y-%m-%d)] - Initial version" >> "\$PROJECT_DIR/CHANGELOG.md"
echo "- Project created" >> "\$PROJECT_DIR/CHANGELOG.md"

# Create .gitignore if git is available
if command -v git >/dev/null 2>&1; then
    cat > "\$PROJECT_DIR/.gitignore" << GITIGNORE
# Temporary files
*.tmp
*.temp
*.log

# OS generated files
.DS_Store
Thumbs.db

# Editor files
*.swp
*.swo
*~

# Build artifacts
build/
dist/
*.o
*.so
GITIGNORE

    cd "\$PROJECT_DIR"
    git init
    git add .
    git commit -m "Initial project setup"
fi

echo "Project \$PROJECT_NAME created in \$PROJECT_DIR"
echo "Structure:"
tree "\$PROJECT_DIR" 2>/dev/null || find "\$PROJECT_DIR" -type d
PROJECT

# Make scripts executable
chmod +x bin/*.sh

# Create configuration files
log "Setting up configuration files..."

# Vim configuration
cat > .vimrc << 'VIMRC'
" Basic vim configuration
syntax on
set number
set tabstop=4
set shiftwidth=4
set expandtab
set autoindent
set showmatch
set hlsearch
set incsearch
set ignorecase
set smartcase
set ruler
set wildmenu
set scrolloff=3
colorscheme default
VIMRC

# Git configuration template
if command -v git >/dev/null 2>&1; then
    log "Setting up git configuration template..."
    cat > .gitconfig_template << 'GITCONFIG'
[user]
    name = Your Name
    email = your.email@example.com

[core]
    editor = nano
    autocrlf = input

[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    df = diff
    lg = log --oneline --graph --all

[push]
    default = simple

[pull]
    rebase = false
GITCONFIG
fi

# SSH directory setup
log "Setting up SSH directory..."
mkdir -p .ssh
chmod 700 .ssh
touch .ssh/authorized_keys
chmod 600 .ssh/authorized_keys

# Create sample project
log "Creating sample project..."
mkdir -p projects/sample_project/{src,docs,tests}
echo "# Sample Project" > projects/sample_project/README.md
echo "This is a sample project to demonstrate the directory structure." >> projects/sample_project/README.md

cat > projects/sample_project/src/hello.py << 'PYTHON'
#!/usr/bin/env python3
"""
Sample Python script
"""

def main():
    print("Hello, World!")
    print("Welcome to your new development environment!")

if __name__ == "__main__":
    main()
PYTHON

chmod +x projects/sample_project/src/hello.py

# Create templates directory with useful templates
log "Creating document templates..."
mkdir -p documents/templates

cat > documents/templates/meeting_notes.md << 'MEETING'
# Meeting Notes

**Date:** [DATE]
**Time:** [TIME]
**Attendees:** [LIST ATTENDEES]

## Agenda
- [ ] Item 1
- [ ] Item 2
- [ ] Item 3

## Discussion Points

### Topic 1
[Notes]

### Topic 2
[Notes]

## Action Items
- [ ] [Action] - [Assigned to] - [Due date]

## Next Meeting
**Date:** [DATE]
**Time:** [TIME]
MEETING

cat > documents/templates/project_plan.md << 'PLAN'
# Project Plan: [PROJECT NAME]

## Overview
[Brief description of the project]

## Objectives
- Objective 1
- Objective 2
- Objective 3

## Timeline
- **Phase 1:** [Description] - [Dates]
- **Phase 2:** [Description] - [Dates]
- **Phase 3:** [Description] - [Dates]

## Resources Required
- Resource 1
- Resource 2
- Resource 3

## Risks and Mitigation
| Risk | Impact | Probability | Mitigation |
|------|--------|-------------|------------|
| Risk 1 | High/Medium/Low | High/Medium/Low | Strategy |

## Success Criteria
- Criteria 1
- Criteria 2
- Criteria 3
PLAN

echo "User environment setup completed for $username"
echo "Directory structure:"
tree /home/$username 2>/dev/null || find /home/$username -type d | sort
USEREOF

    log "SUCCESS" "Environment setup completed for $username"
    
    # Set appropriate permissions
    sudo chown -R "$username:$username" "/home/$username"
    
    # Add user to useful groups
    if [ -n "$department" ]; then
        case "$department" in
            "IT"|"Development")
                sudo usermod -aG sudo,adm "$username" 2>/dev/null
                log "INFO" "Added $username to IT groups"
                ;;
            "Marketing"|"Sales")
                sudo usermod -aG users "$username" 2>/dev/null
                log "INFO" "Added $username to business groups"
                ;;
        esac
    fi
    
    # Generate temporary password
    temp_password=$(openssl rand -base64 12 2>/dev/null || date +%s | sha256sum | base64 | head -c 12)
    echo "$username:$temp_password" | sudo chpasswd
    sudo chage -d 0 "$username"  # Force password change on first login
    
    log "SUCCESS" "User $username setup completed"
    log "INFO" "Temporary password: $temp_password"
    log "WARNING" "User must change password on first login"
    
    return 0
}

# Function to setup multiple users from a file
setup_multiple_users() {
    local user_file="$1"
    
    if [ ! -f "$user_file" ]; then
        log "ERROR" "User file not found: $user_file"
        return 1
    fi
    
    log "INFO" "Setting up users from file: $user_file"
    
    while IFS=',' read -r username fullname department email; do
        # Skip comments and empty lines
        [[ "$username" =~ ^#.*$ ]] && continue
        [[ -z "$username" ]] && continue
        
        log "INFO" "Processing user: $username"
        setup_user_environment "$username" "$fullname" "$department"
        
        if [ -n "$email" ]; then
            log "INFO" "Email for $username: $email"
        fi
        
    done < "$user_file"
}

# Function to create sample user file
create_sample_user_file() {
    cat > users_sample.csv << 'CSV'
# Format: username,fullname,department,email
# Lines starting with # are ignored
alice,Alice Johnson,IT,alice@company.com
bob,Bob Smith,Development,bob@company.com
charlie,Charlie Brown,Marketing,charlie@company.com
diana,Diana Prince,Sales,diana@company.com
CSV
    
    log "SUCCESS" "Sample user file created: users_sample.csv"
}

# Main execution
main() {
    case "$1" in
        "single")
            if [ -z "$2" ]; then
                log "ERROR" "Username required for single user setup"
                echo "Usage: $0 single <username> [fullname] [department]"
                exit 1
            fi
            setup_user_environment "$2" "$3" "$4"
            ;;
        "multiple")
            if [ -z "$2" ]; then
                log "ERROR" "User file required for multiple user setup"
                echo "Usage: $0 multiple <user_file>"
                exit 1
            fi
            setup_multiple_users "$2"
            ;;
        "sample")
            create_sample_user_file
            ;;
        *)
            echo "Usage: $0 {single|multiple|sample}"
            echo ""
            echo "Commands:"
            echo "  single <username> [fullname] [department]"
            echo "    Set up environment for a single user"
            echo ""
            echo "  multiple <user_file>"
            echo "    Set up environments for multiple users from CSV file"
            echo ""
            echo "  sample"
            echo "    Create a sample user CSV file"
            echo ""
            echo "Examples:"
            echo "  $0 single john \"John Doe\" \"IT\""
            echo "  $0 multiple users.csv"
            echo "  $0 sample"
            exit 1
            ;;
    esac
}

# Check if running as root
if [ "$EUID" -ne 0 ]; then
    log "ERROR" "This script must be run as root (use sudo)"
    exit 1
fi

# Run main function
main "$@"
EOF

chmod +x user_environment_setup.sh

# Create a test version for demonstration
echo "Creating sample user setup..."
./user_environment_setup.sh sample

echo "Sample user file created. To test:"
echo "sudo ./user_environment_setup.sh single testuser \"Test User\" \"IT\""

# Test with a simulated user setup (without actually creating the user)
cat > test_environment_setup.sh << 'EOF'
#!/bin/bash
# Test version of user environment setup

echo "=== User Environment Setup Test ==="
echo "This script demonstrates the user environment setup process"
echo

# Simulate directory creation
echo "Creating directory structure for test user..."
mkdir -p test_user_home/{bin,scripts,projects,documents,downloads,tmp,backup}
mkdir -p test_user_home/projects/{personal,work,learning}

# Create sample .bashrc
cat > test_user_home/.bashrc << 'BASHRC'
# Sample .bashrc configuration
export PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
alias ll='ls -alF'
alias la='ls -A'
alias l='ls -CF'
export PATH="$HOME/bin:$PATH"
BASHRC

# Create sample scripts
cat > test_user_home/bin/backup-home.sh << 'BACKUP'
#!/bin/bash
echo "This would create a backup of the home directory"
BACKUP

chmod +x test_user_home/bin/backup-home.sh

echo "Test environment created in: test_user_home/"
echo "Directory structure:"
tree test_user_home/ 2>/dev/null || find test_user_home/ -type d | sort

echo
echo "Sample files created:"
ls -la test_user_home/
ls -la test_user_home/bin/

echo
echo "Test completed successfully!"
EOF

chmod +x test_environment_setup.sh
./test_environment_setup.sh
```

## Verification and Integration:

```bash
# Create integration test script
cat > integration_test.sh << 'EOF'
#!/bin/bash
# Integration test for all lab solutions

echo "=== System Administration Labs Integration Test ==="

# Test 1: Dashboard functionality
echo "1. Testing system dashboard..."
if [ -x system_dashboard.sh ]; then
    echo "✓ Dashboard script is executable"
    timeout 5s ./system_dashboard.sh >/dev/null 2>&1 && echo "✓ Dashboard runs successfully"
else
    echo "✗ Dashboard script not found or not executable"
fi

# Test 2: Maintenance script functionality
echo
echo "2. Testing maintenance script..."
if [ -x system_maintenance_test.sh ]; then
    echo "✓ Maintenance test script is executable"
    ./system_maintenance_test.sh >/dev/null 2>&1 && echo "✓ Maintenance script runs successfully"
else
    echo "✗ Maintenance test script not found"
fi

# Test 3: User environment setup
echo
echo "3. Testing user environment setup..."
if [ -x test_environment_setup.sh ]; then
    echo "✓ User environment test script is executable"
    ./test_environment_setup.sh >/dev/null 2>&1 && echo "✓ User environment setup runs successfully"
else
    echo "✗ User environment test script not found"
fi

# Test 4: Check created files and directories
echo
echo "4. Checking created files..."
files_to_check=(
    "system_dashboard.sh"
    "system_maintenance.sh" 
    "system_maintenance_test.sh"
    "user_environment_setup.sh"
    "test_environment_setup.sh"
    "users_sample.csv"
)

for file in "${files_to_check[@]}"; do
    if [ -f "$file" ]; then
        echo "✓ $file exists"
    else
        echo "✗ $file missing"
    fi
done

# Test 5: Check log files
echo
echo "5. Checking log files..."
if [ -f maintenance_test.log ]; then
    echo "✓ Maintenance log created"
    echo "  Log entries: $(wc -l < maintenance_test.log)"
else
    echo "✗ No maintenance log found"
fi

# Test 6: Check backup directories
echo
echo "6. Checking backup directories..."
if [ -d maintenance_backups ]; then
    echo "✓ Maintenance backup directory exists"
    echo "  Backup files: $(ls maintenance_backups/ | wc -l)"
else
    echo "✗ No maintenance backup directory found"
fi

if [ -d test_user_home ]; then
    echo "✓ Test user home directory exists"
    echo "  Directory structure: $(find test_user_home -type d | wc -l) directories"
else
    echo "✗ No test user home directory found"
fi

echo
echo "=== Integration Test Summary ==="
echo "All system administration lab components have been tested."
echo "Check individual outputs above for detailed results."
EOF

chmod +x integration_test.sh
./integration_test.sh

# Clean up function
cat > cleanup_labs.sh << 'EOF'
#!/bin/bash
# Cleanup script for all lab files

echo "Cleaning up lab files..."

# Remove test files and directories
rm -rf test_user_home/
rm -rf maintenance_backups/
rm -f maintenance_test.log
rm -f users_sample.csv

# Remove scripts (optional - comment out to keep)
# rm -f system_dashboard.sh
# rm -f system_maintenance.sh
# rm -f system_maintenance_test.sh
# rm -f user_environment_setup.sh
# rm -f test_environment_setup.sh
# rm -f integration_test.sh

echo "Cleanup completed."
echo "Main scripts preserved: system_dashboard.sh, system_maintenance.sh, user_environment_setup.sh"
EOF

chmod +x cleanup_labs.sh

echo
echo "=== Lab 11 Completed Successfully ==="
echo
echo "Created scripts:"
echo "1. system_dashboard.sh - System monitoring dashboard"
echo "2. system_maintenance.sh - Automated maintenance (requires sudo)"
echo "3. system_maintenance_test.sh - Safe test version"
echo "4. user_environment_setup.sh - User environment setup (requires sudo)"
echo "5. test_environment_setup.sh - Safe test version"
echo "6. integration_test.sh - Test all components"
echo "7. cleanup_labs.sh - Clean up test files"
echo
echo "To run cleanup: ./cleanup_labs.sh"
```

## Key Learning Points:

1. **System Monitoring**: Automated dashboard for real-time system status
2. **Maintenance Automation**: Scheduled tasks for system upkeep
3. **User Environment**: Standardized setup for new users
4. **Error Handling**: Robust scripts with proper logging and error management
5. **Integration**: Combining multiple system administration tasks
6. **Best Practices**: Secure scripting, logging, and documentation

---

## Navigation

**Previous:** [← Lesson 10: File Permissions and Tools](lesson-10-lab-solutions.md)  
**Home:** [↑ Solutions Index](README.md)