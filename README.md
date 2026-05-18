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
