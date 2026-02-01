# 7. Best Practices


### Globbing:
- Test glob patterns with `echo` first: `echo *.txt`
- Use quotes to prevent globbing: `ls "*.txt"`
- Enable `nullglob` to handle empty matches: `shopt -s nullglob`

### Archiving:
- Always test archives after creation
- Use appropriate compression for your needs
- Include dates in backup filenames
- Verify archives before deleting originals

### Links:
- Prefer soft links for flexibility
- Use hard links for backup strategies
- Document link purposes in systems
- Check for broken links regularly: `find . -type l ! -exec test -e {} \; -print`

### dd Usage:
- Always double-check device names
- Use `sync` after disk operations
- Test with small data first
- Consider using `pv` for progress monitoring
---

## Navigation

**Next:** [→ Review Questions](08-review-questions.md)  
**Previous:** [← Practical Labs](06-practical-labs.md)  
**Lesson Home:** [↑ Lesson 6: Globbing & Archiving](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
