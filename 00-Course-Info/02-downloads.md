# Downloads and Software Requirements

## 📥 Required Software Downloads

This page contains all the software and tools you'll need for the Introduction to Linux course.

---

## 🐧 Linux Distribution (Install Both!)

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

### Distro 1: [AlmaLinux](https://ftp.belnet.be/mirror/almalinux/) 10.3

**Download Link:**

```
https://ftp.belnet.be/mirror/almalinux/10.3/isos/x86_64/AlmaLinux-10-latest-x86_64-minimal.iso
```

or [Alma Official](https://repo.almalinux.org/almalinux/) site:

```
https://repo.almalinux.org/almalinux/10.3/isos/x86_64/AlmaLinux-10-latest-x86_64-minimal.iso
```

**Why AlmaLinux?**

- 🏢 Enterprise-grade Linux distribution
- 🔒 Stable and secure
- 📚 Similar to Red Hat Enterprise Linux
- �� Widely used in professional environments

</div>

<div style="background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

### Distro 2: [Ubuntu](https://ftp.belnet.be/mirror/ubuntu-releases/) 24.04.5.1 LTS

**Download Link:**

```
https://ftp.belnet.be/mirror/ubuntu-releases/24.04.3/ubuntu-24.04.3-desktop-amd64.iso
```

or [Ubuntu](https://releases.ubuntu.com) Official site:

````
https://releases.ubuntu.com/24.04.5.1/ubuntu-24.04.4-desktop-amd64.iso

**Why Ubuntu?**

- 🎯 User-friendly for beginners
- 🌍 Large community support
- 📦 Extensive package repository
- 🖥️ Desktop environment included

</div>

---

## 💻 Virtualization Software

Choose the appropriate virtualization software based on your host operating system:

### For macOS Users 🍎

| Software          | Compatibility | License | Download Link                                                                        |
| ----------------- | ------------- | ------- | ------------------------------------------------------------------------------------ |
| **UTM**           | M1/M2/Intel   | Free    | [https://mac.getutm.app](https://mac.getutm.app)                                     |
| **QEMU**          | M1/M2/Intel   | Free    | [https://www.qemu.org/](https://www.qemu.org/)                                       |
| **VirtualBox**    | Intel only    | Free    | [https://www.virtualbox.org/](https://www.virtualbox.org/)                           |
| **VMWare Fusion** | Both          | Paid    | [https://www.vmware.com/go/downloadplayer](https://www.vmware.com/go/downloadplayer) |

**Recommended:**

- 🍎 **Apple Silicon (M1/M2/M3)**: UTM or QEMU
- ⚡ **Intel Macs**: VirtualBox or UTM

### For Windows Users 🪟

| Software                      | Type      | License | Download Link                                                                        |
| ----------------------------- | --------- | ------- | ------------------------------------------------------------------------------------ |
| **WSL2**                      | Subsystem | Free    | Built into Windows 10/11                                                             |
| **VirtualBox**                | VM        | Free    | [https://www.virtualbox.org/](https://www.virtualbox.org/)                           |
| **VMWare Workstation Player** | VM        | Free    | [https://www.vmware.com/go/downloadplayer](https://www.vmware.com/go/downloadplayer) |
| **QEMU**                      | VM        | Free    | [https://www.qemu.org/](https://www.qemu.org/)                                       |

**Recommended:**

- 🚀 **Easiest**: WSL2 (Windows Subsystem for Linux)
- 💻 **Full VM**: VirtualBox or VMWare

### For Linux Users 🐧

| Software       | Type           | Installation                                               |
| -------------- | -------------- | ---------------------------------------------------------- |
| **Native**     | Direct install | Recommended - best performance                             |
| **QEMU/KVM**   | VM             | `apt install qemu-kvm` or `dnf install qemu-kvm`           |
| **VirtualBox** | VM             | [https://www.virtualbox.org/](https://www.virtualbox.org/) |

**Recommended:**

- 🏆 **Best option**: Dual boot or dedicated Linux machine
- 🔧 **Alternative**: QEMU/KVM for virtualization

---

## 🔧 Installation Guides

### Installing WSL2 on Windows

**Quick Installation (Windows 10 version 2004+ or Windows 11):**

```powershell
wsl --install
````

**Manual Installation:**

1. Open PowerShell as Administrator
2. Enable WSL:
   ```powershell
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   ```
3. Enable Virtual Machine Platform:
   ```powershell
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```
4. Restart your computer
5. Download and install the WSL2 Linux kernel update
6. Set WSL2 as default:
   ```powershell
   wsl --set-default-version 2
   ```

**Or from Microsoft Store:**

- [Windows Subsystem for Linux](https://apps.microsoft.com/store/detail/windows-subsystem-for-linux/9P9TQF7MRM4R)

### Installing VirtualBox

1. Download VirtualBox for your OS
2. Run the installer
3. Follow the installation wizard
4. Install the Extension Pack (optional but recommended)

### Installing UTM (macOS)

1. Visit [https://mac.getutm.app](https://mac.getutm.app)
2. Download UTM
3. Open the DMG file
4. Drag UTM to Applications folder
5. Launch UTM and create a new virtual machine

---

## 📋 Installation Checklist

Before starting the course, ensure you have:

- [ ] 📥 Downloaded Linux ISO (AlmaLinux or Ubuntu)
- [ ] 💻 Installed virtualization software (if not using native Linux)
- [ ] ✅ Created and tested a virtual machine
- [ ] 🔧 Verified you can boot into Linux
- [ ] 🌐 Confirmed network connectivity in VM
- [ ] 📝 Taken a snapshot of fresh installation (recommended)

---

## 🆘 Troubleshooting

### Common Issues

**VirtualBox won't start VM:**

- ✅ Enable virtualization (VT-x/AMD-V) in BIOS
- ✅ Disable Hyper-V on Windows
- ✅ Check if another hypervisor is running

**UTM slow performance:**

- ✅ Allocate more RAM to VM (4GB minimum recommended)
- ✅ Enable hardware acceleration
- ✅ Use QEMU backend instead of Apple Virtualization

**WSL2 installation fails:**

- ✅ Update Windows to latest version
- ✅ Enable virtualization in BIOS
- ✅ Run PowerShell as Administrator

**Network issues in VM:**

- ✅ Check VM network settings (NAT or Bridged)
- ✅ Verify host has internet connection
- ✅ Restart the VM

---

## 💡 Additional Resources

### Optional Tools

- **📝 Text Editor**: Visual Studio Code ([https://code.visualstudio.com/](https://code.visualstudio.com/))
- **🔧 SSH Client**: Built into most systems, or use PuTTY for Windows
- **📊 Terminal**: Windows Terminal for Windows users ([https://apps.microsoft.com/store/detail/windows-terminal/](https://apps.microsoft.com/store/detail/windows-terminal/))

### Documentation Links

- **AlmaLinux Docs**: [https://wiki.almalinux.org/](https://wiki.almalinux.org/)
- **Ubuntu Docs**: [https://help.ubuntu.com/](https://help.ubuntu.com/)
- **WSL Documentation**: [https://docs.microsoft.com/en-us/windows/wsl/](https://docs.microsoft.com/en-us/windows/wsl/)

---

## ⚠️ Important Notes

> **💾 Backup Reminder:** Always create snapshots of your VM before major changes!

> **⏰ Install Early:** Don't wait until the last minute. Install and test your setup well before the first class.

> **❓ Need Help?** If you encounter issues during installation, ask on the Toledo discussion forum or contact your lecturers.

---

## Navigation

**Next:** [→ CA-Lab](03-ca-lab.md)  
**Previous:** [← Course Agreements](01-course-agreements.md)  
**Course Info Home:** [↑ Course Information](../)  
**Course Home:** [⌂ Introduction to Linux](../README.md)
