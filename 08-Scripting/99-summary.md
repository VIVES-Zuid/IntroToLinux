# Summary: Lesson 8 – Shell Scripting Fundamentals

**Concepts**
- Script structure: shebang `#!/bin/bash`, comments `#`
- Making scripts executable & running them (`chmod +x`, `./script.sh`, `bash script.sh`)
- Variables (no spaces around `=`), `"double"` vs `'single'` quotes
- Command substitution `$( )`, arithmetic `$(( ))` (whole numbers only)
- Arguments: `$0`, `$1`…, `$#`, `$@`; exit codes: `$?`, `exit 0` / `exit 1`
- Test conditions with `[ ]`: files (`-e`, `-f`, `-d`, `-r`, `-w`, `-x`), strings (`=`, `!=`, `-z`, `-n`), numbers (`-eq`, `-ne`, `-lt`, `-le`, `-gt`, `-ge`)
- Logical operators `&&`, `||`, `!`
- `if` / `elif` / `else` / `fi`
- Loops: `for`, `while`, `break`
- `case` / `esac`
- Best practices: header, argument check, quoting, `bash -x` for debugging

**Commands**
- `chmod +x`, `bash script.sh`, `bash -x script.sh`
- `read` (`-p`), `echo`, `exit`

**ℹ️ Advanced (optional)**
- `readonly`, `export`, default values `${1:-default}`
- `until` loops, C-style `for ((...))`, `while read` over a file
- Functions, `local`, `return`

---

## Navigation

**Next:** [→ Learning Objectives](../09-Users-Groups/00-learning-objectives.md)  
**Previous:** [← Next Lesson](13-next-lesson.md)  
**Lesson Home:** [↑ Lesson 8: Shell Scripting Fundamentals](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
