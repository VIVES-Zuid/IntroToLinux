# 13. Command Summary: Chapters 1-4

This chapter provides a comprehensive overview of all commands covered in the first four chapters of this course.

---

## 📚 Chapter 1: Introduction to Linux and Unix

**No specific commands covered** - This chapter focused on Linux history, philosophy, and distributions.

---

## 🐚 Chapter 2: The Linux Console and Terminal

### Navigation Commands

| Command | Description |
|---------|-------------|
| `pwd` | Print Working Directory - shows your current location |
| `cd` | Change Directory - navigate to home directory |
| `cd <path>` | Change to specified directory |
| `cd ~` | Change to home directory |
| `cd ..` | Move up one directory level |
| `cd -` | Return to previous directory |
| `cd /` | Change to root directory |

### File Listing Commands

| Command | Description |
|---------|-------------|
| `ls` | List files and directories in current location |
| `ls -l` | List with detailed information (long format) |
| `ls -a` | List all files including hidden files (starting with .) |
| `ls -la` | List all files with detailed information |
| `ls -lh` | List with human-readable file sizes |

### File and Directory Operations

| Command | Description |
|---------|-------------|
| `touch <file>` | Create an empty file or update timestamp |
| `mkdir <dir>` | Create a new directory |
| `mkdir -p <path>` | Create nested directories (parent directories as needed) |
| `cp <source> <dest>` | Copy file from source to destination |
| `cp -r <source> <dest>` | Copy directory recursively |
| `mv <source> <dest>` | Move or rename file/directory |
| `rm <file>` | Remove/delete a file |
| `rm -i <file>` | Remove file with confirmation prompt |
| `rm -r <dir>` | Remove directory and its contents recursively |
| `rm -rf <dir>` | Force remove directory (use with caution!) |
| `rmdir <dir>` | Remove empty directory |

### Viewing File Contents

| Command | Description |
|---------|-------------|
| `cat <file>` | Display entire file contents |
| `cat -n <file>` | Display file with line numbers |
| `less <file>` | View file with pagination (recommended) |
| `more <file>` | View file with pagination (older tool) |
| `head <file>` | Display first 10 lines of file |
| `head -n <num> <file>` | Display first n lines of file |
| `tail <file>` | Display last 10 lines of file |
| `tail -n <num> <file>` | Display last n lines of file |
| `tail -f <file>` | Follow file changes in real-time (useful for logs) |

### Text Editing

| Command | Description |
|---------|-------------|
| `nano <file>` | Open file in nano text editor (beginner-friendly) |

### File Download

| Command | Description |
|---------|-------------|
| `wget <url>` | Download file from URL |
| `wget -O <name> <url>` | Download file with custom name |

### Getting Help

| Command | Description |
|---------|-------------|
| `man <command>` | Display manual page for command |
| `man -k <keyword>` | Search manual pages by keyword |
| `apropos <keyword>` | Search manual pages (same as man -k) |
| `<command> --help` | Display quick help summary for command |
| `<command> -h` | Display short help (some commands) |

### System Information

| Command | Description |
|---------|-------------|
| `whoami` | Display current username |
| `file <file>` | Determine file type |

---

## 🌍 Chapter 3: Shell Environment and Variables

### Environment Variable Commands

| Command | Description |
|---------|-------------|
| `env` | Display all environment variables |
| `set` | Display all variables (shell and environment) |
| `echo $VAR` | Display value of specific variable |
| `export VAR=value` | Create and export environment variable |
| `export VAR` | Export existing variable to environment |

### Common Environment Variables

| Variable | Description |
|----------|-------------|
| `$HOME` | User's home directory path |
| `$USER` | Current username |
| `$PWD` | Present Working Directory |
| `$PATH` | Directories where shell looks for commands |
| `$PS1` | Primary shell prompt appearance |
| `$SHELL` | Path to current shell |

### Command History

| Command | Description |
|---------|-------------|
| `history` | Display command history |
| `history <n>` | Display last n commands |
| `history -c` | Clear command history |
| `history \| grep <term>` | Search history for specific term |
| `!!` | Execute previous command |
| `!<n>` | Execute command number n from history |
| `!<text>` | Execute last command starting with text |
| `!$` | Use last argument from previous command |

### History Keyboard Shortcuts

