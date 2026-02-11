# Recalbox Setup Guide for Raspberry Pi 5

This directory contains documentation and configuration notes for setting up **Recalbox** on the Raspberry Pi 5. Recalbox is an open-source retrogaming OS that allows you to play a vast library of consoles from arcade classics to the 64-bit era.

## 📋 Table of Contents
* [Hardware Requirements](#hardware-requirements)
* [Installation Steps](#installation-steps)
* [ROM Management](#rom-management)
* [BIOS Requirements](#bios-requirements)
* [Post-Installation Tips](#post-installation-tips)

---

## 🛠 Hardware Requirements
* **Board:** Raspberry Pi 5 (4GB or 8GB recommended)
* **Storage:** 32GB+ MicroSD Card (Class 10 or UHS-I)
* **Power:** Official Raspberry Pi 27W USB-C Power Supply (Important for Pi 5 performance)
* **Peripheral:** USB or Bluetooth Controller
* **Storage (Optional):** External USB 3.0 SSD/HDD for large ROM collections

---

## 🚀 Installation Steps

### 1. Flashing the OS
1. Download and install the [Raspberry Pi Imager](https://www.raspberrypi.com).
2. Insert your MicroSD card into your computer.
3. Open the Imager and select:
   * **CHOOSE DEVICE:** Raspberry Pi 5
   * **CHOOSE OS:** Emulation and Game OS > Recalbox > Recalbox - Raspberry Pi 5
   * **CHOOSE STORAGE:** Your MicroSD card
4. Click **Next** and then **Write**.

### 2. Initial Configuration
1. Insert the card into the Pi 5 and connect it to a display.
2. Power on the device. The first boot will take a few minutes to resize partitions.
3. Once the UI loads, plug in a controller and hold any button to enter the **Controller Mapping** menu.

---

## 🎮 ROM Management

Recalbox looks for games in the `/share/roms/` directory. You can transfer games using these methods:

| Method | Access Path | Best For |
| :--- | :--- | :--- |
| **Network (Samba)** | `\\RECALBOX\share\roms\` | Fast transfers over Ethernet |
| **Web Interface** | `http://recalbox.local` | Easy management via Browser |
| **External USB** | Format as `exFAT` | Large libraries (500GB+) |

> **Note:** After adding ROMs, you must go to **Main Menu > Game Settings > Update Games Lists** for them to appear.

---

## 🔑 BIOS Requirements
Certain systems (PlayStation, Dreamcast, Saturn) require BIOS files to function. 
* Place BIOS files in: `/share/bios/`
* Use the **BIOS Checker** in the Recalbox menu to verify if your files have the correct MD5 signatures.

---

## 💡 Post-Installation Tips
* **Scraping:** Use the built-in "Scraper" (Main Menu > Scraper) to download box art and game descriptions.
* **Overclocking:** The Pi 5 is powerful enough for most systems, but ensure you have active cooling (Official Active Cooler) before attempting any performance tweaks.
* **Kodi:** Recalbox comes with Kodi pre-installed. You can toggle it via the Main Menu to turn your Pi into a media center.

---
*Documentation maintained by [Your Name/Username]*
