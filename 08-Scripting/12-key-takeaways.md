# Key Takeaways

- A shell script is a text file with commands: `#!/bin/bash` on line 1, `chmod +x`, run with `./script.sh`
- Variables: `name="value"` (no spaces around `=`), use them as `"$name"` (with double quotes)
- `$( )` stores the output of a command, `$(( ))` calculates with whole numbers
- `read` asks for input while the script runs; `$1`, `$2`, `$#`, `$@` hold the arguments given at startup
- `if [ test ]; then ... fi` makes decisions. Mind the spaces inside `[ ]`
- `for` loops over a list, `while` repeats as long as a condition is true
- `case` is a clean way to handle a menu or a list of options
- Check your arguments, quote your variables, `exit 1` on errors, debug with `bash -x`

---

## Navigation

**Next:** [→ Next Lesson](13-next-lesson.md)  
**Previous:** [← Review Questions](11-review-questions.md)  
**Lesson Home:** [↑ Lesson 8: Shell Scripting Fundamentals](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
