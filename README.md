# Nothing Phone (2a) — Nothing OS + Custom ROM Setup

[![GitHub Release](https://img.shields.io/github/v/release/yourusername/nothing-2a-rom-setup)](https://github.com/yourusername/nothing-2a-rom-setup/releases)
[![Telegram](https://img.shields.io/badge/Telegram-Join-26A5E4?logo=telegram)](https://t.me/yourgroup)

> ⚠️ **For advanced users only. You assume full responsibility.**

---

## 📖 What This Setup Does

This guide helps you:
- **Resize partitions** to create separate `userdata` and `userdata_b`
- **Keep Nothing OS** on one slot with its own data
- **Flash Custom ROM** on the other slot with separate data
- **Switch ROMs** by changing active slot (active-slot-switch app include in rom)

### ⚠️ Important: This is NOT Dual Boot
- ❌ No boot menu to choose at startup
- ✅ One ROM active at a time
- ✅ Other slot's ROM stays installed but inactive

---

## ⚠️ Critical Warnings

### 🛑 Complete Data Wipe
> **Everything will be erased:**
> **👉 Take a full backup before starting!**

### ⚡ No OTA Updates
> **❌ NEVER install OTA updates**
> 
> OTA updates will:
> - Cause bootloops
> - Corrupt slot configuration
> 
> **✅ Always flash modified ROM packages via fastboot Provide by me**

---

## 📋 Requirements

| Item | Description |
|------|-------------|
| Device | Nothing Phone (2a) - Unlock bootloader |
| Computer | Windows / Linux / Mac |
| Cable | USB data cable |
| Files | TWRP, parted, mkfs.ext4, Modified ROM |

---

## 📥 Downloads

| File | Source |
|------|--------|
| **Parted + mkfs.ext4** | [GitHub Releases](https://github.com/yourusername/nothing-2a-rom-setup/releases) |
| **TWRP Recovery** | [TWRP Releases](https://github.com/yourusername/twrp-pacman/releases) |
| **Modified ROM** | [ROM Releases](https://github.com/yourusername/nothing-2a-modified-rom/releases) |
| **ADB/Fastboot + Drivers** | [Telegram Group](https://t.me/yourgroup) |

### 💬 Community Support
🔗 **[Join Telegram Group](https://t.me/yourgroup)** — Latest tools, fixes, and help

---

## 🛠️ Complete Setup Guide

### Step 0 — Unlock Bootloader

# Enable Developer Options

Settings → About Phone → Build Number (tap 7 times)

# Enable in Developer Options
✓ OEM Unlocking
✓ USB Debugging

# Unlock
```bash
adb reboot bootloader
fastboot flashing unlock
```
# Confirm on phone
---

Step 1 — Flash TWRP Recovery

```bash
fastboot flash vendor_boot_a twrp.img
fastboot flash vendor_boot_b twrp.img
```

---

Step 2 — Setup Partition Tools

```bash
fastboot reboot recovery
```
# In TWRP: Mount → Disable MTP

```bash
adb push parted /sbin
adb push mkfs.ext4 /sbin
adb shell chmod 777 /sbin/parted
adb shell chmod 777 /sbin/mkfs.ext4
adb shell parted /dev/block/sdc

# In parted
unit gb
print
```
# 📝 Save partition 82 and 83 start/end values

---

Step 3 — Backup & Remove Partitions

```bash
# Remove partition 82
rm 82

# Exit parted
quit
```
```bash
# Backup partition 83
adb shell dd if=/dev/block/sdc83 of=/sdcard/proinfo.img
adb pull /sdcard/proinfo.img

# Re-enter parted
adb shell parted /dev/block/sdc
unit gb
rm 83
```

---

Step 4 — Create New Partitions

Example for 128GB variant:

Partition Size
userdata 12.1GB → 69.6GB
userdata_b 69.6GB → 128GB

```bash
mkpart userdata ext4 12.1gb 69.6gb
mkpart userdata_b ext4 69.6gb 128gb
name 82 userdata
name 83 userdata_b
quit

# Format
make_f2fs /dev/block/sdc82
make_f2fs /dev/block/sdc83
```

⚠️ Adjust values based on your storage variant (128GB/256GB)

---

Step 5 — Restore Backup

```bash
adb push sdc83.img /sdcard/
adb shell dd if=/sdcard/sdc83.img of=/dev/block/sdc83
```

---

Step 6 — Flash Modified ROM

```bash
adb reboot bootloader
```

Download and flash the tweaked Fastboot ROM from Releases

---

❌ What DOESN'T Work

· ❌ Boot menu selection
· ❌ OTA updates

---

✅ What WORKS

· ✅ Nothing OS with separate data
· ✅ Custom ROM with separate data
· ✅ Stock ROM as backup (keep it on Slot A)

---

📝 Final Notes

Do's ✅

· Keep backups of your working setup
· Join Telegram for updates
· Read everything before starting
· Keep Nothing OS on one slot as fallback

Don'ts ❌

· Never install OTA updates
· Don't flash unverified ROMs in B Slot
· Don't expect dual-boot behavior

---

👨‍💻 Credits

Developer & Maintainer

· Nikhil-Dev

Thanks To

· Testers & TWRP Team
· Android modding community

---

📜 License

MIT License — See LICENSE file

---

⚠️ Disclaimer

This is an unofficial guide.
Modifying your device may void warranty.
You assume all risks.

---

<div align="center">

</div>
```

---
