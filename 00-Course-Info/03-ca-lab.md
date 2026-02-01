# Continuous Assessment Lab: Linux Installation

## 🎯 Lab Objectives

This is a **mandatory continuous assessment lab** that contributes to your final grade. You must complete this lab before the next class session.

**Goals:**

- ✅ Install and configure both Linux distributions
- 💻 Set up working virtual machines (or alternative solutions)
- 🔧 Verify both systems are operational
- 📸 Document your installations

---

## ⚠️ Important Information

<div style="background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

### Assessment Details

- **📊 Weight**: Part of 20% Continuous Assessment (Together with the other CA Labs)
- **⏰ Deadline**: Before the start of next lab session
- **📝 Deliverables**: Screenshots/documentation of both working installations
- **✅ Requirements**: Both AlmaLinux 10.1 AND Ubuntu 24.04 must be running

</div>

---

## 📋 Requirements

You must have **BOTH** of the following Linux distributions installed and running:

### Required Distributions

| Distribution     | Version     | Type    | Purpose                     |
| ---------------- | ----------- | ------- | --------------------------- |
| 🔴 **AlmaLinux** | 10.1        | Minimal | Enterprise Linux experience |
| 🟠 **Ubuntu**    | 24.04.3 LTS | Desktop | User-friendly environment   |

### Installation Options

Choose **ONE** installation method:

1. **💻 Virtual Machines** (Recommended)
   - Both distros in separate VMs
   - Using VirtualBox, VMWare, UTM, QEMU, etc.

2. **🪟 WSL2** (Windows only)
   - Both distros as separate WSL instances
   - Quick and integrated solution

3. **🐧 Native/Dual Boot** (Advanced)
   - One or both as native installations
   - Best performance but more complex

4. **🔄 Hybrid Approach**
   - Mix of methods (e.g., one VM + one WSL)
   - Use what works best for your setup

---

## 📝 Step-by-Step Instructions

### Part 1: AlmaLinux 9.3 Installation

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

#### 1️⃣ Download AlmaLinux

**ISO Location:**

```
https://ftp.belnet.be/mirror/almalinux/10.1/isos/x86_64/AlmaLinux-10-latest-x86_64-minimal.iso
```

**File Details:**

- 📦 Size: ~2GB
- 🔧 Type: Minimal installation (no GUI)
- 🎯 Purpose: Server/command-line practice

#### 2️⃣ Create Virtual Machine

**Recommended VM Settings:**

- 💾 **RAM**: 2GB minimum (4GB recommended)
- 💿 **Disk**: 20GB minimum
- 🖥️ **CPU**: 2 cores
- 🌐 **Network**: NAT or Bridged

**VirtualBox Example:**

1. Click "New"
2. Name: "AlmaLinux-9.3"
3. Type: Linux
4. Version: Red Hat (64-bit)
5. Allocate memory: 2048 MB
6. Create virtual hard disk: 20 GB, VDI, dynamically allocated
7. Settings → Storage → Add ISO to optical drive
8. Start VM

#### 3️⃣ Install AlmaLinux

**Installation Steps:**

1. Boot from ISO
2. Select "Install AlmaLinux 10.1"
3. Choose language
4. **Installation Destination**: Select disk
5. **Network & Hostname**: Enable network, set hostname
6. **Root Password**: Set a strong password (remember it!)
7. **User Creation**: Create a regular user account
8. Begin Installation
9. Wait for completion (~5-10 minutes)
10. Reboot

#### 4️⃣ Verify Installation

After reboot, login and run:

```bash
# Check OS version
cat /etc/os-release

# Check network connectivity
ping -c 4 google.com

# Check disk space
df -h

# Check memory
free -h
```

</div>

---

### Part 2: Ubuntu 24.04 Installation

<div style="background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

#### 1️⃣ Download Ubuntu

**ISO Location:**

```
https://ftp.belnet.be/mirror/ubuntu-releases/24.04.3/ubuntu-24.04.3-desktop-amd64.iso
```

**File Details:**

- 📦 Size: ~5GB
- 🖥️ Type: Desktop (includes GUI)
- 🎯 Purpose: User-friendly Linux experience

#### 2️⃣ Create Virtual Machine

**Recommended VM Settings:**

- 💾 **RAM**: 4GB minimum (8GB recommended)
- 💿 **Disk**: 25GB minimum
- 🖥️ **CPU**: 2 cores
- 🌐 **Network**: NAT or Bridged
- 🎨 **Display**: Enable 3D acceleration (if available)

**VirtualBox Example:**

1. Click "New"
2. Name: "Ubuntu-24.04"
3. Type: Linux
4. Version: Ubuntu (64-bit)
5. Allocate memory: 4096 MB
6. Create virtual hard disk: 25 GB, VDI, dynamically allocated
7. Settings → Display → Video Memory: 128 MB, Enable 3D
8. Settings → Storage → Add ISO to optical drive
9. Start VM

#### 3️⃣ Install Ubuntu

**Installation Steps:**

1. Boot from ISO
2. Select "Install Ubuntu"
3. Choose keyboard layout
4. **Updates and Software**: Normal installation
5. **Installation Type**: Erase disk and install Ubuntu
6. Select timezone
7. Create user account (name, computer name, password)
8. Wait for installation (~10-20 minutes)
9. Restart when prompted
10. Login to your new Ubuntu desktop

#### 4️⃣ Verify Installation

Open Terminal (Ctrl+Alt+T) and run:

```bash
# Check OS version
lsb_release -a

# Check network connectivity
ping -c 4 google.com

# Check disk space
df -h

# Check memory
free -h

# Update system
sudo apt update
```

</div>

---

## 📸 Documentation Requirements

