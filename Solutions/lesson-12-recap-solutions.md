# Lesson 12: Recap — Solutions

Solutions to all exercises in the [Lesson 12 Recap](../12-Recap/00-overview.md).

---

## Section 1: File Listing and Navigation

**1.1** List the contents of `/usr/share` in long format, including hidden files, with human-readable sizes.
```bash
ls -lha /usr/share
```

**1.2** Display the inode number alongside the details of every file in `/etc`.
```bash
ls -li /etc
```

**1.3** From your home directory, list `/usr/share` in detail (including hidden files), save to `details.txt`, and display sorted by file size — one command.
```bash
ls -la /usr/share | tee details.txt | sort -k5 -n
```
> `tee` saves the output to `details.txt` while passing it on to `sort`. Using `-k5 -n` sorts on the 5th field (size) numerically.

**1.4** Find regular files in `/etc` that are exactly 16 bytes. Suppress errors.
```bash
find /etc -type f -size 16c 2>/dev/null
```
> `-size 16c` means exactly 16 bytes (`c` = bytes/characters). Use `/dev/null` to discard stderr.

**1.5** Find regular files in `~` starting with uppercase, ending in `.txt`. No subdirectories.
```bash
find ~ -maxdepth 1 -type f -name '[A-Z]*.txt'
```
> `-maxdepth 1` restricts to the home directory itself. `[A-Z]*` matches names starting with an uppercase letter.

**1.6** Find all files in `/boot` ending in `.cfg`. Suppress errors.
```bash
find /boot -type f -name '*.cfg' 2>/dev/null
```

**1.7** Locate a file in `/etc` with inode number `98765`.
```bash
find /etc -inum 98765
```

**1.8** Copy `/etc/resolvconf/update.d/libc` to `/tmp/` using an absolute path.
```bash
cp /etc/resolvconf/update.d/libc /tmp/
```

**1.9** Move `notes.txt` to the parent directory using a relative path (from `/home/student/projects/`).
```bash
mv notes.txt ../
```

**1.10** Copy `~/documents/report.pdf` to `~/backup/` using relative paths (from home directory).
```bash
cp documents/report.pdf backup/
```
> From `~`, both paths are relative: no leading `/` or `~`.

---

## Section 2: I/O, Redirects and Pipelines

**2.1** Write `ls /etc` to `etcfiles.txt`, then append `--- end of list ---`.
```bash
ls /etc > etcfiles.txt
echo "--- end of list ---" >> etcfiles.txt
```

**2.2** Run `find /etc -type f`, save errors to `find_errors.txt`, show normal output on screen.
```bash
find /etc -type f 2> find_errors.txt
```

**2.3** Display the 10 largest files in `/var/log` by size.
```bash
ls -lS /var/log | head -n 11
```
> `-S` sorts by size (largest first). `head -n 11` gets 10 files + the `total` header line. Alternatively: `ls -lS /var/log | head -11`.

**2.4** Count how many entries are in `/usr/bin`.
```bash
ls /usr/bin | wc -l
```

**2.5** List `/usr/share` in long format with hidden files, save to `details.txt`, display sorted by file size largest last — one command.
```bash
ls -la /usr/share | tee details.txt | sort -k5 -n
```
> For largest last, use `sort -k5 -n`. For largest first, use `sort -k5 -rn`.

**2.6** Find `.conf` files in `/etc`, save to `conf_files.txt`, count them simultaneously. Suppress errors.
```bash
find /etc -name "*.conf" 2>/dev/null | tee conf_files.txt | wc -l
```

**2.7** Append a line to `~/processinfo.txt` with process count, shell name, hostname — separated by `@`.
```bash
echo "$(ps -e --no-headers | wc -l)@$(basename $SHELL)@$(hostname)" >> ~/processinfo.txt
```
> `ps -e --no-headers | wc -l` counts all running processes.  
> `basename $SHELL` extracts just `bash` from `/bin/bash`.  
> `>>` appends without overwriting.

**2.8** Append a line to `~/sysinfo.txt` with user, date, and file count in `/etc` — separated by `:`.
```bash
echo "$USER:$(date +%Y-%m-%d):$(ls /etc | wc -l)" >> ~/sysinfo.txt
```

**2.9** For each filename in `newfiles.txt`, create an empty file in your home directory. Use `xargs`.
```bash
cat newfiles.txt | xargs -I{} touch ~/{}
```
> `-I{}` uses `{}` as the placeholder for each input item, so `touch ~/{}` expands correctly.  
> Alternative: `xargs -n 1 -I{} touch ~/{} < newfiles.txt`

**2.10** Remove each directory listed in `dirs_to_remove.txt` using `xargs`.
```bash
cat dirs_to_remove.txt | xargs rmdir
```

---

## Section 3: Text Filters

**3.1** Show the last 3 lines of `/var/log/dpkg.log`.
```bash
tail -n 3 /var/log/dpkg.log
```

