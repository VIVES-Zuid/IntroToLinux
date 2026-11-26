# 8. Essential File Commands

---

## 📂 File and Directory Operations

### 🔍 Viewing and Navigation

<table>
<tr>
<th width="30%">Command</th>
<th width="40%">Purpose</th>
<th width="30%">Example</th>
</tr>
<tr>
<td><code>ls</code></td>
<td>📋 List directory contents</td>
<td><code>ls</code></td>
</tr>
<tr>
<td><code>ls -l</code></td>
<td>📊 Long format with details</td>
<td><code>ls -l</code></td>
</tr>
<tr>
<td><code>ls -la</code></td>
<td>👁️ Include hidden files</td>
<td><code>ls -la</code></td>
</tr>
<tr>
<td><code>ls -lh</code></td>
<td>📏 Human readable sizes</td>
<td><code>ls -lh</code></td>
</tr>
<tr>
<td><code>pwd</code></td>
<td>📍 Print working directory</td>
<td><code>pwd</code></td>
</tr>
<tr>
<td><code>cd</code></td>
<td>🚶 Change directory</td>
<td><code>cd /etc</code></td>
</tr>
<tr>
<td><code>cd ..</code></td>
<td>⬆️ Go up one level</td>
<td><code>cd ..</code></td>
</tr>
<tr>
<td><code>cd -</code></td>
<td>↩️ Go to previous directory</td>
<td><code>cd -</code></td>
</tr>
</table>

---

## ✨ Creating Files and Directories

```bash
# 📄 Create files
touch filename.txt
touch file1.txt file2.txt file3.txt

# 📁 Create directories
mkdir dirname
mkdir project

# 🗂️ Create nested directories
mkdir -p path/to/nested/directories
mkdir -p project/src/main/java
```

---

## 📋 Copying and Moving

```bash
# 📄➡️📄 Copy files
cp file1.txt file2.txt
cp report.txt backup.txt

# 📁➡️📁 Copy directories (recursive)
cp -r directory1 directory2
cp -r project project-backup

# 🔄 Move/rename files
mv oldname.txt newname.txt
mv report.txt final-report.txt

# 📦 Move to different location
mv file.txt /path/to/destination/
mv document.txt ~/Documents/
```

---

## 🗑️ Removing Files and Directories

```bash
# ❌ Remove files
rm filename.txt

# ❓ Interactive removal (asks before deleting)
rm -i filename.txt

# 📁❌ Remove empty directories
rmdir empty_directory

# 🗂️❌ Remove directories with contents
rm -r directory_with_contents

# ⚠️ Force remove (be careful!)
rm -rf directory
```

> ⚠️ **Warning:** `rm -rf` is permanent! There's no recycle bin in Linux!

---

## 📖 Viewing File Contents

<table>
<tr>
<th width="30%">Command</th>
<th width="40%">Purpose</th>
<th width="30%">Best For</th>
</tr>
<tr>
<td><code>cat filename.txt</code></td>
<td>📄 Display entire file</td>
<td>Small files</td>
</tr>
<tr>
<td><code>less filename.txt</code></td>
<td>📖 View with paging</td>
<td>Large files</td>
</tr>
<tr>
<td><code>more filename.txt</code></td>
<td>📑 View with paging (older)</td>
<td>Legacy systems</td>
</tr>
<tr>
<td><code>head filename.txt</code></td>
<td>⬆️ First 10 lines</td>
<td>Quick preview</td>
</tr>
<tr>
<td><code>tail filename.txt</code></td>
<td>⬇️ Last 10 lines</td>
<td>Log files</td>
</tr>
<tr>
<td><code>head -20 file.txt</code></td>
<td>⬆️ First 20 lines</td>
<td>Custom preview</td>
</tr>
<tr>
<td><code>tail -20 file.txt</code></td>
<td>⬇️ Last 20 lines</td>
<td>Recent logs</td>
</tr>
</table>

### 💡 Pro Tip: Navigation in `less`

```
Key          Action
───────────────────────────────
Space        ⬇️ Next page
b            ⬆️ Previous page
/pattern     🔍 Search forward
?pattern     🔎 Search backward
q            🚪 Quit
```

---

## ✏️ Editing Files

```bash
# 📝 Simple text editor (beginner-friendly)
nano filename.txt

# 💾 Save in nano: Ctrl+O, then Enter
# 🚪 Exit nano: Ctrl+X
```

---

## 🌐 Downloading Files

```bash
# ⬇️ Download files from internet
wget https://example.com/file.txt

# 📥 Download with custom name
wget -O newname.txt https://example.com/file.txt

# 📊 Download with progress bar
wget --progress=bar https://example.com/largefile.zip
```
---

## Navigation

**Next:** [→ Getting Help With Commands](09-getting-help-with-commands.md)  
**Previous:** [← Essential Navigation Commands](07-essential-navigation-commands.md)  
**Lesson Home:** [↑ Lesson 2: The Linux Console and Terminal](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
