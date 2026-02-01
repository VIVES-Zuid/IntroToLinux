# 6. File System Navigation Quiz - Solutions

## Answer Key with Explanations

### Question 1
**Current Location:** `/home/alice`  
**Target:** `/home/alice/documents/reports/2024`

**Answers:**
- Absolute path: `cd /home/alice/documents/reports/2024`
- Relative path: `cd documents/reports/2024`

**Explanation:** From Alice's home directory, navigate down through the subdirectories.

---

### Question 2
**Current Location:** `/home/alice/documents/projects/web`  
**Target:** `/home/alice/documents/projects/scripts`

**Answers:**
- Absolute path: `cd /home/alice/documents/projects/scripts`
- Relative path: `cd ../scripts`

**Explanation:** Both web and scripts are in the same parent directory (projects), so go up one level then down to scripts.

---

### Question 3
**Current Location:** `/home/bob/workspace/python`  
**Target:** `/home/alice/pictures/vacation`

**Answers:**
- Absolute path: `cd /home/alice/pictures/vacation`
- Relative path: `cd ../../../alice/pictures/vacation`

**Explanation:** Go up 3 levels (python → workspace → bob → home), then down to alice/pictures/vacation.

---

### Question 4
**Current Location:** `/var/log/apache`  
**Target:** `/home/alice`

**Answers:**
- Absolute path: `cd /home/alice`
- Relative path: `cd ../../../home/alice`

**Explanation:** Go up 3 levels (apache → log → var → root), then down to home/alice.

---

### Question 5
**Current Location:** `/home/alice/documents/reports/2023`  
**Target:** `/home/alice/documents/reports/2024`

**Answers:**
- Absolute path: `cd /home/alice/documents/reports/2024`
- Relative path: `cd ../2024`

**Explanation:** Both 2023 and 2024 are in the same parent directory (reports).

---

### Question 6
**Current Location:** `/usr/local/share/doc`  
**Target:** `/usr/local/src/project/src`

**Answers:**
- Absolute path: `cd /usr/local/src/project/src`
- Relative path: `cd ../../src/project/src`

**Explanation:** Go up 2 levels (doc → share → local), then down to src/project/src.

---

### Question 7
**Current Location:** `/home/charlie/logs/application`  
**Target:** `/home/charlie/temp`

**Answers:**
- Absolute path: `cd /home/charlie/temp`
- Relative path: `cd ../../temp`

**Explanation:** Go up 2 levels (application → logs → charlie), then down to temp.

---

### Question 8
**Current Location:** `/opt/custom/config`  
**Target:** `/opt/third-party/application/data`

**Answers:**
- Absolute path: `cd /opt/third-party/application/data`
- Relative path: `cd ../../third-party/application/data`

**Explanation:** Go up 2 levels (config → custom → opt), then down to third-party/application/data.

---

### Question 9
**Current Location:** `/home/alice/downloads/software/editors`  
**Target:** `/home/alice/downloads/music/jazz`

**Answers:**
- Absolute path: `cd /home/alice/downloads/music/jazz`
- Relative path: `cd ../../music/jazz`

**Explanation:** Go up 2 levels (editors → software → downloads), then down to music/jazz.

---

### Question 10
**Current Location:** `/etc/config/network`  
**Target:** `/etc/config/apache`

**Answers:**
- Absolute path: `cd /etc/config/apache`
- Relative path: `cd ../apache`

**Explanation:** Both network and apache are in the same parent directory (config).

---

### Question 11
**Current Location:** `/home/bob/backup/daily`  
**Target:** `/home/bob/workspace/databases/mysql`

**Answers:**
- Absolute path: `cd /home/bob/workspace/databases/mysql`
- Relative path: `cd ../../workspace/databases/mysql`

**Explanation:** Go up 2 levels (daily → backup → bob), then down to workspace/databases/mysql.

---

### Question 12
**Current Location:** `/var/www/html`  
**Target:** `/var/mail/alice`

**Answers:**
- Absolute path: `cd /var/mail/alice`
- Relative path: `cd ../../mail/alice`

**Explanation:** Go up 2 levels (html → www → var), then down to mail/alice.

---

### Question 13
**Current Location:** `/usr/share/applications`  
**Target:** `/usr/local/bin`

