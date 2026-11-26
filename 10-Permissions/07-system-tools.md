# 7. System Tools

### locate: Fast File Finding

`locate` doesn't search the disk: it searches a **database** of all filenames. That makes it very fast, but the database is only updated once a day.

| | `find` | `locate` |
|---|---|---|
| Speed | slower: searches the disk live | very fast: searches a database |
| Up to date? | always | only after `updatedb` (daily) |
| Search on | name, size, time, owner, permissions ... | name / path only |

```bash
# Install it if needed (Ubuntu uses plocate)
sudo apt install plocate

# Update the database now (otherwise brand-new files are not found)
sudo updatedb

locate passwd             # all paths containing "passwd"
locate -i readme          # ignore upper/lowercase
locate -n 10 ".conf"      # show at most 10 results
locate -e filename        # only files that still exist
locate /etc/ | grep ssh   # only results under /etc that contain "ssh"
```

### date: Date and Time

```bash
date                              # Mon Jan 15 14:30:00 CET 2024
date +%F                          # 2024-01-15
date "+%Y-%m-%d %H:%M:%S"         # 2024-01-15 14:30:00
date "+%A %d %B %Y"               # Monday 15 January 2024
date -u                           # in UTC
```

| Code | Meaning | Example |
|------|---------|---------|
| `%Y` | year | 2024 |
| `%m` | month (01-12) | 01 |
| `%d` | day (01-31) | 15 |
| `%H:%M:%S` | hours:minutes:seconds | 14:30:00 |
| `%F` | the same as `%Y-%m-%d` | 2024-01-15 |
| `%A` / `%B` | name of the day / month | Monday / January |

> 💡 `date +%F` is perfect for filenames in scripts: `backup-$(date +%F).tar.gz` → `backup-2024-01-15.tar.gz`, and those names sort in the right order.

### ℹ️ Advanced date Tricks

```bash
date -d "yesterday"               # another date than today
date -d "next monday"
date -d "2024-01-15" +%A          # which day of the week was that?
date +%s                          # seconds since 1 Jan 1970 (a "Unix timestamp")
date -d @1705329000               # timestamp back to a readable date

sudo timedatectl set-time "2024-01-15 14:30:00"   # set the clock (normally synced automatically via NTP)
```

### cal: Calendar

```bash
# Install it if needed
sudo apt install ncal

cal                       # this month
cal 1 2024                # January 2024
cal 2024                  # the whole year
cal -3                    # previous, current and next month
```

### sleep and time

```bash
sleep 5                   # wait 5 seconds (handy in scripts and loops)
sleep 2m                  # 2 minutes (s, m, h, d)

time ls -la /etc          # how long did a command take?
# real  0m0.012s   ← clock time
# user  0m0.004s   ← CPU time of the program itself
# sys   0m0.008s   ← CPU time of the kernel for the program
```

---

## Navigation

**Next:** [→ Practical Labs](08-practical-labs.md)  
**Previous:** [← The Find Command Advanced File Search](06-the-find-command-advanced-file-search.md)  
**Lesson Home:** [↑ Lesson 10: File Permissions and Tools](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
