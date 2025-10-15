# Lesson 02: File System Navigation Quiz Solutions

## Exercise: File System Navigation Quiz

**Time:** 30-45 minutes  
**Difficulty:** Intermediate  
**Skills:** Navigation, path comprehension, directory traversal

### Solution Overview

This quiz tests understanding of absolute vs relative paths using a comprehensive file system diagram with realistic directory structures.

## Complete Answer Key

### Questions 1-10: Basic Navigation

**Q1:** `/home/alice` → `/home/alice/documents/reports/2024`
- Absolute: `cd /home/alice/documents/reports/2024`
- Relative: `cd documents/reports/2024`

**Q2:** `/home/alice/documents/projects/web` → `/home/alice/documents/projects/scripts`
- Absolute: `cd /home/alice/documents/projects/scripts`
- Relative: `cd ../scripts`

**Q3:** `/home/bob/workspace/python` → `/home/alice/pictures/vacation`
- Absolute: `cd /home/alice/pictures/vacation`
- Relative: `cd ../../../alice/pictures/vacation`

**Q4:** `/var/log/apache` → `/home/alice`
- Absolute: `cd /home/alice`
- Relative: `cd ../../../home/alice`

**Q5:** `/home/alice/documents/reports/2023` → `/home/alice/documents/reports/2024`
- Absolute: `cd /home/alice/documents/reports/2024`
- Relative: `cd ../2024`

**Q6:** `/usr/local/share/doc` → `/usr/local/src/project/src`
- Absolute: `cd /usr/local/src/project/src`
- Relative: `cd ../../src/project/src`

**Q7:** `/home/charlie/logs/application` → `/home/charlie/temp`
- Absolute: `cd /home/charlie/temp`
- Relative: `cd ../../temp`

**Q8:** `/opt/custom/config` → `/opt/third-party/application/data`
- Absolute: `cd /opt/third-party/application/data`
- Relative: `cd ../../third-party/application/data`

**Q9:** `/home/alice/downloads/software/editors` → `/home/alice/downloads/music/jazz`
- Absolute: `cd /home/alice/downloads/music/jazz`
- Relative: `cd ../../music/jazz`

**Q10:** `/etc/config/network` → `/etc/config/apache`
- Absolute: `cd /etc/config/apache`
- Relative: `cd ../apache`

### Questions 11-20: Intermediate Navigation

**Q11:** `/home/bob/backup/daily` → `/home/bob/workspace/databases/mysql`
- Absolute: `cd /home/bob/workspace/databases/mysql`
- Relative: `cd ../../workspace/databases/mysql`

**Q12:** `/var/www/html` → `/var/mail/alice`
- Absolute: `cd /var/mail/alice`
- Relative: `cd ../../mail/alice`

**Q13:** `/usr/share/applications` → `/usr/local/bin`
- Absolute: `cd /usr/local/bin`
- Relative: `cd ../../local/bin`

**Q14:** `/home/alice/documents/projects/scripts` → `/home/alice`
- Absolute: `cd /home/alice`
- Relative: `cd ../../..`

**Q15:** `/tmp/session-123` → `/home/charlie/logs/system`
- Absolute: `cd /home/charlie/logs/system`
- Relative: `cd ../../home/charlie/logs/system`

**Q16:** `/home/alice/pictures/family` → `/home/alice/documents`
- Absolute: `cd /home/alice/documents`
- Relative: `cd ../../documents`

**Q17:** `/usr/local/src/project/build` → `/usr/local/src/project/src`
- Absolute: `cd /usr/local/src/project/src`
- Relative: `cd ../src`

**Q18:** `/home/bob/workspace/java` → `/home/bob/backup/weekly`
- Absolute: `cd /home/bob/backup/weekly`
- Relative: `cd ../../backup/weekly`

**Q19:** `/var/log/system` → `/etc`
- Absolute: `cd /etc`
- Relative: `cd ../../../etc`

**Q20:** `/home/alice/downloads/software/tools` → `/home/alice/downloads/software/editors`
- Absolute: `cd /home/alice/downloads/software/editors`
- Relative: `cd ../editors`

### Questions 21-30: Advanced Navigation

**Q21:** `/opt/third-party/application/bin` → `/opt/custom/bin`
- Absolute: `cd /opt/custom/bin`
- Relative: `cd ../../../custom/bin`

