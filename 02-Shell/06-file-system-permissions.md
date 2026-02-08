# 6. File System Permissions

---

## 🔒 Security Principles

<table>
<tr>
<td align="center" width="25%">

### 🛡️
**Secure by Design**

Linux is built with security in mind

</td>
<td align="center" width="25%">

### 🏠
**User Isolation**

Users can only modify files in their home

</td>
<td align="center" width="25%">

### 👑
**Administrator Control**

System changes require root

</td>
<td align="center" width="25%">

### 📖
**Read-Only System**

System files are read-only for users

</td>
</tr>
</table>

---

## 🔑 Permission Levels

### Understanding rwx

<table>
<tr>
<th width="33%">📖 Read (r)</th>
<th width="33%">✏️ Write (w)</th>
<th width="33%">▶️ Execute (x)</th>
</tr>
<tr>
<td valign="top">

**For Files:**
- View file contents
- Copy the file

**For Directories:**
- List directory contents
- See files inside

</td>
<td valign="top">

**For Files:**
- Modify file contents
- Delete the file
- Rename the file

**For Directories:**
- Create files inside
- Delete files inside
- Rename files inside

</td>
<td valign="top">

**For Files:**
- Run file as program
- Execute as script

**For Directories:**
- Enter the directory
- Access files inside

</td>
</tr>
</table>

---

## 🎯 Permission Examples

```bash
# File permissions visualization
-rw-r--r--  1 alice users  1024 Feb 8 10:30 report.txt
│││ │ │ │
│││ │ │ └─ Others: read only
│││ │ └─── Group: read only  
│││ └───── Owner: read + write
││└─────── Owner (alice)
│└──────── File type (- = regular file)
└───────── Permissions

drwxr-xr-x  2 bob   users  4096 Feb 8 11:00 documents/
│││ │ │ │
│││ │ │ └─ Others: read + execute (can list)
│││ │ └─── Group: read + execute (can list)
│││ └───── Owner: full access
││└─────── Owner (bob)
│└──────── Directory (d)
└───────── Permissions
```

### 🚦 Permission Summary

| Permission | Files | Directories |
|------------|-------|-------------|
| `r` (read) | 📄 View content | 📋 List contents |
| `w` (write) | ✏️ Modify content | 🗂️ Create/delete files |
| `x` (execute) | ▶️ Run as program | 🚪 Enter directory |
---

## Navigation

**Next:** [→ Essential Navigation Commands](07-essential-navigation-commands.md)  
**Previous:** [← Home Directory Concepts](05-home-directory-concepts.md)  
**Lesson Home:** [↑ Lesson 2: The Shell](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
