# Nothing-Phone2a-DualBoot-Guide
A step-by-step guide to configure dual boot Nothing Phone (2a).
# Nothing Phone (2a) [pacman] Dual Boot Guide

Yeh guide Nothing Phone (2a) par bina primary OS ko nuksaan pahunchaye ya custom partitioning ke sath Dual Boot setup karne ki poori jankari pradan karti hai.

⚠️ **DISCLAIMER (Chetavni):**
Yeh ek advance process hai. Iske karan aapka data delete ho sakta hai ya phone brick ho sakta hai. Ise poori tarah apne risk par karein. Main ya koi bhi anya developer aapke device ko hone wale nuksaan ke liye zimmeydar nahi honge.

---

## 🚨 CRITICAL: Data Backup (Pehle Backup Lein!)
Chunki hum `parted` tool ka use karke partitions (`rm 81`, `rm 82`) ko delete aur re-create karenge, isliye **aapka saara internal storage aur data 100% delete ho jayega**. Process shuru karne se pehle neeche diye gaye tareeqon se backup zaroor lein:

1. **Internal Storage:** Apne saare Photos, Videos, Documents aur Whatsapp Media ko PC ya kisi external OTG drive me copy kar lein.
2. **App Data & Settings:** Google One Backup (Settings -> System -> Backup) ko on karke apna contacts, call logs aur app data cloud par sync kar lein.
3. **Important Files:** Agar aapke paas pehle se koi custom profiles ya keys hain, to unhe safe jagah save karein.

---

## 📱 Features & Context
- **Supported Device:** Nothing Phone (2a) (Codenamed: `pacman`)
- **Setup Type:** Dual Booting Custom ROMs along with Stock ROM inside `user data_b`.
- **Partition Modification:** Custom Super Partition sizing (`14GB super` allocation via `parted` tool).
- **Flash Files:** Modified Fastboot Stock ROM (edited with small tweaks) will be provided in this project.

---

## 🛠️ Prerequisites (Zaroori Cheezein)
- Unlocked Bootloader
- PC with ADB & Fastboot Drivers installed
- **Modified Parted Tool** (Android partition table ko edit karne ke liye)
- `vbmeta.img` (with verity and verification disabled flags)

---

## 🚀 Installation & Partitioning Steps

### 1. Resizing the Super Partition
Partition table ko modify karne ke liye `parted` tool ka upyog karke partition 81 aur 82 (ya aapke specific layout ke anusar) ko delete karke ek naya `super` partition space create karein:

```bash
# Example parted initialization commands
parted /dev/block/sda
print
rm 81
rm 82
mkpart super ext4 <start_sector> <end_sector>