| Shortcut | Description |
|----------|-------------|
| `Ctrl+R` | Reverse search through command history |
| `↑` (Up Arrow) | Navigate to previous command |
| `↓` (Down Arrow) | Navigate to next command |
| `Ctrl+P` | Previous command (same as Up Arrow) |
| `Ctrl+N` | Next command (same as Down Arrow) |

### Command Line Editing Shortcuts

| Shortcut | Description |
|----------|-------------|
| `Ctrl+A` | Move cursor to beginning of line |
| `Ctrl+E` | Move cursor to end of line |
| `Ctrl+U` | Delete from cursor to beginning of line |
| `Ctrl+K` | Delete from cursor to end of line |
| `Ctrl+W` | Delete word before cursor |
| `Ctrl+L` | Clear screen |
| `Ctrl+C` | Cancel current command |

---

## 🔀 Chapter 4: I/O Redirection and Pipes

### Output Redirection

| Command | Description |
|---------|-------------|
| `cmd > file` | Redirect stdout to file (overwrite) |
| `cmd >> file` | Redirect stdout to file (append) |
| `cmd 2> file` | Redirect stderr to file (overwrite) |
| `cmd 2>> file` | Redirect stderr to file (append) |
| `cmd &> file` | Redirect both stdout and stderr to file |
| `cmd > file 2>&1` | Redirect stderr to stdout, then to file |
| `cmd > /dev/null` | Discard stdout output |
| `cmd 2> /dev/null` | Discard stderr output |
| `cmd &> /dev/null` | Discard all output |

### Input Redirection

| Command | Description |
|---------|-------------|
| `cmd < file` | Use file as input for command |
| `cmd << EOF` | Here document - multi-line input until EOF |
| `cmd <<< "text"` | Here string - use string as input |

### Pipes

| Command | Description |
|---------|-------------|
| `cmd1 \| cmd2` | Pipe output of cmd1 to input of cmd2 |
| `cmd1 \| cmd2 \| cmd3` | Chain multiple commands together |

### Text Processing Commands

| Command | Description |
|---------|-------------|
| `head <file>` | Display first 10 lines |
| `head -n <num> <file>` | Display first n lines |
| `tail <file>` | Display last 10 lines |
| `tail -n <num> <file>` | Display last n lines |
| `tail -f <file>` | Follow file changes in real-time |
| `cat <file>` | Concatenate and display file contents |
| `cat -n <file>` | Display file with line numbers |
| `cat -A <file>` | Show non-printing characters |
| `tac <file>` | Display file in reverse line order |
| `more <file>` | Page through file content |
| `less <file>` | Advanced file pager with search capabilities |
| `strings <file>` | Extract readable text from binary files |
| `strings -n <num> <file>` | Extract strings with minimum length |

### Common Pipe Examples

| Command | Description |
|---------|-------------|
| `ls \| wc -l` | Count files in directory |
| `ps aux \| grep <process>` | Find specific process |
| `cat <file> \| head -n 5` | Show first 5 lines of file |
| `history \| tail -10` | Show last 10 commands |
| `du -sh */ \| sort -hr` | Show directory sizes sorted |
| `ps aux \| sort -k4 -nr` | Sort processes by memory usage |

---

## 🎯 Quick Reference by Category

### 📂 File Management
```bash
ls, ls -la, cd, pwd, mkdir, touch, cp, mv, rm, rmdir
```

### 📖 File Viewing
```bash
cat, less, more, head, tail, strings
```

### 🔍 Information & Help
```bash
man, apropos, --help, whoami, file, which, type
```

### 🌍 Environment
```bash
env, set, export, echo $VAR
```

### 📜 History
```bash
history, !!, !n, !text, Ctrl+R
```

### 🔀 Redirection & Pipes
```bash
>, >>, 2>, &>, <, |
```

---

## 💡 Pro Tips

1. **Use Tab Completion**: Press `Tab` to auto-complete commands and file names
2. **Combine Commands**: Use pipes (`|`) to create powerful command chains
3. **Save Output**: Redirect output to files for later analysis
4. **Search History**: Use `Ctrl+R` to quickly find and reuse previous commands
5. **Read Man Pages**: When in doubt, `man <command>` is your best friend
6. **Use Aliases**: Create shortcuts for frequently used commands
7. **Be Careful**: `rm -rf` has no undo - always double-check before executing

---

## Navigation

**Next:** [→ Next Lesson](14-next-lesson.md)  
**Previous:** [← Key Takeaways](12-key-takeaways.md)  
**Lesson Home:** [↑ Lesson 4: Redirects & Pipes](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
