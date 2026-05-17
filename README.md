# Nothing Phone (2a) Dual Boot Project 🚀

## 📱 Parichay (Introduction)
Welcome! 👋 Ye ek bilkul naya aur advanced project he **Nothing Phone (2a) [pacman]** par **Dual Boot** configuration ko successfully setup karne ka. 

Agar aap ek hi phone me Stock Nothing OS ka maza bhi lena chahte hain aur sath me kisi custom ported ROM ko bhi explore karna chahte hain, to yeh guide aur project aap hi ke liye hai! Is project ki madad se aap ek advanced partitioning layout ke sath apne phone me dual boot ka maza le sakte hain.

---

## 🚨 Section 2: Warnings & Critical Data Backup (Chetavni aur Backup)

⚠️ **IMPORTANT DISCLAIMER:** Yeh ek bahut hi advanced process hai. Choti si galti se phone brick ho sakta hai, isliye sab kuch apne risk par karein!

### 🛑 Data Wipe Alert (Aapka Data Delete Hoga!)
- **100% Data Wipe:** Is dual boot process me partitions ko re-size aur modify kiya jayega, jisse aapka saara **Internal Storage aur Data poori tarah wipe (delete) ho jayega**.
- **Backup Mandatory:** Process shuru karne se pehle apne saare Photos, Videos, Documents aur WhatsApp data ka backup apne PC ya kisi external drive me **ZAROOR** le lein.

### ⚡ Vendor-Fastboot Base & OTA Warning (Sabse Zaroori Baat!)
- **Dual Boot Base:** Yeh dual boot setup poori tarah **Vendor-Fastboot Base** par kaam karta hai.
- **OTA Update Update Crash:** Agar aapne stock settings se koi bhi official **OTA update** kiya, to aapka dual boot setup turant **crash** ho jayega aur phone bootloop me ja sakta hai.
- **Manual Flash Only:** Isliye official OTA update ko bilkul block kar dein. Jab bhi koi naya update aayega, aapko use **Manually Flash** karna hoga. 
- **Update Files:** Manual update ke liye jo bhi edited aur tweaked files hongi, wo **Main (Nikhil-Development)** isee project par provide kurunga.

---

## 🛠️ Section 3: Downloads & Requirements (Zaroori Files)

PC me ADB & Fastboot Drivers install rakhein aur neeche di gayi files upar main folder se download kar lein, aur baki tools ke liye Telegram Group join karein:

