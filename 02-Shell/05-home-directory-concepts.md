# 5. Home Directory Concepts

---

## 🏘️ The /home Directory Structure

<div align="center">

![Linux File System Homes](../images/LinuxFileSystemHomes.png)

</div>

### 📁 Every User Gets Their Own Space

```
/home/
├── alice/          👤 Alice's files
├── bob/            👤 Bob's files
├── charlie/        👤 Charlie's files
└── david/          👤 David's files
```

**What you can do in your home directory:**
- ✅ Create files and folders
- ✅ Store personal data
- ✅ Install user-specific programs
- ✅ Customize your environment

---

## 🏠 Your Home Directory

<div align="center">

![Linux File System Home](../images/LinuxFileSystemHome.png)

</div>

### 🔑 Key Concepts

Your home directory (`/home/username`) is:

| Feature | Description | Symbol |
|---------|-------------|--------|
| **Environment Variable** | Referenced by `$HOME` | `$HOME` |
| **Shortcut** | Accessible via tilde | `~` |
| **Default Location** | Where you start when logging in | 🏠 |
| **Full Permissions** | Only place with full write access | ✅ |

### 💡 Quick Access Examples

```bash
# All these go to your home directory:
cd ~
cd $HOME
cd /home/username
cd              # just 'cd' alone!

# Reference files in home:
~/Documents/report.txt
$HOME/.bashrc
```
---

## Navigation

**Next:** [→ File System Permissions](06-file-system-permissions.md)  
**Previous:** [← Linux File System Structure](04-linux-file-system-structure.md)  
**Lesson Home:** [↑ Lesson 2: The Linux Console and Terminal](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
