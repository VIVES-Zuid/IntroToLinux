# 8. User Security Best Practices

### ✅ The Basics

| ✅ Do | Why |
|------|-----|
| Use `sudo` instead of working as root | you only have root rights when you need them, and every command is logged |
| Give `sudo` rights only to people who need them | fewer accounts that can break or misuse the system |
| Lock accounts that are not used (`sudo usermod -L user`) | an unused account with a weak password is an easy target |
| Delete accounts of people who left (`sudo userdel -r user`) | after making a backup if needed |
| Use strong, unique passwords | `passwd` warns you about weak ones |
| Keep home directories private (`750`, Ubuntu's default) | other users can't read your files |

### Disabling an Account Without Deleting It

```bash
sudo usermod -L -s /usr/sbin/nologin username   # lock the password + no login shell
sudo usermod -U -s /bin/bash username           # ... and re-enable later
```

### Monitoring User Activity

```bash
who                 # who is logged in
w                   # who is logged in and what are they doing
last username       # login history of one user
lastlog             # last login of every account
```

### ℹ️ Advanced: Password Aging with chage

> ℹ️ **Advanced** – optional on a first read.

```bash
sudo chage -l username                # show the password and account expiry info
sudo chage -M 90 -m 7 -W 7 username   # max 90 days valid, min 7 days between changes, warn 7 days before
sudo chage -d 0 username              # force a new password at the next login
sudo chage -E 2027-06-30 username     # account expires on 30 June 2027
```

### ℹ️ Advanced: Password Quality and Lockout (PAM)

> ℹ️ **Advanced** – optional. These settings are done through **PAM** (Pluggable Authentication Modules); a configuration error can lock you out, so practice on a VM only.

```bash
# Stronger password rules
sudo apt install libpam-pwquality
# configured in /etc/security/pwquality.conf, e.g.:  minlen = 12

# Lock an account after too many failed logins: pam_faillock
# (the older pam_tally2 no longer exists on current Ubuntu versions)
sudo faillock --user username          # show failed login attempts
sudo faillock --user username --reset  # reset the counter / unlock
```

### ℹ️ Advanced: Permissions in Home Directories

These commands use `chmod` and `find` from **lesson 10**:

```bash
sudo chmod 750 /home/username                      # home directory: owner all, group read/enter, others nothing
find /home -type f -perm -002 -ls 2>/dev/null       # world-writable files (anyone can change them!)
```

---

## Navigation

**Next:** [→ Troubleshooting User Issues](09-troubleshooting-user-issues.md)  
**Previous:** [← Practical User Management Scenarios](07-practical-user-management-scenarios.md)  
**Lesson Home:** [↑ Lesson 9: Users and Groups](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
