# 1. Introduction to Shell Scripting

### What Is a Shell Script?

A shell script is a **text file with commands**, the same commands you type in the terminal. Instead of typing them one by one, you save them in a file and run the file.

```
 Typing by hand, every time:          With a script, once:

 $ mkdir backup                       $ ./backup.sh
 $ cp *.txt backup/                        │
 $ date > backup/when.txt                  ▼
 $ ls backup                         ┌────────────────────────┐
                                     │ #!/bin/bash            │
                                     │ mkdir backup           │
                                     │ cp *.txt backup/       │
                                     │ date > backup/when.txt │
                                     │ ls backup              │
                                     └────────────────────────┘
```

### Why Write Scripts?

- **Automation**: don't repeat the same commands by hand
- **Consistency**: the task is done exactly the same way every time
- **Fewer mistakes**: no typos in step 7 of 10
- **Sharing**: give the script to a colleague and they can do the same task

### When to Use a Shell Script?

- Tasks you do more than twice (backups, cleaning up files, setting up a folder structure)
- Combining commands you already know: `grep`, `cut`, `sort`, `tar`, ...
- Small system administration jobs

> 💡 Everything you learned in the previous lessons (pipes, redirects, filters, variables) works **inside** a script too. A script just adds decisions (`if`) and repetition (loops).

---

## Navigation

**Next:** [→ Creating Your First Script](02-creating-your-first-script.md)  
**Previous:** [← Learning Objectives](00-learning-objectives.md)  
**Lesson Home:** [↑ Lesson 8: Shell Scripting Fundamentals](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
