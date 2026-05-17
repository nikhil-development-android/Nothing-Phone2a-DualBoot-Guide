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
   