You must submit **proof** that both systems are working. Take screenshots of:

### For AlmaLinux ✅

1. **Login screen** showing successful boot
2. **Terminal showing**:
   ```bash
   cat /etc/os-release
   hostname
   ip addr show
   ```
3. **Successful ping** to external site (e.g., `ping -c 4 google.com`)

### For Ubuntu ✅

1. **Desktop environment** after successful login
2. **Terminal showing**:
   ```bash
   lsb_release -a
   hostname
   ip addr show
   ```
3. **System Settings → About** showing Ubuntu version

### Screenshot Tips 📷

**For VMs:**

- VirtualBox: Menu → View → Take Screenshot
- VMWare: Menu → VM → Capture Screen
- UTM: Built-in screenshot feature
- Or use host OS screenshot tools

**For WSL:**

- Windows + Shift + S for screenshot
- Show terminal with both distros

---

## 📤 Submission Guidelines

<div style="background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

### What to Submit

Create a **single document** (PDF preferred) containing:

1. **📄 Title Page**
   - Your name and student ID
   - Date of submission
   - Lab title: "CA Lab - Linux Installation"

2. **🔴 AlmaLinux Section**
   - Screenshots as specified above
   - Brief description of your setup (VM specs, installation method)
   - Any issues encountered and how you solved them

3. **�� Ubuntu Section**
   - Screenshots as specified above
   - Brief description of your setup
   - Any issues encountered and how you solved them

4. **💭 Reflection** (2-3 sentences)
   - What did you learn?
   - Which installation was easier/harder and why?

### Submission Details

- **📍 Where**: Toledo Ultra → Assignments → "CA Lab: Linux Installation"
- **⏰ Deadline**: Before next lab session (check course schedule)
- **📝 Format**: PDF, DOC, or DOCX
- **📏 Max size**: 10 MB
- **🏷️ Filename**: `LastName_FirstName_CA_Lab_Installation.pdf`

</div>

---

## ✅ Grading Rubric

Your submission will be evaluated on:

| Criteria                  | Points | Description                                   |
| ------------------------- | ------ | --------------------------------------------- |
| **AlmaLinux Working**     | 35%    | Installation successful, screenshots complete |
| **Ubuntu Working**        | 35%    | Installation successful, screenshots complete |
| **Documentation Quality** | 20%    | Clear screenshots, organized submission       |
| **Reflection & Learning** | 10%    | Thoughtful reflection on the process          |

**Total: 100 points** (contributes to your 20% continuous assessment)

---

## 🆘 Getting Help

### If You Encounter Problems

1. **📖 Check the documentation**
   - Review [02-downloads.md](02-downloads.md)
   - Check troubleshooting section

2. **💬 Use Discussion Forums**
   - Post on Toledo Ultra Discussions
   - Include error messages/screenshots
   - Describe what you've already tried

3. **🏢 Office Hours**
   - Visit lecturer office hours
   - Bring your laptop if possible

4. **🤝 Collaborate (but don't copy)**
   - Help each other troubleshoot
   - Share solutions to common problems
   - But submit your own work and screenshots

---

## 💡 Common Issues and Solutions

### Issue: VM is very slow

**Solutions:**

- ✅ Allocate more RAM (if host has enough)
- ✅ Enable VT-x/AMD-V in BIOS
- ✅ Use minimal installation (AlmaLinux)
- ✅ Close unnecessary host applications

### Issue: Can't download ISO files

**Solutions:**

- ✅ Use alternative mirror sites
- ✅ Try downloading on campus network
- ✅ Use torrent downloads (if available)
- ✅ Ask lecturers for USB installation media

### Issue: Installation hangs or freezes

**Solutions:**

- ✅ Verify ISO integrity (check MD5/SHA256)
- ✅ Re-download ISO file
- ✅ Increase VM resources
- ✅ Try different virtualization software

### Issue: Network not working in VM

**Solutions:**

- ✅ Check VM network settings (NAT recommended)
- ✅ Restart VM
- ✅ Check host firewall settings
- ✅ Try bridged networking instead

### Issue: Not enough disk space

**Solutions:**

- ✅ Free up space on host system
- ✅ Use external HDD for VMs
- ✅ Consider WSL2 (uses less space)
- ✅ Use minimal installations

---

## 🎯 Success Checklist

Before submitting, verify:

- [ ] ✅ AlmaLinux 9.3 is installed and boots successfully
- [ ] ✅ Ubuntu 22.04 is installed and boots successfully
- [ ] ✅ Both systems have network connectivity
- [ ] ✅ You can login to both systems
- [ ] ✅ All required screenshots are clear and readable
- [ ] ✅ Document is properly formatted and organized
- [ ] ✅ Your name and student ID are on the document
- [ ] ✅ File is named correctly
- [ ] ✅ Submitted before the deadline

---

## 🚀 Bonus Challenges (Optional)

Want to go further? Try these **optional** extras:

- 🌟 Install Guest Additions/Tools for better VM integration
- 🌟 Set up SSH access to both VMs
- 🌟 Create VM snapshots for backup
- 🌟 Configure shared folders between host and VMs
- 🌟 Install additional software packages
- 🌟 Customize the desktop environment (Ubuntu)

_Note: Bonus challenges are not required but show initiative!_

---

## 📚 Learning Outcomes

By completing this lab, you will:

- ✅ Understand the Linux installation process
- ✅ Gain experience with different Linux distributions
- ✅ Learn virtualization basics
- ✅ Develop troubleshooting skills
- ✅ Build confidence working with Linux systems

---

## Navigation

**Next:** [→ Lesson 1: Introduction](../01-Intro/00-introduction-and-objectives.md)  
**Previous:** [← Downloads](02-downloads.md)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