**Q22:** `/home/charlie/logs/application` → `/home/alice/documents/projects/web`
- Absolute: `cd /home/alice/documents/projects/web`
- Relative: `cd ../../../alice/documents/projects/web`

**Q23:** `/usr/share/fonts` → `/usr/local/share/man/man1`
- Absolute: `cd /usr/local/share/man/man1`
- Relative: `cd ../../local/share/man/man1`

**Q24:** `/home/alice/documents/reports/2024` → `/home/alice/documents/projects`
- Absolute: `cd /home/alice/documents/projects`
- Relative: `cd ../../projects`

**Q25:** `/var/www/cgi-bin` → `/tmp/cache`
- Absolute: `cd /tmp/cache`
- Relative: `cd ../../../tmp/cache`

**Q26:** `/home/bob/workspace/databases/postgres` → `/home/bob/workspace/python`
- Absolute: `cd /home/bob/workspace/python`
- Relative: `cd ../../python`

**Q27:** `/etc/config/apache` → `/var/log/apache`
- Absolute: `cd /var/log/apache`
- Relative: `cd ../../../var/log/apache`

**Q28:** `/home/alice/pictures/vacation` → `/home/bob/workspace`
- Absolute: `cd /home/bob/workspace`
- Relative: `cd ../../../bob/workspace`

**Q29:** `/usr/local/share/doc` → `/usr/share/applications`
- Absolute: `cd /usr/share/applications`
- Relative: `cd ../../../share/applications`

**Q30:** `/home/alice/downloads/music/rock` → `/`
- Absolute: `cd /`
- Relative: `cd ../../../../..`

### Bonus Challenges

**Bonus 1:** `/home/alice/documents/reports/2023` → `/home/charlie/temp`
- Relative only: `cd ../../../../charlie/temp`

**Bonus 2:** `/usr/local/src/project/src` → `/var/mail/bob`
- Shortest relative: `cd ../../../../../var/mail/bob`

**Bonus 3:** `/opt/third-party/application/data` → `/home/alice/pictures/family` → `/etc/config/network`
- Command 1: `cd ../../../../home/alice/pictures/family`
- Command 2: `cd ../../../../etc/config/network`

## Key Learning Points

### Path Resolution Strategy

1. **Absolute Paths**: Always complete and unambiguous
   - Start with `/`
   - Specify full path from root
   - Same result regardless of current location

2. **Relative Paths**: Context-dependent
   - No leading `/`
   - Based on current working directory
   - Use `..` to move up directory levels
   - Use directory names to move down

### Common Patterns

```bash
# Sibling directories (same parent)
cd ../sibling_directory

# Moving up multiple levels
cd ../../..  # 3 levels up

# Cross-branch navigation
cd ../../../other_branch/subdirectory

# Root navigation
cd /  # Absolute to root
cd $(printf '../%.0s' {1..N})  # Relative to root (N = depth)
```

### Mental Models

1. **Tree Visualization**: Think of filesystem as an inverted tree
2. **Common Ancestor Method**: Find the lowest common directory
3. **Counting Steps**: Count `../` needed to reach common ancestor
4. **Path Building**: Add target path from common ancestor

### Verification Techniques

```bash
# Always verify your location
pwd

# Preview target before navigating
ls /target/path
ls ../relative/path

# Use tab completion for validation
cd /ho<TAB>  # Expands to /home/
```

## Assessment Rubric

- **Beginner (Q1-10)**: Basic navigation within user directories
- **Intermediate (Q11-20)**: Cross-user and system directory navigation
- **Advanced (Q21-30)**: Complex multi-level and cross-system navigation
- **Expert (Bonus)**: Optimization and multi-step navigation

## Common Mistakes to Avoid

1. **Missing `..` levels**: Undercounting directory levels to traverse
2. **Absolute vs Relative confusion**: Using `/` when not needed
3. **Case sensitivity**: Linux paths are case-sensitive
4. **Missing slashes**: Forgetting `/` separators in paths
5. **Circular thinking**: Getting confused about current vs target location

---

## Navigation

**Previous:** [← Lesson 1: Introduction Solutions](lesson-01-solutions.md)  
**Next:** [→ Lesson 3: Shell Environment Solutions](lesson-03-lab-solutions.md)  
**Home:** [↑ Solutions Index](README.md)