**Answers:**
- Absolute path: `cd /usr/local/bin`
- Relative path: `cd ../../local/bin`

**Explanation:** Go up 2 levels (applications → share → usr), then down to local/bin.

---

### Question 14
**Current Location:** `/home/alice/documents/projects/scripts`  
**Target:** `/home/alice`

**Answers:**
- Absolute path: `cd /home/alice`
- Relative path: `cd ../../..`

**Explanation:** Go up 3 levels (scripts → projects → documents → alice).

---

### Question 15
**Current Location:** `/tmp/session-123`  
**Target:** `/home/charlie/logs/system`

**Answers:**
- Absolute path: `cd /home/charlie/logs/system`
- Relative path: `cd ../../home/charlie/logs/system`

**Explanation:** Go up 2 levels (session-123 → tmp → root), then down to home/charlie/logs/system.

---

### Question 16
**Current Location:** `/home/alice/pictures/family`  
**Target:** `/home/alice/documents` (directory containing notes.txt)

**Answers:**
- Absolute path: `cd /home/alice/documents`
- Relative path: `cd ../../documents`

**Explanation:** Go up 2 levels (family → pictures → alice), then down to documents.

---

### Question 17
**Current Location:** `/usr/local/src/project/build`  
**Target:** `/usr/local/src/project/src`

**Answers:**
- Absolute path: `cd /usr/local/src/project/src`
- Relative path: `cd ../src`

**Explanation:** Both build and src are in the same parent directory (project).

---

### Question 18
**Current Location:** `/home/bob/workspace/java`  
**Target:** `/home/bob/backup/weekly`

**Answers:**
- Absolute path: `cd /home/bob/backup/weekly`
- Relative path: `cd ../../backup/weekly`

**Explanation:** Go up 2 levels (java → workspace → bob), then down to backup/weekly.

---

### Question 19
**Current Location:** `/var/log/system`  
**Target:** `/etc`

**Answers:**
- Absolute path: `cd /etc`
- Relative path: `cd ../../../etc`

**Explanation:** Go up 3 levels (system → log → var → root), then down to etc.

---

### Question 20
**Current Location:** `/home/alice/downloads/software/tools`  
**Target:** `/home/alice/downloads/software/editors`

**Answers:**
- Absolute path: `cd /home/alice/downloads/software/editors`
- Relative path: `cd ../editors`

**Explanation:** Both tools and editors are in the same parent directory (software).

---

### Question 21
**Current Location:** `/opt/third-party/application/bin`  
**Target:** `/opt/custom/bin`

**Answers:**
- Absolute path: `cd /opt/custom/bin`
- Relative path: `cd ../../../custom/bin`

**Explanation:** Go up 3 levels (bin → application → third-party → opt), then down to custom/bin.

---

### Question 22
**Current Location:** `/home/charlie/logs/application`  
**Target:** `/home/alice/documents/projects/web`

**Answers:**
- Absolute path: `cd /home/alice/documents/projects/web`
- Relative path: `cd ../../../alice/documents/projects/web`

**Explanation:** Go up 3 levels (application → logs → charlie → home), then down to alice/documents/projects/web.

---

### Question 23
**Current Location:** `/usr/share/fonts`  
**Target:** `/usr/local/share/man/man1`

**Answers:**
- Absolute path: `cd /usr/local/share/man/man1`
- Relative path: `cd ../../local/share/man/man1`

**Explanation:** Go up 2 levels (fonts → share → usr), then down to local/share/man/man1.

---

### Question 24
**Current Location:** `/home/alice/documents/reports/2024`  
**Target:** `/home/alice/documents/projects`

**Answers:**
- Absolute path: `cd /home/alice/documents/projects`
- Relative path: `cd ../../projects`

**Explanation:** Go up 2 levels (2024 → reports → documents), then down to projects.

---

### Question 25
**Current Location:** `/var/www/cgi-bin`  
**Target:** `/tmp/cache`

**Answers:**
- Absolute path: `cd /tmp/cache`
- Relative path: `cd ../../../tmp/cache`

**Explanation:** Go up 3 levels (cgi-bin → www → var → root), then down to tmp/cache.

---

### Question 26
**Current Location:** `/home/bob/workspace/databases/postgres`  
**Target:** `/home/bob/workspace/python`

