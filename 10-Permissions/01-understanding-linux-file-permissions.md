# 1. Understanding Linux File Permissions

## Permission Model Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    Linux Permission System                  │
│                                                             │
│  ┌─────────────┐   ┌─────────────┐   ┌─────────────┐       │
│  │    Owner    │   │    Group    │   │   Others    │       │
│  │    (User)   │   │  (Members)  │   │  (Everyone) │       │
│  │             │   │             │   │             │       │
│  │  r w x      │   │  r w x      │   │  r w x      │       │
│  │  ┌─┬─┬─┐    │   │  ┌─┬─┬─┐    │   │  ┌─┬─┬─┐    │       │
│  │  │4│2│1│    │   │  │4│2│1│    │   │  │4│2│1│    │       │
│  │  └─┴─┴─┘    │   │  └─┴─┴─┘    │   │  └─┴─┴─┘    │       │
│  └─────────────┘   └─────────────┘   └─────────────┘       │
└─────────────────────────────────────────────────────────────┘

r = Read (4)    w = Write (2)    x = Execute (1)
```

### The Permission Model
Linux uses a three-tier permission model:
- **Owner (User)**: The file's owner
- **Group**: Members of the file's group  
- **Others**: Everyone else on the system

Each tier can have three types of permissions:
- **Read (r)**: View file contents or list directory contents
- **Write (w)**: Modify file or create/delete files in directory
- **Execute (x)**: Run file as program or enter directory

## Permission String Breakdown

```
Example: -rw-r--r-- 1 user group 1024 Jan 15 10:30 file.txt

┌─┬───┬───┬───┬─┬────┬─────┬────┬──────────┬────────┐
│-│rw-│r--│r--│1│user│group│1024│Jan 15...│file.txt│
└─┴───┴───┴───┴─┴────┴─────┴────┴──────────┴────────┘
 │  │   │   │  │  │    │     │      │         │
 │  │   │   │  │  │    │     │      │         └─ Filename
 │  │   │   │  │  │    │     │      └─ Date/Time
 │  │   │   │  │  │    │     └─ Size (bytes)
 │  │   │   │  │  │    └─ Group owner
 │  │   │   │  │  └─ User owner  
 │  │   │   │  └─ Number of hard links
 │  │   │   └─ Others permissions (r--)
 │  │   └─ Group permissions (r--)
 │  └─ Owner permissions (rw-)
 └─ File type (- = regular file)

File Types:
- = regular file    d = directory    l = symbolic link
c = character dev   b = block device s = socket
p = named pipe      ? = unknown
```

### Viewing Permissions

```bash
# Long listing shows permissions
ls -l filename
ls -la directory/

# Example output:
# -rw-r--r-- 1 user group 1024 Jan 15 10:30 file.txt
# drwxr-xr-x 2 user group 4096 Jan 15 10:30 directory

# Permission breakdown:
# - : file type (- = file, d = directory, l = link)
# rw- : owner permissions (read, write, no execute)
# r-- : group permissions (read only)
# r-- : other permissions (read only)
```

## Numeric Permission System

```
Permission Calculation:
r (read)    = 4
w (write)   = 2  
x (execute) = 1

┌─────────────────────────────────────────────────────────┐
│                 Permission Examples                     │
├─────────┬────────────┬─────────────────────────────────┤
│ Numeric │ Symbolic   │ Description                     │
├─────────┼────────────┼─────────────────────────────────┤
│   755   │ rwxr-xr-x  │ Owner: all, Group/Others: r+x   │
│   644   │ rw-r--r--  │ Owner: r+w, Group/Others: r     │
│   700   │ rwx------  │ Owner: all, Group/Others: none  │
│   777   │ rwxrwxrwx  │ Everyone: all (dangerous!)     │
│   000   │ ---------  │ No permissions for anyone      │
└─────────┴────────────┴─────────────────────────────────┘

Calculation Example for 755:
Owner:  r(4) + w(2) + x(1) = 7
Group:  r(4) + -(0) + x(1) = 5  
Others: r(4) + -(0) + x(1) = 5
Result: 755
```

## Permission Effects by File Type

```
                 Regular Files              │    Directories
Permission │ Effect                        │ Effect
───────────┼──────────────────────────────┼────────────────────
Read (r)   │ View file contents           │ List directory contents
           │ cat, less, head, tail        │ ls, find
           │                              │
Write (w)  │ Modify file contents         │ Create/delete files
           │ echo >>, nano, vim           │ touch, rm, mkdir
           │                              │
Execute(x) │ Run file as program/script   │ Enter directory
           │ ./script.sh, /bin/program    │ cd, access files inside

Visual Example:
┌──────────┐    r     ┌──────────┐    w     ┌──────────┐    x     ┌──────────┐
│   User   │ ──────▶ │   Read   │ ──────▶ │  Write   │ ──────▶ │ Execute  │
│          │         │   File   │         │   File   │         │   File   │
└──────────┘         └──────────┘         └──────────┘         └──────────┘
```

## Common Permission Patterns

```bash
# Files
-rw-------  (600)  Private file (owner read/write only)
-rw-r--r--  (644)  Shared file (owner write, others read)
-rwxr-xr-x  (755)  Executable (owner all, others read/execute)
-rwxrwxrwx  (777)  World writable (dangerous!)

# Directories  
drwx------  (700)  Private directory
drwxr-xr-x  (755)  Public directory
drwxrwxrwx  (777)  World writable directory (very dangerous!)

# Scripts
-rwxr--r--  (744)  Owner executable script
-rwxr-xr-x  (755)  Public executable script
```
```

### Permission Representation

#### Symbolic Notation:
- `r` = read (4)
- `w` = write (2)  
- `x` = execute (1)
- `-` = permission not granted

#### Octal (Numeric) Notation:
Permissions are represented as a sum of values:
- Read = 4
- Write = 2
- Execute = 1

Common permission combinations:
- `755` = rwxr-xr-x (owner: all, group/others: read+execute)
- `644` = rw-r--r-- (owner: read+write, group/others: read only)
- `600` = rw------- (owner: read+write, group/others: none)
- `777` = rwxrwxrwx (all permissions for everyone - dangerous!)

### File Type Indicators:
- `-` : Regular file
- `d` : Directory
- `l` : Symbolic link
- `c` : Character device
- `b` : Block device
- `p` : Named pipe (FIFO)
- `s` : Socket
---

## Navigation

**Next:** [→ The Chmod Command](02-the-chmod-command.md)  
**Previous:** [← Learning Objectives](00-learning-objectives.md)  
**Lesson Home:** [↑ Lesson 10: Permissions](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
