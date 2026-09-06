# Nothing Phone 2a — Dual Slot Setup

Nothing Phone 2a mein two partitions create karke alag-alag ROM rakh sakte ho.

## ⚠️ Important
- **Ye dual boot NAHI hai** — ek time mein ek hi ROM chalega
- **Bootloader unlock karna padega**
- **Sab data delete ho jayega** — backup le lo pehle

## Kya Chahiye
- Nothing Phone 2a (bootloader unlocked)
- Computer (Windows/Linux/Mac)
- USB cable
- TWRP, parted, mkfs.ext4

## Steps

### 1. Bootloader Unlock
```bash
adb reboot bootloader
fastboot flashing unlock
```

### 2. TWRP Flash Karo
```bash
fastboot flash vendor_boot_a twrp.img
fastboot flash vendor_boot_b twrp.img
fastboot reboot recovery
```

### 3. Partition Setup
```bash
adb push parted /sbin
adb shell chmod 777 /sbin/parted
adb shell parted /dev/block/sdc
```

TWRP mein ye commands chalao:
```
unit gb
print
```

Partition 82 aur 83 ki details note kar lo.

### 4. Partitions Delete Karo
```bash
# Remove partition 82
rm 82

# Backup partition 83
adb shell dd if=/dev/block/sdc83 of=/sdcard/proinfo.img
adb pull /sdcard/proinfo.img

# Remove partition 83
adb shell parted /dev/block/sdc
rm 83
```

### 5. Naye Partitions Banao
128GB ke liye:
```bash
mkpart userdata ext4 12.1gb 69.6gb
mkpart userdata_b ext4 69.6gb 128gb
quit

make_f2fs /dev/block/sdc82
make_f2fs /dev/block/sdc83
```

### 6. ROM Flash Karo
```bash
adb reboot bootloader
# Flash modified ROM via fastboot
```

## Tips
- **OTA updates mat laga** — ROM corrupt hoga
- **Hamesha backup rakho**
- Kisi issues ke liye Telegram par join kar

## License
MIT
