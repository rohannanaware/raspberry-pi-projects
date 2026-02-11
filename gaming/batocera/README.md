# Batocera Linux Gaming System

![Batocera Logo](https://batocera.org/images/logo.png)

## Overview

Batocera is a Linux-based operating system designed specifically for retro gaming and emulation. It runs entirely from a USB drive or SD card, making it perfect for converting old hardware into dedicated gaming consoles without modifying the existing operating system.

## Project Details

**Hardware Used:** Dell Inspiron 15R (2011)
- CPU: Intel Core i5 (2nd Gen)
- RAM: 4-8GB DDR3
- Storage: USB 3.0 Flash Drive (16GB+)
- Network: Ethernet (WiFi adapter non-functional)
- Display: 15.6" integrated display

**Software:**
- Batocera Linux v42 (x86_64)
- Build Date: 2024-10-06
- Download: [batocera.org](https://batocera.org/)

## Features

- 🎮 Supports 100+ gaming systems and emulators
- 💾 Runs entirely from USB/SD card - no hard drive installation needed
- 🎨 Beautiful EmulationStation frontend
- 🎯 Automatic controller configuration
- 📦 Includes RetroArch and standalone emulators
- 🌐 Network ROM transfer support
- 🖼️ Game metadata and artwork scraping
- 💾 Save states and game progress tracking

## Prerequisites

### Required
- USB flash drive (16GB minimum, 32GB+ recommended)
- Computer with internet access (for initial setup)
- Target device (old laptop, PC, or Raspberry Pi)

### Optional
- Ethernet cable (for ROM transfer and metadata scraping)
- USB game controllers (Xbox, PlayStation, or generic USB controllers)
- External monitor (for troubleshooting display issues)

## Installation Guide

### Step 1: Download Batocera

1. Visit [batocera.org](https://batocera.org/)
2. Download the appropriate version for your hardware:
   - **x86_64**: For Intel/AMD laptops and desktops
   - **RPi4**: For Raspberry Pi 4
   - **RPi3**: For Raspberry Pi 3
3. File will be compressed: `batocera-x86_64-XX-YYYYMMDD.img.gz`

### Step 2: Create Bootable USB

#### Using balenaEtcher (Recommended - All Platforms)

1. Download [balenaEtcher](https://www.balena.io/etcher/)
2. Install and launch balenaEtcher
3. Click "Flash from file" and select the `.img.gz` file (no need to extract)
4. Click "Select target" and choose your USB drive
5. Click "Flash!" 
6. Enter system password when prompted (required for disk access)
7. Wait for the process to complete

#### Using Rufus (Windows Alternative)

1. Download [Rufus](https://rufus.ie/)
2. Insert USB drive
3. Select the Batocera `.img.gz` file
4. Click "Start"
5. Wait for completion

#### Using dd (Linux/Mac Command Line)

```bash
# Extract the image
gunzip batocera-x86_64-42-20251006.img.gz

# Flash to USB (replace /dev/sdX with your USB device)
sudo dd if=batocera-x86_64-42-20251006.img of=/dev/sdX bs=4M status=progress
sudo sync
```

⚠️ **Warning:** Double-check the device path - dd will overwrite the specified drive without confirmation!

### Step 3: Boot from USB

1. Insert the Batocera USB drive into your target device
2. Power on and access boot menu:
   - **Dell**: Press `F12` repeatedly during startup
   - **HP**: Press `F9` or `Esc`
   - **Lenovo**: Press `F12` or `F1`
   - **ASUS**: Press `F8` or `Esc`
3. Select the USB drive from the boot menu
4. Batocera should begin loading

## Troubleshooting

### Blank Screen After Boot Logo

This is common on older laptops with Intel graphics. The system boots but display doesn't initialize properly.

**Solution: Add nomodeset boot parameter**

1. Plug USB drive into another computer
2. Open the USB drive (BATOCERA partition)
3. Edit `/syslinux.cfg` with a text editor
4. Find the `APPEND` lines
5. Add `nomodeset` at the end of each APPEND line

**Original:**
```
APPEND label=BATOCERA console=tty3 quiet loglevel=0 vt.global_cursor_default=0
```

**Modified:**
```
APPEND label=BATOCERA console=tty3 quiet loglevel=0 vt.global_cursor_default=0 nomodeset
```

6. Save the file and safely eject the USB
7. Boot again from the USB drive

### Advanced Boot Parameters (If nomodeset Alone Doesn't Work)

For persistent display issues on older hardware, try these additional parameters:

```
APPEND label=BATOCERA console=tty3 quiet loglevel=0 vt.global_cursor_default=0 nomodeset acpi=off i915.modeset=0
```

**Parameter explanations:**
- `nomodeset` - Disables kernel mode setting for graphics
- `acpi=off` - Disables advanced power management (can cause display issues)
- `i915.modeset=0` - Specifically disables Intel graphics modesetting

### Boot Menu Access

To access boot options at startup:

1. Reboot with Batocera USB inserted
2. Press `Esc` repeatedly as soon as the screen shows anything
3. You'll see a `boot:` prompt
4. Type: `batocera nomodeset` and press Enter

This applies nomodeset for one boot without editing the config file.

### System Responds But No Display

If pressing `F2` shows battery percentage but screen is otherwise blank:

1. Try cycling display outputs: `Fn + F8` (or monitor icon key)
2. Connect an external monitor to rule out laptop display issues
3. The system may have booted successfully but display output is incorrect

### No Internet Connection

Batocera works perfectly fine without internet for:
- Basic operation and gaming
- Controller configuration
- Playing games

Internet is only needed for:
- Game metadata/artwork scraping
- System updates
- Netplay (online multiplayer)
- Network ROM transfer

You can add ROMs by directly copying files to the USB drive on another computer.

## Adding Games (ROMs)

### Method 1: Direct USB Access (No Internet Required)

1. Plug the Batocera USB into another computer
2. Navigate to the USB drive
3. Open the `share/roms/` folder
4. You'll see folders for each system (nes, snes, psx, etc.)
5. Copy your ROM files into the appropriate folders
6. Safely eject and boot Batocera

### Method 2: Network Transfer (Requires Ethernet)

1. Connect ethernet cable to your Batocera device
2. Boot into Batocera
3. Note the IP address shown on screen
4. On another computer, access via network share:
   - **Windows**: `\\BATOCERA` or `\\[IP-ADDRESS]`
   - **Mac**: `smb://batocera` or `smb://[IP-ADDRESS]`
   - **Linux**: `smb://batocera` or `smb://[IP-ADDRESS]`
5. Navigate to `share/roms/` and copy files

### Supported ROM Formats

Different emulators support different formats. Common ones:

| System | Formats |
|--------|---------|
| NES | .nes, .zip |
| SNES | .sfc, .smc, .zip |
| Game Boy | .gb, .gbc, .zip |
| Game Boy Advance | .gba, .zip |
| PlayStation 1 | .bin/.cue, .iso, .pbp |
| PlayStation 2 | .iso |
| Nintendo 64 | .z64, .n64, .v64 |
| Sega Genesis | .md, .bin, .gen |
| Arcade (MAME) | .zip |

## Configuration

### Controller Setup

1. Plug in your USB controller
2. Press `Start` on the controller (or `Enter` on keyboard)
3. Select "Controller Settings"
4. Press and hold a button on the controller to configure
5. Follow on-screen prompts to map all buttons
6. Configuration is saved automatically

### System Settings

Press `Start` → `System Settings` to access:
- Display resolution
- Audio settings
- Language
- Timezone
- Network configuration
- Storage information

### Scraping Game Metadata

Requires internet connection:

1. Press `Start` → `Scraper`
2. Choose scraping source (ScreenScraper recommended)
3. Select systems to scrape
4. Start scraping
5. Batocera will download:
   - Game cover art
   - Screenshots
   - Game descriptions
   - Ratings and release dates

## Performance Tips

### For Older Hardware (Like Inspiron 15R)

- Stick to 8-bit and 16-bit systems (NES, SNES, Genesis)
- PlayStation 1 should run fine
- N64 may have some slowdowns
- Avoid PlayStation 2 and GameCube
- Dreamcast might work for some games
- Arcade games vary - MAME 2003 core is lighter

### Recommended Systems for 2011-era Laptops

✅ **Will Run Well:**
- NES, SNES, Genesis, Master System
- Game Boy, Game Boy Color, Game Boy Advance
- PlayStation 1
- DOS games
- Arcade (older MAME sets)

⚠️ **May Struggle:**
- Nintendo 64
- Dreamcast
- PSP

❌ **Likely Too Heavy:**
- PlayStation 2
- GameCube
- Wii

## File Structure

Understanding the USB drive layout:

```
/batocera/
├── boot/                    # Boot files (don't modify)
├── share/
│   ├── roms/               # Game ROMs organized by system
│   │   ├── nes/
│   │   ├── snes/
│   │   ├── psx/
│   │   └── ...
│   ├── bios/               # BIOS files for systems that need them
│   ├── saves/              # Save states and game saves
│   ├── screenshots/        # Screenshots taken in-game
│   └── system/             # System configuration files
└── ...
```

## Required BIOS Files

Some systems require BIOS files to work. Place these in `share/bios/`:

| System | Required Files |
|--------|----------------|
| PlayStation 1 | scph5501.bin, scph5502.bin, scph7001.bin |
| PlayStation 2 | SCPH-XXXXX.bin files |
| Dreamcast | dc_boot.bin, dc_flash.bin |
| Nintendo DS | bios7.bin, bios9.bin, firmware.bin |

Check [Batocera Wiki](https://wiki.batocera.org/) for complete BIOS requirements.

## Useful Hotkeys

During gameplay (with keyboard or configured controller):

- `Hotkey + Start` - Exit game
- `Hotkey + Select` - Show RetroArch menu
- `Hotkey + L1` - Load state
- `Hotkey + R1` - Save state
- `Hotkey + Y` - Screenshot
- `Hotkey + B` - Reset game

**Default hotkey is `Select` button on controller or `F1` on keyboard**

## Maintenance

### Updating Batocera

1. Connect ethernet cable
2. Press `Start` → `System Settings` → `Updates`
3. Check for updates
4. Download and install if available
5. System will reboot automatically

### Backing Up Save Data

Copy the `share/saves/` folder from the USB drive to another location regularly.

### Freeing Up Space

- Delete scraped images you don't need: `share/system/.emulationstation/downloaded_media/`
- Remove unused ROM files
- Clear old save states: `share/saves/[system]/`

## Common Issues

### "No Games Found" After Adding ROMs

- Ensure ROMs are in the correct system folder
- Check file extensions are supported
- Restart Batocera to rescan

### Controller Not Working

- Try different USB ports
- Some controllers need manual configuration
- Check in `Start` → `Controller Settings`

### Game Runs Slowly

- Try different emulator core: In-game menu → Quick Menu → Core
- Lower resolution in game settings
- Disable shaders and effects

### Can't Connect via Network

- Check ethernet cable is properly connected
- Note the IP address shown on Batocera screen
- Windows may need SMBv1 enabled for network shares

## Resources

- **Official Website**: [batocera.org](https://batocera.org/)
- **Wiki**: [wiki.batocera.org](https://wiki.batocera.org/)
- **Forum**: [batocera.org/forum](https://batocera.org/forum/)
- **Discord**: Official Batocera Discord server
- **GitHub**: [github.com/batocera-linux](https://github.com/batocera-linux)

## Legal Notice

Batocera is free and open-source software. However:

- Downloading ROMs of games you don't own is illegal in most countries
- BIOS files are copyrighted and should only be used if you own the original hardware
- This documentation is for educational purposes only
- Always respect copyright laws in your jurisdiction

## Project Status

- ✅ Successfully created bootable USB
- ✅ Boot process working
- ⚠️ Display issues resolved with `nomodeset` parameter
- ⏳ ROM collection in progress
- ⏳ Controller configuration pending
- ⏳ Network setup pending (awaiting ethernet cable)

## Next Steps

- [ ] Resolve display initialization issues
- [ ] Configure USB controllers
- [ ] Add ROM collection
- [ ] Test performance across different emulators
- [ ] Set up network sharing for easier ROM transfer
- [ ] Document performance benchmarks for this hardware
- [ ] Create backup strategy for save files

## Notes

This setup demonstrates that even a 2011 laptop with a broken WiFi adapter can be repurposed as a dedicated retro gaming console. The ethernet-only connectivity is sufficient for ROM transfer and metadata scraping, while the USB boot method preserves the original hard drive installation.

---

**Created:** February 2026  
**Hardware:** Dell Inspiron 15R (2011)  
**Status:** In Progress
