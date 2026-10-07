# Summary: Lesson 11 – Server Management

**Concepts**
- Processes: PID, states, signals, priorities (nice values)
- systemd: services, units, targets (runlevels)
- Job scheduling: cron time format, user & system crontabs
- Networking basics: interfaces, IP, ports, firewall
- Logs & log rotation
- Package management & updates
- Backups (file, database, image) & recovery
- Performance monitoring & troubleshooting

**Commands**
- Processes: `ps`, `top`, `htop`, `kill`, `pkill`, `pgrep`, `nice`, `renice`, `jobs`, `fg`, `bg`
- Services: `systemctl` (`start`, `stop`, `restart`, `enable`, `status`), `journalctl`
- Scheduling: `crontab` (`-e`, `-l`)
- Network: `ip`, `ping`, `ss`/`netstat`, `ssh`, `ufw`
- Resources: `free`, `df`, `du`, `uptime`, `vmstat`, `iostat`, `lsof`, `uname`
- Packages: `apt`, `dnf`
- Backup: `tar`, `rsync`, `dd`, `mysqldump`, `logrotate`

---

## Navigation

**Previous:** [← Course Completion](13-course-completion.md)  
**Lesson Home:** [↑ Lesson 11: Server Management](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
