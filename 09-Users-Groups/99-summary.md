# Summary: Lesson 9 – Users and Groups

**Concepts**
- User types: root (UID 0), system users (1–999), regular users (1000+)
- UID, GID, primary vs secondary groups
- Files: `/etc/passwd` (7 fields), `/etc/shadow` (password hashes), `/etc/group`
- Passwords are stored as a **hash**, never in plain text
- `usermod -aG` adds to a group; `-G` without `-a` **replaces** all secondary groups
- New group membership only works after a new login
- `sudo` (your password, one command) vs `su` (target user's password, switch user)
- New users get a copy of `/etc/skel`; personal settings go in `~/.bashrc`

**Commands**
- Info: `whoami`, `id`, `groups`, `getent`, `who`, `w`, `last`
- Users: `adduser`, `useradd` (`-m`, `-s`, `-c`, `-G`), `usermod` (`-aG`, `-L`, `-U`, `-s`), `userdel` (`-r`), `passwd` (`-l`, `-u`, `-S`)
- Groups: `groupadd`, `groupdel`, `gpasswd -d`
- Privileges: `sudo` (`-i`, `-u`, `-l`), `su -`

**ℹ️ Advanced (optional)**
- `/etc/sudoers` and `/etc/sudoers.d/` with `visudo`
- `chage` (password aging), PAM (`pwquality`, `faillock`)
- `groupmod`, `chsh`, `chfn`, bash startup files

---

## Navigation

**Next:** [→ Learning Objectives](../10-Permissions/00-learning-objectives.md)  
**Previous:** [← Next Lesson](12-next-lesson.md)  
**Lesson Home:** [↑ Lesson 9: Users and Groups](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