**3.2** Find the **first** occurrence of "error" (case-insensitive) in `/var/log/dpkg.log`.
```bash
grep -m 1 -i "error" /var/log/dpkg.log
```
> `-m 1` stops after the first match. `-i` makes it case-insensitive.

**3.3** Show lines in `/etc/passwd` that do **not** contain `nologin`.
```bash
grep -v "nologin" /etc/passwd
```

**3.4** Extract usernames from `/etc/passwd` (field 1, delimiter `:`).
```bash
cut -d':' -f1 /etc/passwd
```

**3.5** Extract username and home directory from `/etc/passwd` (fields 1 and 6).
```bash
cut -d':' -f1,6 /etc/passwd
```

**3.6** Show the home directory of every user whose shell is `/bin/bash`.
```bash
grep "/bin/bash" /etc/passwd | cut -d':' -f6
```

**3.7** Show group names from `/etc/group` where field 4 is not empty.
```bash
grep -v ':$' /etc/group | cut -d':' -f1
```
> Lines ending with `:` have an empty last field. `-v ':$'` keeps lines that don't end with `:`.

**3.8** Convert `names.txt` to uppercase and save to `upper.txt`.
```bash
tr 'a-z' 'A-Z' < names.txt > upper.txt
```

**3.9** Replace spaces with underscores and join all lines into one. Write to `oneline.txt`.
```bash
tr -d '\n' < sentence.txt | tr ' ' '_' > oneline.txt
```
> Delete newlines first (joins lines), then replace spaces with underscores.

**3.10** Remove all `:` characters from `data.txt` and display on screen.
```bash
tr -d ':' < data.txt
```

**3.11** Show each unique word in `words.txt` with its frequency, most frequent first.
```bash
sort words.txt | uniq -c | sort -nr
```

**3.12** Count files in `/boot` per file extension, sorted most to least frequent. Suppress errors.
```bash
find /boot -type f 2>/dev/null | sed 's/.*\.//' | sort | uniq -c | sort -nr
```
> `sed 's/.*\.//'` strips everything up to and including the last dot, leaving just the extension.  
> Alternative: `find /boot -type f | cut -d'.' -f2- | sort | uniq -c | sort -nr`

**3.13** Show all files in `/tmp` sorted by modification time, oldest first.
```bash
ls -ltr /tmp | tail -n +2
```
> `ls -ltr`: long format (`-l`), all (`-t` sort by time), reversed (`-r` = oldest first).  
> `tail -n +2` skips the `total` header line.  
> Alternative: `ls -ltr /tmp` (the `total` line is acceptable in some contexts).

**3.14** List `/var/log` in long format sorted by file size, largest first.
```bash
ls -l /var/log | sort -k5 -rn
```

**3.15** Display lines in both `list_a.txt` and `list_b.txt` (both already sorted).
```bash
comm -12 list_a.txt list_b.txt
```
> `-1` suppresses lines only in file1, `-2` suppresses lines only in file2 → only common lines remain.

**3.16** Display lines in `list_a.txt` but not in `list_b.txt` (both already sorted).
```bash
comm -23 list_a.txt list_b.txt
```
> `-2` suppresses lines only in file2, `-3` suppresses common lines → only lines unique to file1.

**3.19** Count how many lines are in `/etc/passwd`.
```bash
wc -l /etc/passwd
```

**3.20** Count how many processes are currently running.
```bash
ps -e --no-headers | wc -l
```
> `ps -e` lists all processes. `--no-headers` removes the header line so the count is accurate.

**3.21** Count how many regular files exist in `/var/log`. Suppress errors.
```bash
find /var/log -type f 2>/dev/null | wc -l
```

**3.22** Count lines in `/var/log/dpkg.log` containing "error" (case-insensitive). Two solutions:
```bash
# Pipeline solution:
grep -i "error" /var/log/dpkg.log | wc -l

# grep flag solution (shortcut):
grep -ci "error" /var/log/dpkg.log
```
> `-c` makes `grep` print only the count of matching lines — no need for `wc -l`.

---

**3.23** From `data.csv`, print each row as `Column 3: (N) value!` where N is the line number.
```bash
awk -F',' '{print "Column 3: (" NR ") " $3 "!"}' data.csv
```

**3.24** From `/etc/passwd`, print each username and shell as `user -> shell`.
```bash
awk -F':' '{print $1 " -> " $7}' /etc/passwd
```

---

## Section 4: Links and Permissions

**4.1** Create a soft link `myShell` in `~` pointing to `/bin/bash`.
```bash
ln -s /bin/bash ~/myShell
```
> `-s` creates a symbolic link. If `/bin/bash` is deleted, the link will be broken.

**4.2** Create a hard link `backup.conf` to `/etc/hostname` in `~`.
```bash
ln /etc/hostname ~/backup.conf
```
> No `-s` flag = hard link. Both names point to the same inode.

