# 2. System Services and Systemd


### Understanding systemd
systemd is the init system used by most modern Linux distributions. It manages:
- System services (daemons)
- Mount points
- Device management
- Login sessions
- System state

### Service Management:

```bash
# Check service status
systemctl status ssh
systemctl status nginx
systemctl status --all        # All services

# Start/stop services
sudo systemctl start ssh
sudo systemctl stop ssh
sudo systemctl restart ssh
sudo systemctl reload ssh     # Reload configuration

# Enable/disable services (start at boot)
sudo systemctl enable ssh
sudo systemctl disable ssh
sudo systemctl enable --now ssh   # Enable and start

# List services
systemctl list-units --type=service
systemctl list-units --state=failed
systemctl list-unit-files --type=service
```

### Creating Custom Services:

```bash
# Create a simple service file
sudo nano /etc/systemd/system/myapp.service

# Service file content:
[Unit]
Description=My Application
After=network.target

[Service]
Type=simple
User=myuser
WorkingDirectory=/opt/myapp
ExecStart=/opt/myapp/start.sh
ExecReload=/bin/kill -HUP $MAINPID
Restart=always

[Install]
WantedBy=multi-user.target

# Enable the service
sudo systemctl daemon-reload
sudo systemctl enable myapp
sudo systemctl start myapp
```

### System Targets (Runlevels):

```bash
# Check current target
systemctl get-default

# Change target
sudo systemctl set-default multi-user.target   # Console only
sudo systemctl set-default graphical.target    # GUI

# Switch targets temporarily
sudo systemctl isolate rescue.target           # Single user mode
sudo systemctl isolate multi-user.target       # Multi-user mode
```
---

## Navigation

**Previous:** [← Process Management](01-process-management.md)  
**Next:** [→ Job Scheduling With Cron](03-job-scheduling-with-cron.md)  
**Lesson Home:** [↑ Lesson 11: Server Management](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
