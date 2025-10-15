# 2. User Information Files


### /etc/passwd: User Account Information

The `/etc/passwd` file contains basic user account information:

```bash
# View the file
cat /etc/passwd

# Format: username:x:UID:GID:GECOS:home_directory:shell
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
john:x:1000:1000:John Doe,,,:/home/john:/bin/bash
```

#### Field Explanation:
1. **Username**: Login name
2. **Password**: `x` (password stored in /etc/shadow)
3. **UID**: User ID number
4. **GID**: Primary group ID
5. **GECOS**: Full name and other info
6. **Home Directory**: User's home directory path
7. **Shell**: Default shell program

### /etc/shadow: Password Information

The `/etc/shadow` file contains encrypted passwords and password policies:

```bash
# View shadow file (requires root)
sudo cat /etc/shadow

# Format: username:encrypted_password:last_change:min:max:warn:inactive:expire
root:$6$randomsalt$encryptedpassword:19000:0:99999:7:::
john:$6$anothersalt$anotherpassword:19500:0:99999:7:::
```

#### Field Explanation:
1. **Username**: Login name
2. **Encrypted Password**: Hashed password (empty = no password, ! = locked)
3. **Last Change**: Days since password was last changed (since Jan 1, 1970)
4. **Minimum**: Minimum days between password changes
5. **Maximum**: Maximum days password is valid
6. **Warning**: Days before password expiry to warn user
7. **Inactive**: Days after expiry before account is disabled
8. **Expire**: Account expiration date

### /etc/group: Group Information

```bash
# View group file
cat /etc/group

# Format: group_name:x:GID:member_list
root:x:0:
adm:x:4:syslog,john
sudo:x:27:john,alice
john:x:1000:
```

#### Field Explanation:
1. **Group Name**: Name of the group
2. **Password**: Usually `x` (group passwords rarely used)
3. **GID**: Group ID number
4. **Members**: Comma-separated list of group members

### Examining User Information:

```bash
# Check user ID and groups
id
id username

# Check current user
whoami

# Check user information
finger username      # If finger is installed
getent passwd username

# Check group membership
groups
groups username

# Check who's logged in
who
w
last                  # Login history
```
---

## Navigation

**Previous:** [← Introduction To Users And Groups](01-introduction-to-users-and-groups.md)  
**Next:** [→ Creating And Managing Users](03-creating-and-managing-users.md)  
**Lesson Home:** [↑ Lesson 09: Users Groups](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