**4.3** Confirm `backup.conf` and `/etc/hostname` share the same inode.
```bash
ls -li ~/backup.conf /etc/hostname
```
> Both lines should show the same inode number in column 1.

**4.4** Which link type survives original file deletion?
> **Hard link.** The file data stays on disk as long as at least one hard link exists.

**4.5** Set `examenscript.sh`: owner=none, group=read, others=read+execute. One command.
```bash
chmod u=,g=r,o=rx examenscript.sh
```
> `u=` sets owner to nothing. `g=r` sets group to read only. `o=rx` sets others to read+execute.  
> Alternative: `chmod u-rwx,g=r,o=rx examenscript.sh`

**4.6** Add execute permission for the owner on all `.sh` files in the current directory.
```bash
chmod u+x *.sh
```

**4.7** Remove write from owner and set others to read-only — one command.
```bash
chmod u-w,o=r file.txt
```

**4.8** Set `report.txt`: owner=r+x, group=none, others=write only (octal).
```bash
chmod 502 report.txt
```
> owner: r-x = 5, group: --- = 0, others: -w- = 2 → **502**

**4.9** What octal value represents owner=rwx, group=r-x, others=r--?
> **754**  
> owner: rwx = 7, group: r-x = 5, others: r-- = 4

**4.10** What are the permissions for octal `634`?
> owner: rw- (6), group: -wx (3), others: r-- (4) → `-rw--wxr--`

**4.11** Set all files inside `~/executables/` to owner=rw, group=wx, others=r. Octal. Contents only.
```bash
chmod 634 ~/executables/*
```
> owner: rw- = 6, group: -wx = 3, others: r-- = 4 → **634**  
> The `*` applies only to files inside the directory, not the directory itself.

**4.12** Recursively set all regular files in `~/project/` to permission `644`.
```bash
find ~/project -type f -exec chmod 644 {} \;
```

---

## Section 5: Alias and Scripting

**5.1** Create a temporary alias `lsize`.
```bash
alias lsize='ls -lhS'
```

**5.2** Create a permanent alias `mygrep`.
```bash
# Step 1: Add to ~/.bashrc
echo "alias mygrep='grep -i --color'" >> ~/.bashrc

# Step 2: Reload .bashrc
source ~/.bashrc
```

**5.3** Script `hello.sh`:
```bash
#!/bin/bash
if [[ $# -gt 1 ]]; then
    echo "Error: too many arguments."
    exit 1
elif [[ $# -eq 1 ]]; then
    echo "Hello, $1!"
else
    echo "Hello, World!"
fi
```

**5.4** Script `check_args.sh`:
```bash
#!/bin/bash
if [[ $# -eq 0 ]]; then
    echo "Error: no argument provided."
    exit 1
elif [[ $# -gt 1 ]]; then
    echo "Error: too many arguments."
    exit 1
fi
echo "Argument received: $1"
```

**5.5** Script `mode.sh`:
```bash
#!/bin/bash
if [[ $# -ne 1 ]]; then
    echo "Error: exactly one argument required."
    exit 1
fi
case "$1" in
    -r) echo "Read mode" ;;
    -w) echo "Write mode" ;;
    -x) echo "Execute mode" ;;
    *)  echo "Unknown mode: $1"; exit 1 ;;
esac
```

**5.6** Script `studylog.sh`:
```bash
#!/bin/bash
LOGFILE="/home/$USER/studylog.txt"

if [[ $# -eq 0 ]]; then
    read -p "Name: " naam
    read -p "Time slot (e.g. 14:00-16:00): " tijdslot
    read -p "Date (YYYY-MM-DD): " datum
    timestamp=$(date "+%Y-%m-%d %H:%M")
    echo "${naam}#${tijdslot}#${datum}#${timestamp}" >> "$LOGFILE"

elif [[ "$1" == "-l" && $# -eq 1 ]]; then
    read -p "How many recent entries to show? " aantal
    tail -n "$aantal" "$LOGFILE"

elif [[ "$1" == "-d" && $# -eq 1 ]]; then
    > "$LOGFILE"
    echo "Log file cleared."

elif [[ $# -gt 1 ]]; then
    echo "Error: Incorrect usage. No arguments provided or too many arguments."
    exit 1

else
    echo "Invalid option! Use -l to view recent entries or -d to clear the log file."
    exit 1
fi
```
> **Key points:**  
> - `LOGFILE` uses an absolute path with `$USER` for portability.  
> - `> "$LOGFILE"` truncates the file to zero bytes without deleting it — the cleanest way to "clear" a file.  
> - Each `elif` checks both the flag value and `$#` to avoid matching wrong usage.

---

**Back to exercises:** [↑ Lesson 12: Exam Preparation Recap](../12-Recap/00-overview.md)
