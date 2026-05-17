🚀 Nothing Phone (2a) Dual Boot Project

📱 Introduction

Welcome! 👋
This guide explains how to set up Dual Boot on the Nothing Phone (2a) (codename: pacman).

> यह गाइड Nothing Phone (2a) में Dual Boot सेटअप करने के लिए है।



With this setup, you can run:

✅ Stock Nothing OS

✅ Custom ROM / Ported ROM

✅ Two systems on one device


> एक ही फोन में दो सिस्टम चला सकते हैं।




---

⚠️ Important Warnings

🛑 Full Data Wipe

This process will completely erase:

Internal storage

Apps

Photos/Videos

Documents


> पूरा फोन wipe हो जाएगा।



👉 Take a full backup before starting.

> शुरू करने से पहले backup जरूर लें।




---

⚡ OTA Update Warning

This setup works only on a Vendor-Fastboot base.

❌ Never install OTA updates

OTA updates can:

Break dual boot

Cause bootloops

Corrupt partitions


> OTA update dual boot को खराब कर सकता है।



✅ Manual Flash Only

Always flash modified fastboot ROM packages manually.

> हमेशा modified ROM manually flash करें।




---

🛠️ Downloads & Requirements

📂 Required Files

TWRP Recovery

Parted Tool

Commands.txt

ADB & Fastboot Drivers


> जरूरी फाइलें डाउनलोड करें।




---

📥 Download Required Files

Download:

TWRP Recovery

Parted Tool

Commands.txt


from the GitHub repository below:

🔗 GitHub Repository:
https://github.com/Nikhil-Development-Android

> हरे रंग वाले Code बटन पर क्लिक करें।




---

📥 Download Dual Boot ROM

Download the latest Dual Boot ROM package from the official release page:

🔗 Releases Page:
https://github.com/Nikhil-Development-Android/releases

> केवल Nothing Phone (2a) build डाउनलोड करें।




---

📢 Telegram Group

Get latest:

ADB & Fastboot Tools

Drivers

ROM Updates

Fixed Files

Recovery Tools


🔗 Telegram Group:
https://t.me/+aUQEu17jvVo3MjBl

> लेटेस्ट tools और updates यहां मिलेंगे।




---

🔓 Step 0 — Unlock Bootloader

🔧 Enable Developer Options

Go to: Settings → About Phone → Build Number

Tap Build Number 7 times.

> Build Number पर 7 बार टैप करें।




---

🔧 Enable OEM Unlocking

Go to: Settings → System → Developer Options

Enable:

OEM Unlocking

USB Debugging


> OEM Unlocking और USB Debugging ON करें।




---

🔄 Reboot to Fastboot

adb reboot bootloader


---

🔓 Unlock Bootloader

fastboot flashing unlock


---

✅ Confirm on Phone

Use the Volume Buttons to select: Yes or Unlock the bootloader

Press the Power Button to confirm.

> फोन reset हो जाएगा।




---

🛠️ Step 1 — Flash TWRP Recovery

🔄 Reboot to Fastboot

adb reboot bootloader


---

📂 Flash TWRP on Both Slots

fastboot flash vendor_boot_a twrp.img
fastboot flash vendor_boot_b twrp.img

> दोनों slots में flash करें।




---

🏗️ Step 2 — Partition Tool Setup

🔄 Boot Into Recovery

fastboot reboot recovery


---

❌ Disable MTP

In TWRP: Mount → Disable MTP

> adb push error रोकने के लिए।




---

📂 Push Required Tools

adb push parted /sbin
adb push mkfs.ext4 /sbin
adb shell
chmod 777 /sbin/parted
chmod 777 /sbin/mkfs.ext4


---

📂 Open Partition Table

parted /dev/block/sdc


---

📏 Change Unit to GB

unit gb


---

📋 Print Partition List

print

> GB unit calculations आसान बनाती है।




---

✂️ Step 3 — Backup & Modify Partitions

💾 Save Partition Info

Copy partition 82 and 83 Start/End values into Notepad.

> Partition details सेव करें।




---

❌ Remove Partition 82

rm 82


---

💾 Backup Partition 83

Exit parted:

quit

Then run:

adb shell dd if=/dev/block/sdc83 of=/sdcard/sdc83.img
adb pull /sdcard/sdc83.img

> Backup skip मत करें।




---

❌ Delete Partition 83

Re-enter parted:

parted /dev/block/sdc
unit gb

Then:

rm 83


---

📐 Step 4 — Create New Partitions

📱 Example Layout (128GB Variant)

Partition	Size

userdata	12.1GB → 69.6GB
userdata_b	69.6GB → 128GB



---

➕ Create New Partitions

mkpart userdata ext4 12.1gb 69.6gb
mkpart userdata_b ext4 69.6gb 128gb

> 256GB model में values अलग होंगी।




---

🏷️ Rename Partitions

name 82 userdata
name 83 userdata_b


---

🚪 Exit Parted

quit


---

📂 Format Partitions

make_f2fs /dev/block/sdc82
make_f2fs /dev/block/sdc83

> F2FS Android performance बेहतर बनाता है।




---

💾 Step 5 — Restore Partition Backup

📤 Push Backup Image

adb push sdc83.img /sdcard/


---

♻️ Restore Backup

adb shell dd if=/sdcard/sdc83.img of=/dev/block/sdc83

> Restore पूरा होने तक इंतजार करें।




---

📦 Step 6 — Flash the Dual Boot ROM

🔄 Reboot to Bootloader

adb reboot bootloader


---

📂 Flash the Modified ROM

Download the tweaked Fastboot Flashable Stock ROM provided by me
(Nikhil-Development).

> मेरे द्वारा दी गई modified ROM flash करें।




---

This special build is engineered to:

Respect your new partition layout

Properly use userdata_b for Stock OS

Keep dual boot stable


> यह build dual boot stability के लिए बनाई गई है।




---

👨‍💻 Credits

Created & Maintained By

Nikhil-Development-Android 🚀
