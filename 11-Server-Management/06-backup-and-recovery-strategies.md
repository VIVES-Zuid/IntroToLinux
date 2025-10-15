# 6. Backup and Recovery Strategies


### File-based Backups:

```bash
# Simple tar backup
tar -czf backup-$(date +%Y%m%d).tar.gz /home /etc

# Incremental backup with rsync
rsync -av --delete /home/ /backup/home/
rsync -av /home/ user@backup-server:/backups/home/

# Backup with exclusions
tar --exclude='*.tmp' --exclude='*.log' -czf backup.tar.gz /home

# Automated backup script
#!/bin/bash
BACKUP_DIR="/backups"
DATE=$(date +%Y%m%d_%H%M%S)
SOURCE_DIRS="/home /etc /opt"

for dir in $SOURCE_DIRS; do
    if [ -d "$dir" ]; then
        tar -czf "$BACKUP_DIR/$(basename $dir)_$DATE.tar.gz" "$dir"
        echo "Backed up $dir"
    fi
done

# Cleanup old backups (keep last 7 days)
find "$BACKUP_DIR" -name "*.tar.gz" -mtime +7 -delete
```

### Database Backups:

```bash
# MySQL backup
mysqldump -u root -p database_name > backup.sql
mysqldump -u root -p --all-databases > all_databases.sql

# PostgreSQL backup
pg_dump database_name > backup.sql
pg_dumpall > all_databases.sql

# Restore database
mysql -u root -p database_name < backup.sql
psql database_name < backup.sql
```

### System Image Backups:

```bash
# Create disk image
sudo dd if=/dev/sda of=/backup/disk_image.img bs=4M status=progress

# Create compressed image
sudo dd if=/dev/sda bs=4M | gzip > /backup/disk_image.img.gz

# Restore from image
sudo gunzip -c /backup/disk_image.img.gz | dd of=/dev/sda bs=4M
```
---

## Navigation

**Previous:** [← System Maintenance And Monitoring](05-system-maintenance-and-monitoring.md)  
**Next:** [→ Performance Tuning And Troubleshooting](07-performance-tuning-and-troubleshooting.md)  
**Lesson Home:** [↑ Lesson 11: Server Management](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
