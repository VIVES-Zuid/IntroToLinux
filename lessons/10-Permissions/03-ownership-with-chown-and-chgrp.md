# 3. Ownership with chown and chgrp


### Changing File Ownership

#### chown Command:
```bash
# Change owner only
chown newowner file.txt

# Change owner and group
chown newowner:newgroup file.txt
chown newowner. file.txt          # Use owner's primary group

# Change recursively
chown -R newowner:newgroup directory/

# Change only if current owner matches
chown --from=oldowner newowner file.txt
```

#### chgrp Command:
```bash
# Change group only
chgrp newgroup file.txt

# Change recursively
chgrp -R newgroup directory/

# Use reference file's group
chgrp --reference=reference.txt target.txt
```

### Practical Ownership Examples:

```bash
# Web server files
sudo chown -R www-data:www-data /var/www/html/

# User's files (fixing ownership issues)
sudo chown -R user:user /home/user/

# Shared project directory
sudo chown -R :developers /opt/project/
sudo chmod -R g+w /opt/project/

# System files
sudo chown root:root /etc/passwd
sudo chmod 644 /etc/passwd

# Log files
sudo chown syslog:adm /var/log/syslog
sudo chmod 640 /var/log/syslog
```
---

## Navigation

**Previous:** [← The Chmod Command](02-the-chmod-command.md)  
**Next:** [→ Special Permission Bits](04-special-permission-bits.md)  
**Lesson Home:** [↑ Lesson 10: Permissions](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
