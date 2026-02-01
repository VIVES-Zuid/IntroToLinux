# 5. File System Navigation Quiz

## Sample File System Diagram

For this quiz, assume you are working with the following file system structure:

```
/                                    ← Root directory
├── bin/
├── boot/
├── dev/
├── etc/
│   ├── passwd
│   ├── hosts
│   └── config/
│       ├── network/
│       │   └── interfaces
│       └── apache/
│           └── httpd.conf
├── home/
│   ├── alice/
│   │   ├── documents/
│   │   │   ├── reports/
│   │   │   │   ├── 2023/
│   │   │   │   │   ├── january.txt
│   │   │   │   │   └── february.txt
│   │   │   │   └── 2024/
│   │   │   │       ├── march.txt
│   │   │   │       └── april.txt
│   │   │   ├── projects/
│   │   │   │   ├── web/
│   │   │   │   │   ├── index.html
│   │   │   │   │   └── style.css
│   │   │   │   └── scripts/
│   │   │   │       ├── backup.sh
│   │   │   │       └── deploy.sh
│   │   │   └── notes.txt
│   │   ├── downloads/
│   │   │   ├── software/
│   │   │   │   ├── editors/
│   │   │   │   │   └── vim.tar.gz
│   │   │   │   └── tools/
│   │   │   │       └── git.deb
│   │   │   └── music/
│   │   │       ├── rock/
│   │   │       │   └── song1.mp3
│   │   │       └── jazz/
│   │   │           └── song2.mp3
│   │   ├── pictures/
│   │   │   ├── vacation/
│   │   │   │   ├── beach.jpg
│   │   │   │   └── mountains.jpg
│   │   │   └── family/
│   │   │       └── reunion.jpg
│   │   └── .bashrc
│   ├── bob/
│   │   ├── workspace/
│   │   │   ├── python/
│   │   │   │   ├── app.py
│   │   │   │   └── utils.py
│   │   │   ├── java/
│   │   │   │   └── Main.java
│   │   │   └── databases/
│   │   │       ├── mysql/
│   │   │       │   └── schema.sql
│   │   │       └── postgres/
│   │   │           └── setup.sql
│   │   ├── backup/
│   │   │   ├── daily/
│   │   │   │   └── backup-2024-01-15.tar.gz
│   │   │   └── weekly/
│   │   │       └── backup-2024-week-03.tar.gz
│   │   └── .profile
│   └── charlie/
│       ├── logs/
│       │   ├── application/
│       │   │   ├── app.log
│       │   │   └── error.log
│       │   └── system/
│       │       └── syslog
│       └── temp/
│           └── scratch.txt
├── lib/
├── opt/
│   ├── custom/
│   │   ├── bin/
│   │   │   └── mytool
│   │   └── config/
│   │       └── settings.conf
│   └── third-party/
│       └── application/
│           ├── bin/
│           │   └── app
│           └── data/
│               └── database.db
├── tmp/
│   ├── session-123/
│   │   └── temp-file.txt
│   └── cache/
│       └── web-cache.tmp
├── usr/
│   ├── bin/
│   ├── lib/
│   ├── local/
│   │   ├── bin/
│   │   ├── share/
│   │   │   ├── doc/
│   │   │   │   └── manual.pdf
│   │   │   └── man/
│   │   │       └── man1/
│   │   │           └── command.1
│   │   └── src/
│   │       └── project/
│   │           ├── src/
│   │           │   └── main.c
│   │           └── build/
│   │               └── output
│   └── share/
│       ├── applications/
│       │   └── firefox.desktop
│       └── fonts/
│           └── arial.ttf
└── var/
    ├── log/
    │   ├── apache/
    │   │   ├── access.log
    │   │   └── error.log
    │   ├── system/
    │   │   └── messages
    │   └── auth.log
    ├── mail/
    │   ├── alice/
    │   │   └── inbox
    │   └── bob/
    │       └── inbox
    └── www/
        ├── html/
        │   ├── index.html
        │   └── about.html
        └── cgi-bin/
            └── script.cgi
```

## Navigation Quiz Questions

**Instructions:** For each question, provide both the absolute path and relative path to navigate from the current location to the target location. Use the `cd` command in your answers.

---

