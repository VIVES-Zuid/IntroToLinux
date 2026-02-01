# 9. Script Examples and Best Practices


### Example: System Backup Script

```bash
#!/bin/bash
#
# Script: backup-system.sh
# Purpose: Create timestamped backups of important directories
# Author: System Administrator
# Version: 1.0
#

# Configuration
SOURCE_DIRS=("/etc" "/home" "/var/log")
BACKUP_BASE="/backups"
DATE=$(date +%Y%m%d_%H%M%S)
LOG_FILE="/var/log/backup.log"

# Functions
log_message() {
    echo "$(date '+%Y-%m-%d %H:%M:%S') - $1" | tee -a "$LOG_FILE"
}

create_backup() {
    local source_dir="$1"
    local backup_name=$(basename "$source_dir")
    local backup_path="$BACKUP_BASE/${backup_name}_$DATE.tar.gz"
    
    log_message "Starting backup of $source_dir"
    
    if tar -czf "$backup_path" "$source_dir" 2>/dev/null; then
        log_message "Backup completed: $backup_path"
        return 0
    else
        log_message "ERROR: Backup failed for $source_dir"
        return 1
    fi
}

# Main script
log_message "Backup script started"

# Check if backup directory exists
if [ ! -d "$BACKUP_BASE" ]; then
    log_message "Creating backup directory: $BACKUP_BASE"
    mkdir -p "$BACKUP_BASE"
fi

# Perform backups
backup_count=0
failed_count=0

for dir in "${SOURCE_DIRS[@]}"; do
    if [ -d "$dir" ]; then
        if create_backup "$dir"; then
            backup_count=$((backup_count + 1))
        else
            failed_count=$((failed_count + 1))
        fi
    else
        log_message "WARNING: Directory $dir does not exist"
    fi
done

log_message "Backup script completed. Success: $backup_count, Failed: $failed_count"
```

### Example: File Organizer Script

```bash
#!/bin/bash
#
# Script: organize-files.sh
# Purpose: Organize files by extension into subdirectories
#

# Configuration
SOURCE_DIR="${1:-.}"  # Use first argument or current directory
ORGANIZE_TYPES=("txt:documents" "jpg:images" "png:images" "pdf:documents" "mp3:audio")

# Function to create directory if it doesn't exist
create_dir_if_needed() {
    local dir="$1"
    if [ ! -d "$dir" ]; then
        echo "Creating directory: $dir"
        mkdir -p "$dir"
    fi
}

echo "Organizing files in: $SOURCE_DIR"

# Process each file type
for type_mapping in "${ORGANIZE_TYPES[@]}"; do
    extension="${type_mapping%%:*}"  # Everything before :
    category="${type_mapping##*:}"   # Everything after :
    
    # Find files with this extension
    files=$(find "$SOURCE_DIR" -maxdepth 1 -name "*.$extension" -type f 2>/dev/null)
    
    if [ -n "$files" ]; then
        create_dir_if_needed "$SOURCE_DIR/$category"
        
        echo "Moving .$extension files to $category directory:"
        while IFS= read -r file; do
            if [ -f "$file" ]; then
                echo "  Moving: $(basename "$file")"
                mv "$file" "$SOURCE_DIR/$category/"
            fi
        done <<< "$files"
    fi
done

echo "File organization completed!"
```
---

## Navigation

**Next:** [→ Practical Labs](10-practical-labs.md)  
**Previous:** [← Case Statements](08-case-statements.md)  
**Lesson Home:** [↑ Lesson 8: Scripting](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
