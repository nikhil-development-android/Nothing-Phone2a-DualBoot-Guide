# Nothing Phone 2a — Dual Rom Setup Guide

This guide helps you create two separate partitions on your Nothing Phone 2a to store different ROMs.

## ⚠️ Important Notes
- **This is NOT dual boot** — Only one ROM runs at a time
- **Bootloader must be unlocked**
- **All data will be deleted** — Backup everything before starting
- **Join our support group:** https://t.me/nikki_chats

## Requirements
- Nothing Phone 2a (with unlocked bootloader)
- Computer (Windows, Linux, or Mac)
- USB cable
- ADB and Fastboot tools installed
- TWRP recovery, parted tool, mkfs.ext4

## Step-by-Step Guide

### Step 1: Unlock Bootloader
Connect your phone and run:
```bash
adb reboot bootloader
fastboot flashing unlock
```

### Step 2: Flash TWRP Recovery (and flash boot images)
Flash TWRP to the vendor_boot slots, then (when ready) flash vendor_boot, boot and init_boot images to both slots A and B. Replace the .img filenames with your actual filenames if they differ.

```bash
# Flash TWRP to vendor_boot slots (temporary recovery)
fastboot flash vendor_boot_a twrp.img
fastboot flash vendor_boot_b twrp.img
fastboot reboot recovery

# After booting to recovery (or when ready to flash your ROM), flash these to both slots:

# Slot A
fastboot flash vendor_boot_a vendor_boot.img
fastboot flash boot_a boot.img
fastboot flash init_boot_a init_boot.img

# Slot B
fastboot flash vendor_boot_b vendor_boot.img
fastboot flash boot_b boot.img
fastboot flash init_boot_b init_boot.img

# Reboot device
fastboot reboot
```

### Step 3: Check Current Partitions
```bash
adb push parted /sbin
adb shell chmod 777 /sbin/parted
adb shell parted /dev/block/sdc
```

In TWRP, run these commands:
```
unit gb
print
```

Note the details of partitions 82 and 83.

### Step 4: Delete Old Partitions
```bash
# Remove partition 82
rm 82

# Backup partition 83 first
adb shell dd if=/dev/block/sdc83 of=/sdcard/proinfo.img
adb pull /sdcard/proinfo.img

# Remove partition 83
adb shell parted /dev/block/sdc
rm 83
quit
```

### Step 5: Create New Partitions
For 128GB storage:
```bash
adb shell parted /dev/block/sdc
mkpart userdata ext4 12.1gb 69.6gb
mkpart userdata_b ext4 69.6gb 128gb
quit

adb shell make_f2fs /dev/block/sdc82
adb shell make_f2fs /dev/block/sdc83
```

### Step 6: Flash Your ROM
```bash
adb reboot bootloader
```

## Tips & Tricks
- **Do NOT install OTA updates** — It will corrupt your setup
- **Always keep backups** of your original partitions
- **Stuck?** Join our Telegram support group: https://t.me/nikki_chats
- Ask questions in the group if something goes wrong

## License
MIT

## Support
For help and questions, join: **https://t.me/nikki_chats**
