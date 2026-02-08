# 7. Essential Navigation Commands

---

## 🧭 Basic Navigation

### 🎯 Core Commands

| Command | Purpose | Icon |
|---------|---------|------|
| `pwd` | **P**rint **W**orking **D**irectory | 📍 |
| `cd` | **C**hange **D**irectory | 🚶 |
| `ls` | **L**i**s**t directory contents | 📋 |

---

## 🧪 Lab 2.1: Basic Navigation (5 minutes)

### Step-by-Step Guide

```bash
# 1️⃣ Check current directory
pwd
# Output: /home/user

# 2️⃣ Go to home directory (all equivalent)
cd
cd ~
cd $HOME

# 3️⃣ Go to root directory
cd /

# 4️⃣ Go back to home directory
cd ~

# 5️⃣ List current directory contents
ls

# 6️⃣ List with details
ls -l

# 7️⃣ List including hidden files
ls -la
```

---

## 📺 Sample Terminal Session

```bash
user@localhost:~$ pwd
/home/user
┌────────────────────────────┐
│ ✅ Shows current location  │
└────────────────────────────┘

user@localhost:~$ cd
user@localhost:~$ pwd
/home/user
┌────────────────────────────┐
│ ✅ Still in home directory │
└────────────────────────────┘

user@localhost:~$ cd /
user@localhost:~$ pwd
/
┌────────────────────────────┐
│ ✅ Now at root directory   │
└────────────────────────────┘

user@localhost:~$ cd ~
user@localhost:~$ pwd
/home/user
┌────────────────────────────┐
│ ✅ Back to home directory  │
└────────────────────────────┘
```

---

## 🎓 Navigation Shortcuts

| Shortcut | Destination | Example |
|----------|-------------|---------|
| `cd` or `cd ~` | 🏠 Home directory | `/home/user` |
| `cd /` | 🌳 Root directory | `/` |
| `cd ..` | ⬆️ Parent directory | Up one level |
| `cd -` | ↩️ Previous directory | Last location |
| `cd ~/Documents` | 📁 Subdirectory of home | `/home/user/Documents` |
---

## Navigation

**Next:** [→ Essential File Commands](08-essential-file-commands.md)  
**Previous:** [← File System Permissions](06-file-system-permissions.md)  
**Lesson Home:** [↑ Lesson 2: The Shell](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
