# Review Questions

1. What does the permission `755` mean? Write it as letters (`rwx...`).
2. A file has `-rw-r-----` and owner `alice`, group `developers`. Can `bob` (member of `developers`) read it? Can he change it? And `carol` (not in the group)?
3. You have `w` and `x` on a directory, but the file inside is read-only (`444`). Can you delete the file? Why?
4. What's the difference between `chmod 644 file` and `chmod u+rw,g+r,o+r file`?
5. ℹ️ *Advanced:* How do you make a directory where all new files automatically get the directory's group?
6. ℹ️ *Advanced:* What does the sticky bit do and where is it used?
7. Your umask is `022`. Which permissions does a new file get, and a new directory?
8. How do you find all files larger than 100 MB in your home directory?

---

## Navigation

**Next:** [→ Key Takeaways](11-key-takeaways.md)  
**Previous:** [← Security Best Practices](09-security-best-practices.md)  
**Lesson Home:** [↑ Lesson 10: File Permissions and Tools](../)
**Course Home:** [⌂ Introduction to Linux](../README.md)