- 📂 **TWRP Recovery:** Nothing Phone (2a) compatible recovery file.
- 📂 **Parted Tool:** Partition table modify karne ke liye modified tool.
- 📂 **Commands.txt:** Saare partitioning aur flashing commands ki ready-made text file.
- 📢 **ADB Fastboot Tools & Drivers:** Latest drivers aur tools hamesha mere Telegram Group par update hote rahenge. [Download from My Telegram Group](https://t.me/star7725)

---

## 🔓 Section 4: Step 0: Bootloader Unlocking (Bootloader Unlock Kaise Karein)

Dual Boot setup shuru karne ke liye aapke Nothing Phone (2a) ka bootloader unlocked hona zaroori hai. Agar unlock nahi hai, to in steps ko follow karein:

1. **Enable Developer Options:** Phone ki Settings -> About Phone me jayein aur `Build Number` par 7 baar click karein.
2. **OEM Unlocking:** Settings -> System -> Developer Options me jakar `OEM Unlocking` aur `USB Debugging` ko ON kar dein.
3. **Fastboot Mode:** Phone ko PC se connect karein aur command prompt (CMD) me yeh command run karke phone ko fastboot mode me dalein:
```bash
adb reboot bootloader
```
 4. **Unlock Command:** Fastboot mode me aane ke baad, PC par yeh command run karein:
```bash
fastboot flashing unlock

```
 5. **Confirm on Phone:** Phone ki screen par Volume buttons ka use karke Unlock the bootloader ko select karein aur Power button daba dein. *(Note: Isse aapka phone wipe/reset ho jayega).*
## 🛠️ Section 5: Step 1: Flashing TWRP Recovery (TWRP Flash Kaise Karein)
Bootloader unlock karne ke baad, phone ko fastboot mode me re-boot karein aur TWRP recovery ko flash karein:
 1. **Reboot to Fastboot Mode:**
```bash
adb reboot bootloader

```
 2. **Flash TWRP to Both Slots (A & B):**
   PC par cmd/terminal open karein aur yeh command run karein:
```bash
fastboot flash vendor_boot_a twrp.img
fastboot flash vendor_boot_b twrp.img

```
## 🏗️ Section 6: Step 2: Partitioning Initialization (Parted Tool Setup)
TWRP flash karne ke baad phone ko recovery me le jayein aur partition table ko initialize karne ke liye yeh steps follow karein:
 1. **Reboot to Recovery Mode:**
```bash
fastboot reboot recovery

```
 2. **Disable MTP (Zaroori Step):**
   Phone me TWRP screen par **Mount** option me jayein aur **Disable MTP** par tap karein. Isse adb push me koi error nahi aayega.
 3. **Connect Phone to PC:** USB cable ke zariye phone ko PC se connect rakhein.
 4. **Push Tools & Set Permissions:**
   PC par Command Prompt (CMD) open karein aur ek-ek karke yeh saare commands run karein:
```bash
adb push parted /sbin
adb push mkfs.ext4 /sbin
adb shell
chmod 777 /sbin/parted
chmod 777 /sbin/mkfs.ext4

```
 5. **Open Partition Table:**
   Ab phone ki internal storage layout open karne ke liye yeh command run karein:
```bash
parted /dev/block/sdc

```
 6. **Change Unit to GB (Confusion Door Karne Ke Liye):**
   Parted mode me sizes ko KB/MB ke bajaye GB me dekhne ke liye print chalane se pehle yeh command run karein:
```text
unit gb

```
 7. **Print Partition List:**
   Ab apne phone ki saari partitions ki list dekhne ke liye type karein:
```text
print

```
## ✂️ Section 7: Step 3: Backup & Modifying Partition Table
⚠️ **RULE:** Jis partition ko resize karna hota hai, uske baad wale partitions ko reverse order me remove kiya jata hai. Hum yahan userdata layout par kaam kar rahe hain.
 1. **Copy & Paste to Notepad:**
   print chalane ke baad, terminal me partition **82** aur **83** ki details (Start and End Values) ko copy karke PC par **Notepad** me safe save kar lein.
 2. **Delete Partition 82:**
```text
rm 82

```
 3. **Backup Partition 83 (Crucial Step):**
   Partition 83 delete karne se pehle uska backup PC par lena mandatory hai. Parted se temporary bahar aane ke liye type karein:
```text
quit

```
Ab CMD me yeh commands run karke partition 83 ka backup PC par copy karein:
```bash
adb shell dd if=/dev/block/sdc83 of=/sdcard/sdc83.img
adb pull /sdcard/sdc83.img

```
 4. **Delete Partition 83:**
   Backup successfully PC par copy hone ke baad, wapas parted me jayein:
```bash
parted /dev/block/sdc
unit gb

```
Aur partition 83 ko remove karein:
```text
rm 83

```
## 📐 Section 8: Step 4: Creating & Formatting New Partitions
Ab hum dono OS ke liye alag-alag userdata space create karenge. Apne phone ke variant ke hisab se calculations follow karein:
### 📱 For 128GB Variant (Example Layout):
 * **Userdata (Slot A - Custom ROM):** 12.1 se 69.6
 * **Userdata_b (Slot B - Stock ROM):** 69.6 se 128
*(Note: Agar aapka **256GB Variant** hai, to isi tarah space ko divide karke size calculate karein, jaise 12.1 se 134 aur 134 se 256).*
### 1. Create New Partitions (Parted ke andar):
```text
mkpart userdata 12.1gb 69.6gb
mkpart userdata 69.6gb 128gb

```
*(Apne variant ke calculated GB values hi enter karein).*
### 2. Name the Partitions:
Partitions ko sahi identity dene ke liye yeh commands run karein:
```text
name 82 userdata
name 83 userdata_b

```
### 3. Exit Parted:
```text
quit

```
### 4. Format New Partitions to F2FS:
Ab terminal (adb shell) me dono naye partitions ko f2fs filesystem me format karein:
```bash
make_f2fs /dev/block/sdc82
make_f2fs /dev/block/sdc83

```
## 💾 Section 9: Step 5: Restoring Partition 83 Backup (sdc83 Restore)
Naye partitions banne aur format hone ke baad, ab hum PC par save kiye gaye partition 83 ke backup ko wapas restore karenge:
 1. **Push Backup Image to Phone:**
   PC par CMD open karein aur yeh command run karein:
```bash
adb push sdc83.img /sdcard/

```
 2. **Flash Backup to New Partition 83 (sdc83):**
   Image push hone ke baad, use dd command se wapas original location par flash karein:
```bash
adb shell dd if=/sdcard/sdc83.img of=/dev/block/sdc83

```
## 📦 Section 10: Step 6: Flashing the Dual Boot ROM
Saare partitions taiyar aur restore hone ke baad, ab phone ko wapas fastboot mode me le jana hai:
 1. **Reboot to Bootloader:**
```bash
adb reboot bootloader

```
 2. **Flash the Modified ROM:**
   Ab mere (Nikhil-Development) dwara provide ki gayi **tweaked/edited Fastboot Flashable Stock ROM** ko flash karein, jo user data_b aur baki settings ke sath safely dual boot run karegi.
**Created and maintained by Nikhil-Development-Android**
```
