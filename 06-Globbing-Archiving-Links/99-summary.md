# Summary: Lesson 6 – Globbing, Archiving & Links

**Concepts**
- Globbing: `*`, `?`, `[abc]`, `[a-z]`, `[!x]`, `{a,b}`
- Extended/recursive globbing (`shopt -s extglob`, `globstar` → `**`)
- Block devices & partitions (`/dev/sda`, `/dev/nvme0n1`, …)
- Archiving vs compression
- Hard links vs soft (symbolic) links
- Inodes: metadata, link count, filename ↔ inode

**Commands**
- Blocks: `dd` (`if`, `of`, `bs`, `count`), `lsblk`, `sync`
- Archiving: `tar` (`-c`, `-x`, `-t`, `-v`, `-f`, `-z`, `-j`, `-J`, `-C`)
- Compression: `gzip`/`gunzip`, `bzip2`/`bunzip2`, `xz`, `zip`/`unzip`
- Links: `ln`, `ln -s`, `readlink`
- Inodes: `ls -i`, `stat`, `df -i`, `find -inum`
- `file`, `shopt`

---

## Navigation

**Next:** [→ Learning Objectives](../07-Filters-Pipelines/00-learning-objectives.md)  
**Previous:** [← Next Lesson](10-next-lesson.md)  
**Lesson Home:** [↑ Lesson 6: Globbing & Archiving](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
