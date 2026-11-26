# 13. Best Practices

### ✅ Build Step by Step
- Start with one command, check the output, then add the next `| command`
- Use the ↑ arrow key to recall the previous command and extend it
- Use `tee step.txt` in the middle of a pipeline to save what the data looks like at that point

### ✅ Filter Early
- Put `grep` (select lines) **early** in the pipeline: less data for the next commands
- `sort` before `uniq` and before `comm`, always

### ✅ Know the Classic Traps

| Trap | Fix |
|------|-----|
| `uniq` doesn't remove all duplicates | `sort` first |
| `sort` puts `10` before `2` | use `sort -n` |
| `cut -d' '` gives empty fields on `ls -l` / `ps` / `df` output | squeeze first: `tr -s ' '` |
| `wc -l file` also prints the filename | `wc -l < file` |
| pattern with `*`, `$` or `[ ]` behaves strangely | put the pattern in **quotes** |
| `sed -i` destroyed my file | test without `-i` first, or use `-i.bak` |
| long pipeline over several lines breaks | end each line with `\|`, not with `\   # comment` |

### ✅ Regular Expressions
- Start simple (`^`, `$`, `.`, `[ ]`) and make the pattern more precise step by step
- Test the pattern with `grep` on a small sample file first
- For complex patterns an online regex tester helps you see what matches

---

## Navigation

**Next:** [→ Review Questions](14-review-questions.md)  
**Previous:** [← Practical Labs](12-practical-labs.md)  
**Lesson Home:** [↑ Lesson 7: Text Processing and Filters](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
