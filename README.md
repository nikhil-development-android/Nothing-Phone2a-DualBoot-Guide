# 🚀 Nothing Phone (2a) — Nothing OS + Custom ROM Dual Boot Setup

**Device Codename:** `pacman`

[![GitHub Release](https://img.shields.io/github/v/release/yourusername/nothing-2a-dual-rom)](https://github.com/yourusername/nothing-2a-dual-rom/releases)
[![Telegram](https://img.shields.io/badge/Telegram-Join-26A5E4?logo=telegram)](https://t.me/yourgroup)
[![XDA](https://img.shields.io/badge/XDA-Forum-FC6B26?logo=xda-developers)](https://forum.xda-developers.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> ⚠️ **Warning:** This guide is for advanced users. Proceed at your own risk. You assume full responsibility for any damage to your device.

---

## 📖 Introduction

This guide enables you to run **Nothing OS** and a **Custom ROM** simultaneously on the Nothing Phone (2a).

### ✨ What You Get

| Feature | Status |
|---------|--------|
| Use Stock Nothing OS | ✅ |
| Use Custom/Ported ROM | ✅ |
| Switch between ROMs | ✅ |
| Fallback to Stock if bugs occur | ✅ |

---

## ⚠️ Critical Warnings

### 🛑 Data Loss Warning
> **This process will COMPLETELY WIPE your device including:**
> - Internal storage
> - Apps & app data
> - Photos, videos, documents
> 
> **👉 Take a full backup before starting!**

### ⚡ OTA Update Warning
> **❌ NEVER install OTA updates with this setup**
> 
> OTA updates can:
> - Break the dual-boot setup
> - Cause bootloops
> - Corrupt partition tables
> 
> **✅ Always flash modified fastboot ROM packages manually.**

---

## 📋 Requirements

### Prerequisites
- Nothing Phone (2a) with unlockable bootloader
- Windows/Linux/Mac computer
- USB Cable (data transfer capable)
- Basic knowledge of ADB/Fastboot commands

### 📂 Required Files

| File | Purpose |
|------|---------|
| `twrp.img` | Custom Recovery |
| `parted` | Partition tool |
| `mkfs.ext4` | Filesystem utility |
| `modified_rom.zip` | Custom ROM package |
| `commands.txt` | Command reference |

---

## 📥 Downloads

### 🔗 Official Sources

| Resource | Link |
|----------|------|
| **Parted & mkfs.ext4** | [GitHub Releases](https://github.com/yourusername/nothing-2a-dual-rom/releases) |
| **TWRP Recovery** | [TWRP Releases](https://github.com/yourusername/twrp-pacman/releases) |
| **Modified ROM** | [ROM Releases](https://github.com/yourusername/nothing-2a-modified-rom/releases) |
| **ADB & Fastboot Tools** | [Telegram Group](https://t.me/yourgroup) |

### 💬 Community Support
Join our **Telegram Group** for:
- Latest ADB/Fastboot tools
- Driver updates
- ROM updates & fixes
- Recovery tools
- Community support

🔗 **[Join Telegram Group](https://t.me/yourgroup)**

---

## 🛠️ Setup Guide

### Step 0 — Unlock Bootloader

#### Enable Developer Options
```bash
Settings → About Phone → Build Number (Tap 7 times)
```

Enable Required Options

```bash
Settings → System → Developer Options
# Enable:
✓ OEM Unlocking
✓ USB Debugging
```

Unlock Bootloader

```bash
adb reboot bootloader
fastboot flashing unlock
# Confirm on phone using volume buttons
```

---

Step 1 — Flash TWRP Recovery

```bash
adb reboot bootloader
fastboot flash vendor_boot_a twrp.img
fastboot flash vendor_boot_b twrp.img
```

---

Step 2 — Partition Tool Setup

```bash
fastboot reboot recovery
```

In TWRP: Mount → Disable MTP

```bash
adb push parted /sbin
adb push mkfs.ext4 /sbin
adb shell chmod 777 /sbin/parted
adb shell chmod 777 /sbin/mkfs.ext4
adb shell parted /dev/block/sdc
```

In parted:

```bash
unit gb
print
# Save partition 82 & 83 start/end values
```

---

Step 3 — Backup & Modify Partitions

```bash
# Remove partition 82
rm 82

# Exit parted
quit

# Backup partition 83
adb shell dd if=/dev/block/sdc83 of=/sdcard/sdc83.img
adb pull /sdcard/sdc83.img

# Re-enter parted
adb shell parted /dev/block/sdc
unit gb
rm 83
```

---

Step 4 — Create New Partitions

Example Layout (128GB variant):

Partition Size Range
userdata 12.1GB → 69.6GB
userdata_b 69.6GB → 128GB

```bash
mkpart userdata ext4 12.1gb 69.6gb
mkpart userdata_b ext4 69.6gb 128gb
name 82 userdata
name 83 userdata_b
quit

# Format partitions
make_f2fs /dev/block/sdc82
make_f2fs /dev/block/sdc83
```

---

Step 5 — Restore Partition Backup

```bash
adb push sdc83.img /sdcard/
adb shell dd if=/sdcard/sdc83.img of=/dev/block/sdc83
```

---

Step 6 — Flash Modified ROM

```bash
adb reboot bootloader
```

Download and flash the tweaked Fastboot Flashable Stock ROM from Releases.

This special build respects your new partition layout and properly uses userdata_b for Stock OS.

---

🔄 Switching Between ROMs

To Boot Method
Nothing OS Normal boot
Custom ROM Depends on your setup

---

❓ Troubleshooting

Common Issues

Issue Solution
Bootloop after OTA Re-flash modified ROM from fastboot
Can't boot to recovery Re-flash TWRP to both slots
Partition errors Restore from backup and restart process
USB detection issues Reinstall drivers from Telegram group

---

📝 Final Notes

✅ Do's

· Keep backups of your working setup
· Read instructions carefully before each step
· Join Telegram group for updates
· Use only compatible builds

❌ Don'ts

· Never install OTA updates
· Don't skip backup steps
· Don't modify partitions without understanding
· Don't flash unverified ROMs

---

👨‍💻 Credits & Acknowledgements

Project Developer & Maintainer

Your Name / Username
https://img.shields.io/badge/GitHub-Follow-181717?logo=github
https://img.shields.io/badge/XDA-Developer-FC6B26?logo=xda-developers

Special Thanks

· All beta testers and contributors
· Nothing Technology Limited
· Android Open Source Project
· TWRP Team
· The Android modding community

---

📜 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

⭐ Support the Project

If this guide helped you:

· ⭐ Star this repository
· 🔔 Watch for updates
· 📢 Share with others
· 💬 Join our Telegram community

---

📞 Contact & Links

Platform Link
GitHub github.com/yourusername
Telegram t.me/yourgroup
XDA Thread XDA Forums

---

⚠️ DISCLAIMER: This is an unofficial guide. Nothing Technology Limited is not affiliated with or responsible for this project. Modifying your device may void your warranty. Proceed at your own risk.

---

<div align="center">

Made with ❤️ for the Android community

</div>
```
