# Batocera Troubleshooting Guide

Common issues and solutions encountered during Batocera setup and operation.

## Table of Contents
- [Boot Issues](#boot-issues)
- [Display Problems](#display-problems)
- [Input/Controller Issues](#inputcontroller-issues)
- [Network Problems](#network-problems)
- [Performance Issues](#performance-issues)
- [ROM and Game Issues](#rom-and-game-issues)
- [Audio Problems](#audio-problems)

---

## Boot Issues

### USB Drive Not Detected in Boot Menu

**Symptoms:**
- USB drive doesn't appear in BIOS boot menu
- Computer boots to existing OS instead of Batocera

**Solutions:**
1. **Try different USB port**
   - Use USB 2.0 port if USB 3.0 doesn't work
   - Try all available USB ports

2. **Check BIOS boot order**
   - Enter BIOS/UEFI (F2, Del, F10 depending on manufacturer)
   - Enable USB boot option
   - Move USB drive higher in boot priority

3. **Disable Secure Boot** (if present)
   - Enter BIOS/UEFI
   - Find Security settings
   - Disable Secure Boot
   - Save and exit

4. **Verify USB drive was flashed correctly**
   - Reflash using balenaEtcher
   - Try a different USB drive
   - Ensure complete image was written

### Boot Hangs at "Booting Batocera.linux..."

**Symptoms:**
- Sees boot message but system hangs
- No progress beyond initial boot screen

**Solutions:**
1. **Access boot menu and add verbose mode**
   - Press `Esc` at boot
   - Type: `batocera verbose`
   - This will show detailed boot messages

2. **Check USB drive integrity**
   - Reflash the USB drive
   - Try a different USB drive
   - Test USB drive on another computer

3. **Try older Batocera version**
   - Some hardware works better with older releases
   - Download previous stable version

---

## Display Problems

### Blank Screen After Boot Logo

**Symptoms:**
- Batocera logo appears briefly
- Screen goes black but system is running
- Pressing F2 shows battery info (system responsive)

**Primary Solution: Add nomodeset**

1. **Using boot prompt (temporary fix):**
   ```
   - Reboot
   - Press Esc repeatedly at startup
   - At boot: prompt, type: batocera nomodeset
   - Press Enter
   ```

2. **Permanent fix - edit syslinux.cfg:**
   ```
   - Remove USB drive
   - Insert into another computer
   - Open /syslinux/syslinux.cfg
   - Add 'nomodeset' to end of APPEND lines
   - Save and eject
   ```

**Advanced Solution: Additional Parameters**

If nomodeset alone doesn't work, try:
```
nomodeset acpi=off i915.modeset=0
```

**Parameter combinations to try:**
1. `nomodeset`
2. `nomodeset acpi=off`
3. `nomodeset acpi=off i915.modeset=0`
4. `nomodeset acpi=off i915.modeset=0 nouveau.modeset=0 radeon.modeset=0`

### Display Output on Wrong Monitor

**Symptoms:**
- Laptop screen blank but system running
- External monitor might show output
- Or vice versa

**Solutions:**
1. **Cycle display outputs**
   - Press `Fn + F8` (or monitor icon key)
   - Try multiple times to cycle through options

2. **Connect/disconnect external monitor**
   - System may default to HDMI if connected
   - Try booting with only laptop display

3. **Edit display configuration**
   - Press `Start` → `System Settings` → `Display`
   - Change output settings

### Resolution Issues

**Symptoms:**
- Display is stretched or squashed
- Wrong aspect ratio
- Text is too large/small

**Solutions:**
1. **In EmulationStation:**
   - Press `Start` → `System Settings` → `Display`
   - Change resolution
   - Set to native display resolution

2. **Edit es_settings.cfg:**
   ```
   - Access USB drive on another computer
   - Navigate to /share/system/.emulationstation/
   - Edit es_settings.cfg
   - Find VideoMode setting
   - Change to your native resolution
   ```

---

## Input/Controller Issues

### Controller Not Recognized

**Symptoms:**
- USB controller plugged in but not responding
- No vibration or LED indication

**Solutions:**
1. **Try different USB port**
   - Switch to USB 2.0 port
   - Try all available ports

2. **Manual configuration:**
   - Press `Start` on keyboard
   - Go to "Controller Settings"
   - Hold a button on controller to start config
   - Map all buttons as prompted

3. **Check controller compatibility:**
   - Press `Fn + F4` to open terminal
   - Type: `evtest`
   - See if controller is detected

4. **Restart Batocera:**
   - Sometimes requires reboot after connecting controller

### Keyboard Not Working

**Symptoms:**
- Cannot navigate menus with keyboard
- No response to key presses

**Solutions:**
1. **Default keyboard mappings:**
   - Enter = Start (open menu)
   - Space = Select
   - Arrow keys = Navigation
   - A = Accept
   - B = Back

2. **Try different keyboard:**
   - Some keyboards need USB hub
   - Wireless keyboards may not work without proper receiver

3. **Configure keyboard as controller:**
   - If recognized as controller, reconfigure
   - `Start` → `Controller Settings`

### Controller Buttons Mapped Wrong

**Symptoms:**
- Buttons don't match expected actions
- Can't navigate menus properly

**Solutions:**
1. **Reconfigure controller:**
   - Press `Start` → `Controller Settings`
   - Hold any button on the controller
   - Follow prompts to remap all buttons
   - Skip unmapped buttons with long press

2. **Reset to defaults:**
   - Delete controller config file
   - Navigate to: `/share/system/configs/`
   - Remove controller-specific files
   - Restart and reconfigure

---

## Network Problems

### Can't Access Network Shares

**Symptoms:**
- Unable to connect to \\BATOCERA
- Network share not visible

**Solutions:**
1. **Check ethernet connection:**
   - Verify cable is plugged in
   - Look for link lights on port
   - Check if IP address is shown on Batocera screen

2. **Note the IP address:**
   - Use IP directly: `\\192.168.1.xxx` (Windows)
   - Or: `smb://192.168.1.xxx` (Mac/Linux)

3. **Windows SMBv1 requirement:**
   - Windows 10/11 may need SMBv1 enabled
   - Control Panel → Programs → Turn Windows features on/off
   - Enable "SMB 1.0/CIFS File Sharing Support"

4. **Check firewall:**
   - Temporarily disable firewall to test
   - Add exception for Samba/SMB if firewall blocks

5. **Authentication:**
   - Default username: `root`
   - Default password: `linux`

### WiFi Not Working

**Symptoms:**
- WiFi adapter not detected
- Cannot see wireless networks

**Solutions:**
1. **Check WiFi hardware:**
   - Some adapters require firmware
   - USB WiFi adapters usually work better than internal

2. **Use ethernet instead:**
   - More reliable for file transfers
   - Batocera works fine with wired connection

3. **Manual WiFi configuration:**
   - Press `Start` → `Network Settings`
   - Enable WiFi
   - Scan for networks
   - Enter credentials

---

## Performance Issues

### Games Running Slowly

**Symptoms:**
- Choppy gameplay
- Low frame rates
- Audio stuttering

**Solutions:**
1. **Try different emulator core:**
   - In-game: Press `Hotkey + X` (RetroArch menu)
   - Quick Menu → Core
   - Switch to lighter alternative (e.g., Snes9x instead of bsnes)

2. **Reduce resolution:**
   - In-game RetroArch menu
   - Settings → Video → Scaling
   - Lower internal resolution

3. **Disable shaders and effects:**
   - RetroArch menu → Quick Menu → Shaders
   - Set to "None"

4. **Overclock (if supported):**
   - Press `Start` → `System Settings` → `Overclock`
   - Increase if available for your hardware

5. **Close background processes:**
   - Press `Fn + F4` for terminal
   - Check running processes: `top`
   - Kill unnecessary processes

### System Overheating

**Symptoms:**
- Laptop gets very hot
- Fan running constantly at high speed
- Performance drops after extended play

**Solutions:**
1. **Clean cooling system:**
   - Blow out dust from vents
   - Consider opening laptop to clean fans

2. **Replace thermal paste:**
   - If laptop is old, thermal paste may be dry
   - Professional service or DIY if comfortable

3. **Improve ventilation:**
   - Don't block air vents
   - Use laptop cooling pad
   - Elevate laptop for better airflow

4. **Reduce graphics settings:**
   - Lower resolution in demanding games
   - Disable shaders and effects
   - Use lighter emulator cores

---

## ROM and Game Issues

### "No Games Found"

**Symptoms:**
- Added ROMs but they don't appear
- System shows empty game list

**Solutions:**
1. **Verify ROM location:**
   - Must be in `/share/roms/[system]/` folders
   - e.g., NES games in `/share/roms/nes/`

2. **Check file extensions:**
   - Ensure files have correct extension
   - Refer to system-specific supported formats
   - Remove any extra extensions (e.g., .txt)

3. **Restart Batocera:**
   - Games are scanned at boot
   - Reboot to rescan ROM folders

4. **Manual rescan:**
   - Press `Start` → `Game Settings`
   - Select "Update Games Lists"

### Game Won't Load

**Symptoms:**
- Game appears in list but won't start
- Black screen when launching
- Returns to menu immediately

**Solutions:**
1. **Check BIOS requirement:**
   - Some systems need BIOS files
   - PlayStation, Dreamcast, etc.
   - Place in `/share/bios/` folder
   - Check Batocera Wiki for required BIOS files

2. **Verify ROM integrity:**
   - File may be corrupted
   - Try different ROM dump
   - Check file size matches expected

3. **Try different emulator:**
   - Press `Select` on game
   - Choose "Advanced System Options"
   - Select different emulator core

4. **Check file format:**
   - Some formats need extraction (.zip, .7z)
   - Some systems prefer certain formats
   - PlayStation: .bin/.cue preferred over .iso

### Saves Not Working

**Symptoms:**
- Game progress not saving
- Save states fail
- Memory card errors

**Solutions:**
1. **Check storage space:**
   - USB drive may be full
   - Free up space by removing unused ROMs/media

2. **Verify save location:**
   - Saves stored in `/share/saves/[system]/`
   - Ensure USB drive is writable

3. **Use save states instead:**
   - Press `Hotkey + R1` to save state
   - Press `Hotkey + L1` to load state
   - More reliable than in-game saves

4. **Check BIOS for memory card systems:**
   - PlayStation needs proper BIOS
   - Memory card may not initialize without correct BIOS

---

## Audio Problems

### No Sound

**Symptoms:**
- Games load but no audio
- System sounds missing

**Solutions:**
1. **Check volume:**
   - Press `Start` → `Sound Settings`
   - Increase system volume
   - Check if muted

2. **Test different output:**
   - Try headphones in 3.5mm jack
   - Check if HDMI audio works (if using external monitor)

3. **Audio device selection:**
   - `Start` → `Sound Settings` → `Output Device`
   - Select correct audio output

4. **In-game audio settings:**
   - RetroArch menu → Quick Menu → Audio
   - Ensure audio is enabled
   - Check volume levels

### Crackling or Distorted Audio

**Symptoms:**
- Audio plays but sounds bad
- Popping, crackling, or stuttering

**Solutions:**
1. **Adjust audio latency:**
   - RetroArch menu → Settings → Audio
   - Increase audio latency (32ms to 64ms)

2. **Change audio driver:**
   - `Start` → `Sound Settings`
   - Try different audio backend

3. **Reduce system load:**
   - Lower graphics settings
   - Disable shaders
   - Close unnecessary processes

4. **Check for overheating:**
   - Thermal throttling can cause audio issues
   - Improve cooling

---

## General Troubleshooting Steps

### System Won't Boot at All

1. Verify USB drive is bootable
2. Try different USB port
3. Check BIOS settings
4. Reflash USB drive
5. Try different USB drive
6. Test on different computer to isolate issue

### System Crashes or Freezes

1. Check for overheating
2. Test RAM (if persistent)
3. Try older Batocera version
4. Check system logs:
   - Press `Fn + F4` for terminal
   - View logs: `dmesg` or `journalctl`

### Reset to Default Settings

1. Delete configuration files:
   - `/share/system/.emulationstation/`
   - `/share/system/configs/`
2. Restart Batocera
3. Reconfigure from scratch

### Complete Reinstall

1. Backup save files from `/share/saves/`
2. Backup ROMs from `/share/roms/`
3. Reformat USB drive
4. Reflash Batocera image
5. Restore saves and ROMs

---

## Getting Help

### Before Asking for Help

Gather this information:
- Exact hardware model and specs
- Batocera version (shown at boot)
- Specific error messages
- Steps taken so far
- What works and what doesn't

### Useful Debug Commands

Access terminal with `Fn + F4`:

```bash
# Check system information
batocera-info

# View system logs
dmesg | tail -50

# Check USB devices
lsusb

# Check input devices
evtest

# View running processes
top

# Check disk space
df -h

# Network status
ifconfig
```

### Resources

- **Batocera Wiki**: https://wiki.batocera.org/
- **Official Forum**: https://batocera.org/forum/
- **Discord**: Official Batocera Discord
- **Reddit**: r/batocera
- **GitHub Issues**: https://github.com/batocera-linux/batocera.linux

---

## Known Hardware-Specific Issues

### Dell Inspiron 15R (2011)
- **Issue**: Blank screen after boot
- **Solution**: Add `nomodeset` to boot parameters
- **Status**: Confirmed fix

### Intel HD Graphics 3000
- **Issue**: Display initialization problems
- **Solution**: `nomodeset` parameter
- **Status**: Common issue, well documented

### Broken WiFi Adapters
- **Issue**: Internal WiFi hardware failure
- **Solution**: Use ethernet or USB WiFi adapter
- **Status**: Hardware issue, not Batocera related

---

**Last Updated:** February 2026  
**For:** Batocera v42 and similar versions
