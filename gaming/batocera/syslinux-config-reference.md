# Batocera Syslinux Configuration Reference

This file shows the modified `syslinux.cfg` configuration used to resolve display issues on Dell Inspiron 15R (2011).

## File Location
`/syslinux/syslinux.cfg` on the Batocera USB drive boot partition

## Modified Configuration

```
UI menu.c32
TIMEOUT 10
TOTALTIMEOUT 300
SAY Booting Batocera.linux...
MENU CLEAR
MENU TITLE Batocera.linux
MENU HIDDEN
LABEL batocera
	MENU LABEL Batocera.linux (^normal)
	MENU DEFAULT
	LINUX /boot/linux
	APPEND label=BATOCERA console=tty3 quiet loglevel=0 vt.global_cursor_default=0 nomodeset
	INITRD /boot/initrd.gz
LABEL verbose
	MENU lABEL Batocera.linux (^verbose)
	LINUX /boot/linux
	APPEND label=BATOCERA vt.global_cursor_default=0 nomodeset
	INITRD /boot/initrd.gz
```

## Original Configuration (For Reference)

```
UI menu.c32
TIMEOUT 10
TOTALTIMEOUT 300
SAY Booting Batocera.linux...
MENU CLEAR
MENU TITLE Batocera.linux
MENU HIDDEN
LABEL batocera
	MENU LABEL Batocera.linux (^normal)
	MENU DEFAULT
	LINUX /boot/linux
	APPEND label=BATOCERA console=tty3 quiet loglevel=0 vt.global_cursor_default=0
	INITRD /boot/initrd.gz
LABEL verbose
	MENU lABEL Batocera.linux (^verbose)
	LINUX /boot/linux
	APPEND label=BATOCERA vt.global_cursor_default=0
	INITRD /boot/initrd.gz
```

## Changes Made

Added `nomodeset` to the end of both APPEND lines:
- Normal boot mode (line 11)
- Verbose boot mode (line 16)

## Alternative Configuration (If Basic nomodeset Fails)

For more stubborn display issues, try these additional parameters:

```
APPEND label=BATOCERA console=tty3 quiet loglevel=0 vt.global_cursor_default=0 nomodeset acpi=off i915.modeset=0 nouveau.modeset=0 radeon.modeset=0
```

### Parameter Explanations

- `nomodeset` - Disables kernel mode setting, forces basic framebuffer
- `acpi=off` - Disables ACPI (Advanced Configuration and Power Interface)
- `i915.modeset=0` - Disables Intel graphics modesetting specifically
- `nouveau.modeset=0` - Disables Nvidia open-source driver modesetting
- `radeon.modeset=0` - Disables AMD Radeon modesetting

## How to Edit

1. Remove Batocera USB drive from target device
2. Insert into another computer
3. Open the BATOCERA partition (FAT32)
4. Navigate to `/syslinux/` folder
5. Open `syslinux.cfg` with text editor (Notepad, TextEdit, nano, etc.)
6. Add parameters to APPEND lines
7. Save file
8. Safely eject USB drive
9. Boot target device from USB

## Temporary Boot Parameter

To test parameters without editing the file:

1. At boot, press `Esc` to get `boot:` prompt
2. Type: `batocera nomodeset`
3. Press Enter

This only applies for that single boot session.

## Troubleshooting

### File Not Found
If `syslinux.cfg` is not in `/syslinux/`, check:
- Root of USB drive
- `/boot/syslinux/`
- `/extlinux/` (file might be named `extlinux.conf`)

### GRUB-based Systems
Some Batocera builds use GRUB instead of Syslinux. Look for:
- `/boot/grub/grub.cfg`
- Edit the `linux` line instead of `APPEND`

### Changes Don't Take Effect
- Ensure file is saved before ejecting USB
- Try removing and reinserting USB drive
- Verify you edited the correct partition (BATOCERA boot partition, not share partition)

## Notes

The blank screen issue is common on 2011-era laptops with integrated Intel HD Graphics 3000. The `nomodeset` parameter forces the system to use VESA framebuffer mode instead of kernel mode setting, which resolves display initialization problems.

---

**Hardware Tested:** Dell Inspiron 15R (2011)  
**Graphics:** Intel HD Graphics 3000  
**Working Configuration:** `nomodeset` (basic version)
