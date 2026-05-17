# 🚀 Nothing Phone (2a) Dual Boot Project

> Advanced Dual Boot Setup Guide for Nothing Phone (2a) (pacman)

---

## 📱 Introduction

Welcome! 👋  
This guide explains how to set up Dual Boot on the Nothing Phone (2a) (codename: pacman).

<small>यह गाइड Nothing Phone (2a) में Dual Boot सेटअप करने के लिए है।</small>

With this setup, you can run:

✅ Stock Nothing OS

✅ Custom ROM / Ported ROM

✅ Two systems on one device

<small>एक ही फोन में दो सिस्टम चला सकते हैं।</small>

---

## ⚠️ Important Warnings

### 🛑 Full Data Wipe

This process will completely erase:

- Internal storage
- Apps
- Photos/Videos
- Documents

<small>पूरा फोन wipe हो जाएगा।</small>

👉 **Take a full backup before starting.**

<small>शुरू करने से पहले backup जरूर लें।</small>

---

### ⚡ OTA Update Warning

This setup works only on a Vendor-Fastboot base.

❌ **Never install OTA updates**

OTA updates can:
- Break dual boot
- Cause bootloops
- Corrupt partitions

<small>OTA update dual boot को खराब कर सकता है।</small>

### ✅ Manual Flash Only

Always flash modified fastboot ROM packages manually.

<small>हमेशा modified ROM manually flash करें।</small>

---

## 🛠️ Downloads & Requirements

### 📂 Required Files

- TWRP Recovery
- Parted Tool
- Commands.txt
- ADB & Fastboot Drivers

<small>जरूरी फाइलें डाउनलोड करें।</small>

---

### 📥 Download Required Files

Download:

- TWRP Recovery
- Parted Tool
- Commands.txt

from the GitHub repository below:

🔗 **GitHub Repository:**  
https://github.com/Nikhil-Development-Android

<small>हरे रंग वाले Code बटन पर क्लिक करें।</small>

---

### 📥 Download Dual Boot ROM

Download the latest Dual Boot ROM package from the official release page:

🔗 **Releases Page:**  
https://github.com/Nikhil-Development-Android/releases

<small>केवल Nothing Phone (2a) build डाउनलोड करें।</small>

---

### 📢 Telegram Group

Get latest:

- ADB & Fastboot Tools
- Drivers
- ROM Updates
- Fixed Files
- Recovery Tools

🔗 **Telegram Group:**  
https://t.me/+aUQEu17jvVo3MjBl

<small>लेटेस्ट tools और updates यहां मिलेंगे।</small>

---

## 🔓 Step 0 — Unlock Bootloader

### 🔧 Enable Developer Options

Go to: `Settings → About Phone → Build Number`

Tap Build Number 7 times.

<small>Build Number पर 7 बार टैप करें।</small>

---

### 🔧 Enable OEM Unlocking

Go to: `Settings → System → Developer Options`

Enable:

- OEM Unlocking
- USB Debugging

<small>OEM Unlocking और USB Debugging ON करें।</small>

---

### 🔄 Reboot to Fastboot

```bash
adb reboot bootloader
```

---

🔓 Unlock Bootloader

```bash
fastboot flashing unlock
```

---

✅ Confirm on Phone

Use the Volume Buttons to select: Yes or Unlock the bootloader

Press the Power Button to confirm.

<small>फोन reset हो जाएगा।</small>

---

🛠️ Step 1 — Flash TWRP Recovery

🔄 Reboot to Fastboot

```bash
adb reboot bootloader
```

---

📂 Flash TWRP on Both Slots

```bash
fastboot flash vendor_boot_a twrp.img
fastboot flash vendor_boot_b twrp.img
```

<small>दोनों slots में flash करें।</small>

---

🏗️ Step 2 — Partition Tool Setup

🔄 Boot Into Recovery

```bash
fastboot reboot recovery
```

---

❌ Disable MTP

In TWRP: Mount → Disable MTP

<small>adb push error रोकने के लिए।</small>

---

📂 Push Required Tools

```bash
adb push parted /sbin
adb push mkfs.ext4 /sbin
adb shell
chmod 777 /sbin/parted
chmod 777 /sbin/mkfs.ext4
```

---

📂 Open Partition Table

```bash
parted /dev/block/sdc
```

---

📏 Change Unit to GB

```bash
unit gb
```

---

📋 Print Partition List

```bash
print
```

<small>GB unit calculations आसान बनाती है।</small>

---

✂️ Step 3 — Backup & Modify Partitions

💾 Save Partition Info

Copy partition 82 and 83 Start/End values into Notepad.

<small>Partition details सेव करें।</small>

---

❌ Remove Partition 82

```bash
rm 82
```

---

💾 Backup Partition 83

Exit parted:

```bash
quit
```

Then run:

```bash
adb shell dd if=/dev/block/sdc83 of=/sdcard/sdc83.img
adb pull /sdcard/sdc83.img
```

<small>Backup skip मत करें।</small>

---

❌ Delete Partition 83

Re-enter parted:

```bash
parted /dev/block/sdc
unit gb
```

Then:

```bash
rm 83
```

---

📐 Step 4 — Create New Partitions

📱 Example Layout (128GB Variant)

Partition Size
userdata 12.1GB → 69.6GB
userdata_b 69.6GB → 128GB

---

➕ Create New Partitions

```bash
mkpart userdata ext4 12.1gb 69.6gb
mkpart userdata_b ext4 69.6gb 128gb
```

<small>256GB model में values अलग होंगी।</small>

---

🏷️ Rename Partitions

```bash
name 82 userdata
name 83 userdata_b
```

---

🚪 Exit Parted

```bash
quit
```

---

📂 Format Partitions

```bash
make_f2fs /dev/block/sdc82
make_f2fs /dev/block/sdc83
```

<small>F2FS Android performance बेहतर बनाता है।</small>

---

💾 Step 5 — Restore Partition Backup

📤 Push Backup Image

```bash
adb push sdc83.img /sdcard/
```

---

♻️ Restore Backup

```bash
adb shell dd if=/sdcard/sdc83.img of=/dev/block/sdc83
```

<small>Restore पूरा होने तक इंतजार करें।</small>

---

📦 Step 6 — Flash the Dual Boot ROM

🔄 Reboot to Bootloader

```bash
adb reboot bootloader
```

---

📂 Flash the Modified ROM

Download the tweaked Fastboot Flashable Stock ROM provided by me
(Nikhil-Development).

<small>मेरे द्वारा दी गई modified ROM flash करें।</small>

---

This special build is engineered to:

· Respect your new partition layout
· Properly use userdata_b for Stock OS
· Keep dual boot stable

<small>यह build dual boot stability के लिए बनाई गई है।</small>

---

## 👨‍💻 Credits & Acknowledgements

* **Project Developer & Maintainer:** [Nikhil-Development-Android](https://github.com/Nikhil-Development-Android) 🚀
* **TWRP Recovery Source Tree:** Huge thanks to [Sidharthify](https://github.com/sidharthify) for the Nothing Phone (2a) [pacman] device tree.
* 
