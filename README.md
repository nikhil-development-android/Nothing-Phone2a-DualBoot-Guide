🚀 Nothing Phone (2a) — Nothing OS + Custom ROM Setup Guide
Setup Guide for Running Nothing OS and Custom ROM on the Same Device
Device Codename: pacman
📱 Introduction
Welcome! 👋
This guide explains how to set up Nothing OS + Custom ROM on the Nothing Phone (2a).
With this setup, you can:
✅ Use Stock Nothing OS
✅ Use a Custom ROM / Ported ROM
✅ Switch back to Stock ROM anytime if the Custom ROM has bugs or issues
⚠️ Important Warnings
🛑 Full Data Wipe
This process will completely erase:
Internal storage
Apps
Photos/Videos
Documents
👉 Take a full backup before starting.
⚡ OTA Update Warning
This setup works only on a Vendor-Fastboot base.
❌ Never install OTA updates
OTA updates can:
Break the setup
Cause bootloops
Corrupt partitions
✅ Manual Flash Only
Always flash modified fastboot ROM packages manually.
🛠️ Downloads & Requirements
📂 Required Files
TWRP Recovery
Parted Tool
mkfs.ext4 Utility
Commands.txt
ADB & Fastboot Drivers
📥 Download Required Files
Download:
Parted Tool
mkfs.ext4 Utility
Commands.txt
from the GitHub repository below:
🔗 GitHub Repository:
github.com⁠�
📥 Download TWRP Recovery
Download the latest TWRP recovery image from:
🔗 Releases Page:
github.com⁠�
📥 Download Modified ROM
Download the latest modified ROM package from:
🔗 Releases Page:
github.com⁠�
Download only the Nothing Phone (2a) build.
📢 Telegram Group
Get latest:
ADB & Fastboot Tools
Drivers
ROM Updates
Fixed Files
Recovery Tools
🔗 Telegram Group:
t.me⁠�
🔓 Step 0 — Unlock Bootloader
🔧 Enable Developer Options
Go to:
Settings → About Phone → Build Number
Tap Build Number 7 times.
🔧 Enable OEM Unlocking
Go to:
Settings → System → Developer Options
Enable:
OEM Unlocking
USB Debugging
🔄 Reboot to Fastboot
Bash
adb reboot bootloader
🔓 Unlock Bootloader
Bash
fastboot flashing unlock
✅ Confirm on Phone
Use the Volume Buttons to select:
Yes or Unlock the bootloader
Press the Power Button to confirm.
🛠️ Step 1 — Flash TWRP Recovery
🔄 Reboot to Fastboot
Bash
adb reboot bootloader
📂 Flash TWRP on Both Slots
Bash
fastboot flash vendor_boot_a twrp.img
fastboot flash vendor_boot_b twrp.img
🏗️ Step 2 — Partition Tool Setup
🔄 Boot Into Recovery
Bash
fastboot reboot recovery
❌ Disable MTP
In TWRP:
Mount → Disable MTP
📂 Push Required Tools
Bash
adb push parted /sbin
adb push mkfs.ext4 /sbin
adb shell
chmod 777 /sbin/parted
chmod 777 /sbin/mkfs.ext4
📂 Open Partition Table
Bash
parted /dev/block/sdc
📏 Change Unit to GB
Bash
unit gb
📋 Print Partition List
Bash
print
✂️ Step 3 — Backup & Modify Partitions
💾 Save Partition Info
Copy partition 82 and 83 Start/End values into Notepad.
❌ Remove Partition 82
Bash
rm 82
💾 Backup Partition 83
Exit parted:
Bash
quit
Then run:
Bash
adb shell dd if=/dev/block/sdc83 of=/sdcard/sdc83.img
adb pull /sdcard/sdc83.img
❌ Delete Partition 83
Re-enter parted:
Bash
parted /dev/block/sdc
unit gb
Then:
Bash
rm 83
📐 Step 4 — Create New Partitions
📱 Example Layout (128GB Variant)
Partition
Size
userdata
12.1GB → 69.6GB
userdata_b
69.6GB → 128GB
➕ Create New Partitions
Bash
mkpart userdata ext4 12.1gb 69.6gb
mkpart userdata_b ext4 69.6gb 128gb
🏷️ Rename Partitions
Bash
name 82 userdata
name 83 userdata_b
🚪 Exit Parted
Bash
quit
📂 Format Partitions
Bash
make_f2fs /dev/block/sdc82
make_f2fs /dev/block/sdc83
💾 Step 5 — Restore Partition Backup
📤 Push Backup Image
Bash
adb push sdc83.img /sdcard/
♻️ Restore Backup
Bash
adb shell dd if=/sdcard/sdc83.img of=/dev/block/sdc83
📦 Step 6 — Flash the Modified ROM
🔄 Reboot to Bootloader
Bash
adb reboot bootloader
📂 Flash the Modified ROM
Download the tweaked Fastboot Flashable Stock ROM provided by:
github.com⁠�
This special build is engineered to:
Respect your new partition layout
Properly use userdata_b for Stock OS
Keep the setup stable
🔄 Switching Between ROMs
You can switch between:
Nothing OS
Custom ROM
depending on your setup and boot method.
⚠️ Final Notes
✅ Always keep backups
✅ Never flash OTA updates
✅ Flash only compatible builds
✅ Read instructions carefully before modifying partitions
👨‍💻 Credits & Acknowledgements
🚀 Project Developer & Maintainer
github.com⁠�
❤️ Special Thanks
Thanks to all testers, contributors, and the Android modding community.