**Answers:**
- Absolute path: `cd /home/bob/workspace/python`
- Relative path: `cd ../../python`

**Explanation:** Go up 2 levels (postgres → databases → workspace), then down to python.

---

### Question 27
**Current Location:** `/etc/config/apache`  
**Target:** `/var/log/apache`

**Answers:**
- Absolute path: `cd /var/log/apache`
- Relative path: `cd ../../../var/log/apache`

**Explanation:** Go up 3 levels (apache → config → etc → root), then down to var/log/apache.

---

### Question 28
**Current Location:** `/home/alice/pictures/vacation`  
**Target:** `/home/bob/workspace`

**Answers:**
- Absolute path: `cd /home/bob/workspace`
- Relative path: `cd ../../../bob/workspace`

**Explanation:** Go up 3 levels (vacation → pictures → alice → home), then down to bob/workspace.

---

### Question 29
**Current Location:** `/usr/local/share/doc`  
**Target:** `/usr/share/applications`

**Answers:**
- Absolute path: `cd /usr/share/applications`
- Relative path: `cd ../../../share/applications`

**Explanation:** Go up 3 levels (doc → share → local → usr), then down to share/applications.

---

### Question 30
**Current Location:** `/home/alice/downloads/music/rock`  
**Target:** `/`

**Answers:**
- Absolute path: `cd /`
- Relative path: `cd ../../../../..`

**Explanation:** Go up 5 levels (rock → music → downloads → alice → home → root).

---

## Bonus Challenge Solutions

### Bonus 1
**Current Location:** `/home/alice/documents/reports/2023`  
**Target:** `/home/charlie/temp`

**Answer:**
- Relative path: `cd ../../../../charlie/temp`

**Explanation:** Go up 4 levels (2023 → reports → documents → alice → home), then down to charlie/temp.

### Bonus 2
**Current Location:** `/usr/local/src/project/src`  
**Target:** `/var/mail/bob`

**Answer:**
- Relative path: `cd ../../../../../var/mail/bob`

**Explanation:** Go up 5 levels (src → project → src → local → usr → root), then down to var/mail/bob.

### Bonus 3
**Current Location:** `/opt/third-party/application/data`  
**Target:** First to `/home/alice/pictures/family`, then to `/etc/config/network`

**Answers:**
- Command 1: `cd ../../../../home/alice/pictures/family`
- Command 2: `cd ../../../../etc/config/network`

**Explanation:** 
- First: Go up 4 levels to root, then down to alice's family pictures
- Second: From family pictures, go up 4 levels to root, then down to etc config network

---

## Path Analysis Patterns

### Common Relative Path Patterns

```
Moving Between Sibling Directories:
/path/to/dir1 → /path/to/dir2
Answer: cd ../dir2

Moving Up Multiple Levels:
/a/b/c/d → /a
Answer: cd ../../..

Moving Across Different Branches:
/branch1/leaf → /branch2/leaf
Answer: cd ../../branch2/leaf

Moving to Root:
/any/path/here → /
Answer: cd (count levels up to root)
```

### Tip for Counting Relative Paths

```
Visual Method:
Current:  /home/alice/documents/reports/2023
Target:   /home/bob/workspace

Step 1: Find common ancestor (/home)
        /home/alice/documents/reports/2023
              ↑ common ancestor

Step 2: Count up from current to common ancestor
        2023 → reports → documents → alice → home
        That's 4 levels up: ../../../../

Step 3: Count down from common ancestor to target
        home → bob → workspace
        That's: bob/workspace

Result: cd ../../../../bob/workspace
```

### Memory Aids

1. **Absolute paths:** Always start with `/` - you're giving the complete address
2. **Relative paths:** Think of walking from where you are - use `..` to go "back/up"
3. **Sibling directories:** If targets are in the same parent, use `../target`
4. **Root navigation:** Count the slashes in your current path to know how many `../` you need

---

## Verification Commands

After each navigation, you can verify your location:

```bash
pwd                    # Print working directory
ls                     # List contents of current directory
ls -la                 # List detailed contents including hidden files
```

---

## Navigation

**Next:** [→ Home Directory Concepts](07-home-directory-concepts.md)  
**Previous:** [← File System Navigation Quiz](05-file-system-navigation-quiz.md)  
**Lesson Home:** [↑ Lesson 2: The Shell](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
