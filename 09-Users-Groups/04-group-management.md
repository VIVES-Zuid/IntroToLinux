# 4. Group Management


### Creating Groups:

```bash
# Create new group
sudo groupadd newgroup

# Create group with specific GID
sudo groupadd -g 2000 newgroup

# Create system group
sudo groupadd -r systemgroup
```

### Modifying Groups:

```bash
# Change group name
sudo groupmod -n newname oldname

# Change group GID
sudo groupmod -g 2500 groupname

# Add user to group
sudo usermod -aG groupname username
sudo gpasswd -a username groupname

# Remove user from group
sudo gpasswd -d username groupname

# Set group administrators
sudo gpasswd -A admin1,admin2 groupname
```

### Deleting Groups:

```bash
# Delete group
sudo groupdel groupname

# Alternative (Debian/Ubuntu)
sudo delgroup groupname
```

### Group Membership Management:

```bash
# Check group members
getent group groupname

# List all groups
getent group

# Add multiple users to group
for user in alice bob charlie; do
    sudo usermod -aG developers $user
done

# Create group and add users in one operation
sudo groupadd project-team
sudo usermod -aG project-team alice
sudo usermod -aG project-team bob
```
---

## Navigation

**Next:** [→ The Sudo System](05-the-sudo-system.md)  
**Previous:** [← Creating And Managing Users](03-creating-and-managing-users.md)  
**Lesson Home:** [↑ Lesson 9: Users & Groups](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
