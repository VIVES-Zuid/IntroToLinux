# Exercises: Filters & Pipelines

20 hands-on questions on `grep`, `cut`, `wc`, `tr`, `sort`, `uniq`, `comm` and pipes.

All exercises use files that exist on every freshly installed Ubuntu VM (`/etc/passwd`, `/etc/group`, `/etc/os-release`, `/etc/hostname`) or the output of standard commands (`echo`, `date`, `ls`). No extra packages or test files are needed.

> **Note:** The example outputs come from an Ubuntu 24.04 VM with a user called `student`. Your output can differ slightly (other usernames, more system accounts, other version numbers). That is normal.

**Quick reminder: the structure of `/etc/passwd`**

```
username:password:UID:GID:comment:home-directory:login-shell
   1        2      3   4     5          6             7
```

---

## grep

**1.** Show the line from `/etc/passwd` that belongs to the user `root`. Only that line may be shown.

Example output:
```
root:x:0:0:root:/root:/bin/bash
```

**2.** How many accounts in `/etc/passwd` use `/bin/bash` as login shell? Show only the number.

Example output:
```
2
```

**3.** Show all lines from `/etc/passwd` that do **not** contain the word `nologin`.

Example output:
```
root:x:0:0:root:/root:/bin/bash
sync:x:4:65534:sync:/bin:/bin/sync
student:x:1000:1000:student:/home/student:/bin/bash
```

**4.** Search `/etc/os-release` for every line that contains `ubuntu`, regardless of upper or lower case. Show the line numbers as well.

Example output:
```
1:PRETTY_NAME="Ubuntu 24.04.1 LTS"
2:NAME="Ubuntu"
6:ID=ubuntu
8:HOME_URL="https://www.ubuntu.com/"
...
```

---

## cut

**5.** Show only the usernames (first field) of the first 5 accounts in `/etc/passwd`.

Example output:
```
root
daemon
bin
sys
sync
```

**6.** Show the username and the home directory of the first 3 accounts in `/etc/passwd`.

Example output:
```
root:/root
daemon:/usr/sbin
bin:/bin
```

**7.** Show only the **values** (the part after the `=`) of all lines in `/etc/os-release` that **start with** `VERSION`.

Example output:
```
"24.04"
"24.04.1 LTS (Noble Numbat)"
noble
```

**8.** Show the first 3 characters of the first 5 usernames in `/etc/passwd`.

Example output:
```
roo
dae
bin
sys
syn
```

---

## wc

**9.** How many accounts are there in `/etc/passwd`? Show only the number, **without** the filename behind it.

**10.** Show the number of lines, words and bytes of `/etc/os-release` with a single command.

---

## tr

**11.** Show the first 5 usernames of `/etc/passwd` in UPPERCASE.

Example output:
```
ROOT
DAEMON
BIN
SYS
SYNC
```

**12.** Show every directory of your `$PATH` variable on a separate line. Then count how many directories there are in your `PATH`.

Example output (first part):
```
/usr/local/sbin
/usr/local/bin
/usr/sbin
/usr/bin
/sbin
/bin
/usr/games
/usr/local/games
/snap/bin
```

**13.** Show today's date as only digits (year, month, day), without the dashes. Start from `date +%F`.

Example output:
```
20261118
```

**14.** Use `ls -l` on `/etc/passwd`, `/etc/group` and `/etc/hostname` and show **only the size and the filename** of each file. Hint: `ls -l` uses multiple spaces between columns, so `cut` alone will not work.

Example output:
```
1029 /etc/group
9 /etc/hostname
1934 /etc/passwd
```

---

## sort

**15.** Show all usernames of `/etc/passwd`, sorted alphabetically in **reverse** order (Z → A).

**16.** Sort `/etc/passwd` numerically on the UID (field 3) and show the username and UID of the 5 accounts with the lowest UID.
Extra: what goes wrong if you forget `-n`? Explain why.

Example output:
```
root:0
daemon:1
bin:2
sys:3
sync:4
```

---

## uniq

**17.** Count how many accounts use each login shell. Show the most used shell at the top.
Extra: how many **different** login shells are used on your system? Show only the number.

Example output (the numbers will differ):
```
     24 /usr/sbin/nologin
      2 /bin/bash
      1 /bin/sync
```

**18.** Some accounts share the same primary group (GID, field 4). Show only the GID(s) that appear **more than once** in `/etc/passwd`.

Example output:
```
65534
```

---

## comm

For the last two questions, first create two sorted lists in your home directory:

- `~/users.txt`: all usernames from `/etc/passwd`, sorted
- `~/groups.txt`: all group names from `/etc/group`, sorted

**19.** Use `comm` to show only the names that appear in **both** files (users that have a group with exactly the same name).

**20.** Use `comm` to show the users that do **not** have a group with the same name. Then count how many there are.

Example output (first part, can contain more names):
```
_apt
nobody
sync
```

---

## Navigation

**Next:** [→ Summary](99-summary.md)  
**Previous:** [← Next Lesson](16-next-lesson.md)  
**Solutions:** [✔ Exercise solutions](../Solutions/lesson-07-exercises-solutions.md)  
**Lesson Home:** [↑ Lesson 7: Text Processing and Filters](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