### Question 1
**Current Location:** `/home/alice`  
**Target:** `/home/alice/documents/reports/2024`  
**Task:** Navigate to the 2024 reports folder

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 2
**Current Location:** `/home/alice/documents/projects/web`  
**Target:** `/home/alice/documents/projects/scripts`  
**Task:** Navigate to the scripts folder

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 3
**Current Location:** `/home/bob/workspace/python`  
**Target:** `/home/alice/pictures/vacation`  
**Task:** Navigate to Alice's vacation pictures

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 4
**Current Location:** `/var/log/apache`  
**Target:** `/home/alice`  
**Task:** Navigate to Alice's home directory

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 5
**Current Location:** `/home/alice/documents/reports/2023`  
**Target:** `/home/alice/documents/reports/2024`  
**Task:** Navigate to the 2024 reports folder

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 6
**Current Location:** `/usr/local/share/doc`  
**Target:** `/usr/local/src/project/src`  
**Task:** Navigate to the project source code

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 7
**Current Location:** `/home/charlie/logs/application`  
**Target:** `/home/charlie/temp`  
**Task:** Navigate to Charlie's temp folder

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 8
**Current Location:** `/opt/custom/config`  
**Target:** `/opt/third-party/application/data`  
**Task:** Navigate to the third-party application data

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 9
**Current Location:** `/home/alice/downloads/software/editors`  
**Target:** `/home/alice/downloads/music/jazz`  
**Task:** Navigate to the jazz music folder

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 10
**Current Location:** `/etc/config/network`  
**Target:** `/etc/config/apache`  
**Task:** Navigate to the Apache configuration

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 11
**Current Location:** `/home/bob/backup/daily`  
**Target:** `/home/bob/workspace/databases/mysql`  
**Task:** Navigate to the MySQL database folder

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 12
**Current Location:** `/var/www/html`  
**Target:** `/var/mail/alice`  
**Task:** Navigate to Alice's mail folder

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 13
**Current Location:** `/usr/share/applications`  
**Target:** `/usr/local/bin`  
**Task:** Navigate to the local bin directory

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 14
**Current Location:** `/home/alice/documents/projects/scripts`  
**Target:** `/home/alice`  
**Task:** Navigate back to Alice's home directory

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 15
**Current Location:** `/tmp/session-123`  
**Target:** `/home/charlie/logs/system`  
**Task:** Navigate to Charlie's system logs

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 16
**Current Location:** `/home/alice/pictures/family`  
**Target:** `/home/alice/documents/notes.txt` (parent directory)  
**Task:** Navigate to the directory containing notes.txt

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 17
**Current Location:** `/usr/local/src/project/build`  
**Target:** `/usr/local/src/project/src`  
**Task:** Navigate to the source code directory

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 18
**Current Location:** `/home/bob/workspace/java`  
**Target:** `/home/bob/backup/weekly`  
**Task:** Navigate to the weekly backup folder

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 19
**Current Location:** `/var/log/system`  
**Target:** `/etc`  
**Task:** Navigate to the system configuration directory

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 20
**Current Location:** `/home/alice/downloads/software/tools`  
**Target:** `/home/alice/downloads/software/editors`  
**Task:** Navigate to the editors folder

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 21
**Current Location:** `/opt/third-party/application/bin`  
**Target:** `/opt/custom/bin`  
**Task:** Navigate to the custom bin directory

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 22
**Current Location:** `/home/charlie/logs/application`  
**Target:** `/home/alice/documents/projects/web`  
**Task:** Navigate to Alice's web project

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 23
**Current Location:** `/usr/share/fonts`  
**Target:** `/usr/local/share/man/man1`  
**Task:** Navigate to the manual pages

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 24
**Current Location:** `/home/alice/documents/reports/2024`  
**Target:** `/home/alice/documents/projects`  
**Task:** Navigate to the projects directory

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 25
**Current Location:** `/var/www/cgi-bin`  
**Target:** `/tmp/cache`  
**Task:** Navigate to the cache directory

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 26
**Current Location:** `/home/bob/workspace/databases/postgres`  
**Target:** `/home/bob/workspace/python`  
**Task:** Navigate to the Python workspace

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 27
**Current Location:** `/etc/config/apache`  
**Target:** `/var/log/apache`  
**Task:** Navigate to the Apache log directory

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 28
**Current Location:** `/home/alice/pictures/vacation`  
**Target:** `/home/bob/workspace`  
**Task:** Navigate to Bob's workspace

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 29
**Current Location:** `/usr/local/share/doc`  
**Target:** `/usr/share/applications`  
**Task:** Navigate to the applications directory

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

### Question 30
**Current Location:** `/home/alice/downloads/music/rock`  
**Target:** `/`  
**Task:** Navigate to the root directory

**Your Answer:**
- Absolute path: `cd _______________`
- Relative path: `cd _______________`

---

## Bonus Challenge Questions

### Bonus 1
**Current Location:** `/home/alice/documents/reports/2023`  
**Task:** Navigate to `/home/charlie/temp` using only relative paths and without using absolute paths

**Your Answer:**
- Relative path: `cd _______________`

### Bonus 2
**Current Location:** `/usr/local/src/project/src`  
**Task:** Navigate to `/var/mail/bob` using the shortest possible relative path

**Your Answer:**
- Relative path: `cd _______________`

### Bonus 3
**Current Location:** `/opt/third-party/application/data`  
**Task:** Navigate to `/home/alice/pictures/family` and then to `/etc/config/network` using two commands

**Your Answer:**
- Command 1: `cd _______________`
- Command 2: `cd _______________`

---

## Tips for Success

1. **Absolute Paths:** Always start with `/` and specify the complete path from root
2. **Relative Paths:** Use `..` to go up directories, and directory names to go down
3. **Special Symbols:**
   - `.` = current directory
   - `..` = parent directory
   - `~` = home directory (user's home)
   - `/` = root directory
4. **Count Your Steps:** For relative paths, count how many levels up (`..`) you need to go
5. **Draw It Out:** Sketch the path on paper if needed

---

## Navigation

**Next:** [→ Quiz Solutions](06-file-system-navigation-quiz-solutions.md)  
**Previous:** [← Linux File System Structure](04-linux-file-system-structure.md)  
**Lesson Home:** [↑ Lesson 2: The Shell](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
