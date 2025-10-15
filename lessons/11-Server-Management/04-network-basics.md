# 4. Network Basics


### Network Information:

```bash
# Show network interfaces
ip addr show
ip a                   # Short form
ifconfig              # Traditional command (may need installation)

# Show routing table
ip route show
route -n              # Traditional command

# Show network connections
netstat -tuln         # TCP/UDP listening ports
ss -tuln              # Modern replacement for netstat
ss -tup               # Show process names

# DNS lookup
nslookup google.com
dig google.com
host google.com

# Test connectivity
ping google.com
ping -c 4 google.com   # Send 4 packets
traceroute google.com  # Show network path
```

### Network Configuration:

```bash
# Temporary IP configuration
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip route add default via 192.168.1.1

# Bring interface up/down
sudo ip link set eth0 up
sudo ip link set eth0 down

# Check network manager status
systemctl status NetworkManager
nmcli device status    # NetworkManager command line
```

### Network Services:

```bash
# SSH service
sudo systemctl status ssh
sudo systemctl start ssh

# Check SSH connections
who
w
last

# Basic firewall (ufw)
sudo ufw status
sudo ufw enable
sudo ufw allow ssh
sudo ufw allow 80/tcp
sudo ufw deny 23/tcp
```
---

## Navigation

**Previous:** [← Job Scheduling With Cron](03-job-scheduling-with-cron.md)  
**Next:** [→ System Maintenance And Monitoring](05-system-maintenance-and-monitoring.md)  
**Lesson Home:** [↑ Lesson 11: Server Management](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
