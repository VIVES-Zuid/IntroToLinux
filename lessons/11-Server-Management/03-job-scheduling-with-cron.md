# 3. Job Scheduling with cron


### Understanding cron
cron allows you to schedule commands to run automatically at specified times.

### Cron Time Format:
```
* * * * * command
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7, Sunday = 0 or 7)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
```

### Managing Cron Jobs:

```bash
# Edit user's crontab
crontab -e

# List current cron jobs
crontab -l

# Remove all cron jobs
crontab -r

# Edit another user's crontab (as root)
sudo crontab -e -u username
```

### Cron Examples:

```bash
# Run every minute
* * * * * /path/to/script.sh

# Run every day at 2:30 AM
30 2 * * * /path/to/backup.sh

# Run every Monday at 9:00 AM
0 9 * * 1 /path/to/weekly-report.sh

# Run every 15 minutes
*/15 * * * * /path/to/check-status.sh

# Run on weekdays at 6:00 PM
0 18 * * 1-5 /path/to/workday-end.sh

# Run first day of every month
0 0 1 * * /path/to/monthly-cleanup.sh

# Run at system startup
@reboot /path/to/startup-script.sh
```

### System-wide Cron:

```bash
# System cron directories
/etc/cron.d/           # Custom cron files
/etc/cron.daily/       # Daily scripts
/etc/cron.weekly/      # Weekly scripts
/etc/cron.monthly/     # Monthly scripts
/etc/cron.hourly/      # Hourly scripts

# Example: Create daily backup script
sudo nano /etc/cron.daily/backup
#!/bin/bash
tar -czf /backups/daily-$(date +%Y%m%d).tar.gz /home /etc

sudo chmod +x /etc/cron.daily/backup
```
---

## Navigation

**Previous:** [← System Services And Systemd](02-system-services-and-systemd.md)  
**Next:** [→ Network Basics](04-network-basics.md)  
**Lesson Home:** [↑ Lesson 11: Server Management](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
