# Summary: Lesson 10 – File Permissions and Tools

**Concepts**
- Permission string: file type + user/group/others `rwx`
- Linux uses the **first** matching category: owner → group → others
- Effect of `rwx` on files vs directories (deleting a file depends on the **directory**)
- Symbolic (`u+x`, `g-w`, `o=r`) and numeric (`755`, `644`) notation; r=4, w=2, x=1
- Ownership: user & group, changing the owner needs `sudo`
- Default permissions with umask (files from 666, directories from 777)
- Principle of least privilege

**Commands**
- `ls -l`, `ls -ld`, `chmod` (`-R`), `chown` (`user:group`), `chgrp`
- `umask` (`-S`)
- `find` (`-name`, `-iname`, `-type`, `-size`, `-mtime`, `-mmin`, `-user`, `-group`, `-empty`)
- `locate`, `updatedb`
- `date` (`+%F`), `cal`, `sleep`, `time`

**ℹ️ Advanced (optional)**
- Special bits: SUID (4000), SGID (2000), sticky bit (1000); `s`/`t` vs `S`/`T`
- `find -perm`, `-exec`, `-ok`, `-delete`, `-printf`
- `chmod X`, `date -d`, a security audit script

---

## Navigation

**Next:** [→ Learning Objectives](../11-Server-Management/00-learning-objectives.md)  
**Previous:** [← Next Lesson](12-next-lesson.md)  
**Lesson Home:** [↑ Lesson 10: File Permissions and Tools](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
