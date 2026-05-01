# 2. I/O, Redirects and Pipelines

---

## 2.1 Output Redirection: `>` and `>>`

| Operator | Effect |
|----------|--------|
| `>` | Write stdout to file — **overwrites** existing content |
| `>>` | Append stdout to file — preserves existing content |
| `2>` | Redirect stderr (error output) to file |
| `2>/dev/null` | Discard all error messages |

```bash
ls /etc > listing.txt         # overwrite listing.txt
echo "Done" >> listing.txt    # append to listing.txt
find /etc 2>/dev/null         # suppress permission errors
find /etc 2> errors.txt       # save errors to a file
```

### Exercises

**2.1** Write the output of `ls /etc` to `etcfiles.txt`. Then append the text `--- end of list ---` to the same file. Use two separate commands.

**2.2** Run `find /etc -type f` from your home directory and save any error messages to `find_errors.txt`, while displaying normal output on screen.

---

## 2.2 Pipes `|`

A pipe sends the **stdout** of one command directly to the **stdin** of the next. No temporary file needed.

```bash
command1 | command2 | command3
```

```bash
ls /etc | grep "conf"         # filter ls output
cat /etc/passwd | sort        # sort the file
ps aux | grep bash | wc -l    # count bash processes
```

> The power of Linux comes from **chaining simple tools** into pipelines. Most exam questions test this skill.

### Exercises

**2.3** Display the 10 largest files in `/var/log` by size (use `ls -lS` and show only the top 10).

**2.4** Count how many files and directories are in `/usr/bin`.

---

## 2.3 `tee` — Split Output to File and Screen

`tee` reads stdin and writes it to **both stdout and a file** simultaneously.

```bash
command | tee file.txt          # write to file AND show on screen
command | tee file.txt | next   # also pass to next command in pipeline
command | tee -a file.txt       # append instead of overwrite
```

```bash
ls -la /usr/share | tee listing.txt | sort -k5 -n
# ^ saves the ls output to listing.txt, then sorts and displays
```

> `ls -la ... > file.txt | sort` does NOT work — `>` consumes all output before the pipe.  
> Use `tee` when you need to **save and continue processing** in one pipeline.

### Exercises

**2.5** List `/usr/share` in long format including hidden files. Save the output to `details.txt` **and** display the same list sorted by file size (column 5, numeric, largest last). Use a single command.

**2.6** Run `find /etc -name "*.conf"` and save the results to `conf_files.txt` while simultaneously counting the total number of results (pipe to `wc -l`). Suppress errors.

---

## 2.4 `echo` with Variables and Command Substitution

`echo` prints text. Its real power in the exam comes from combining it with:

- **Environment variables**: `$SHELL`, `$HOSTNAME`, `$HOME`, `$USER`
- **Command substitution**: `$(command)` — runs a command and inserts its output
- **Redirection**: `>>` to append to files

```bash
echo $SHELL                           # prints /bin/bash
echo $(hostname)                      # prints the server name
echo "Shell: $(basename $SHELL)"      # prints: Shell: bash
echo "$(date): started" >> log.txt    # appends with timestamp
```

> ℹ️ `basename $SHELL` strips the path: `/bin/bash` → `bash`.  
> `$HOSTNAME` and `$(hostname)` usually give the same result — both are safe.

### Exercises

**2.7** In one command, append a line to `~/processinfo.txt` containing: the number of running processes (use `ps -e --no-headers | wc -l`), the current shell name (just the name, not the path), and the hostname — separated by `@`. Do **not** overwrite existing content.

**2.8** Append a line to `~/sysinfo.txt` containing the current user (`$USER`), the current date (`$(date +%Y-%m-%d)`), and the number of files in `/etc` — separated by `:`.

---

## 2.5 `xargs` — Turn stdin into Arguments

`xargs` reads items from stdin (one per line, or separated by whitespace) and passes them as arguments to a command.

```bash
cat filenames.txt | xargs touch           # touch each filename from the file
cat filenames.txt | xargs -n 1 touch ~/   # touch one at a time, in home dir
cat filenames.txt | xargs -I{} touch ~/{}  # use {} placeholder for full control
xargs -n 1 -I{} touch ~/{}  < filenames.txt  # same but using input redirection
```

| Option | Meaning |
|--------|---------|
| `-n 1` | Pass one argument at a time |
| `-I{}` | Use `{}` as placeholder for each argument |

> `xargs` bridges the gap when a command does not read from stdin. For example, `touch` does not accept a list of names from stdin — but `xargs touch` makes it work.

### Exercises

**2.9** The file `newfiles.txt` contains one filename per line. For each filename, create an empty file with that name in your **home directory**. Use `xargs`.

**2.10** The file `dirs_to_remove.txt` contains one directory name per line. Use `xargs` to remove each directory (use `rmdir`).

---

## Navigation

**Next:** [→ Text Filters](03-text-filters.md)
**Previous:** [← File Listing and Navigation](01-filesystem-and-listing.md)
**Lesson Home:** [↑ Lesson 12: Exam Preparation Recap](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
