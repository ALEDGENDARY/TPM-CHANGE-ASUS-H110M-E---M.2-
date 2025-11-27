# TPM Change Guide for ASUS H110M-E M.2

## ⚠️ WARNING
**This process will cause a black screen** because we are downgrading the BIOS to the very first version. Proceed with caution and at your own risk.

## Overview
This guide explains how to clear TPM and perform a BIOS downgrade/upgrade process for ASUS H110M-E M.2 motherboards to reset TPM security settings.

## Prerequisites

### Hardware Requirements
- USB drive (4GB minimum)
- CH341A BIOS Programmer
- ASUS H110M-E M.2 Motherboard
- BIOS chip removal tools

### Software Requirements
- Rufus (for creating DOS bootable USB)
- NeoProgrammer
- UEFI Tool
- AFUDOS.exe

### BIOS Files Needed
- Lowest version: `H110M-E-M2-ASUS-0404.rom`
- Latest version: `H110M-E-M2-ASUS-4210.bin`

## Preparation Steps

### Step 1: Clear TPM Settings
1. Open `tpm.msc` in Windows
2. Clear TPM from the management console
3. Restart computer

### Step 2: Clear Secure Boot Keys
1. Enter BIOS setup (Del key during boot)
2. Navigate to Security/Secure Boot settings
3. Clear Secure Boot keys
4. Save and restart

## BIOS Modification Process

### Step 3: Prepare DOS Bootable USB
1. Use Rufus to format USB as DOS bootable
2. Copy these files to the USB root:
   - `AFUDOS.exe`
   - `AUTOEXEC.BAT`
   - `bios.rom` (converted from H110M-E-M2-ASUS-0404)

### AUTOEXEC.BAT Content
```bat
@echo off
cls
afudos.exe bios.rom /GAN
```

### Step 4: Convert BIOS Files
1. Use UEFI Tool to convert:
   - `H110M-E-M2-ASUS-0404` → `bios.rom` (for USB flashing)
   - `H110M-E-M2-ASUS-4210` → `.bin` (for programmer flashing)

## BIOS Flashing Process

### Step 5: Flash Lowest BIOS via USB
1. Boot from the prepared USB drive
2. The AUTOEXEC.BAT will automatically run AFUDOS
3. Wait for flashing to complete
4. System may black screen - this is expected

### Step 6: Physical BIOS Chip Programming
1. **Carefully remove the BIOS chip** from the motherboard
2. Connect to CH341A programmer
3. Open NeoProgrammer software
4. Load the latest BIOS file (`H110M-E-M2-ASUS-4210.bin`)
5. Execute in order:
   - **Erase** the chip
   - **Program** the new BIOS
   - **Verify** the programming
6. Reinstall BIOS chip on motherboard

## Final Steps
1. Reassemble all components
2. Power on the system
3. Enter BIOS setup to configure settings
4. The TPM should now be cleared and ready for new configuration

## ⚠️ Important Notes
- Backup important data before starting
- Ensure stable power supply during flashing
- Follow ESD precautions when handling components
- This process voids warranty
- Incorrect flashing can brick your motherboard

## Success Indicators
- System boots normally after final BIOS programming
- TPM can be reconfigured in Windows
- No security-related boot errors

## Troubleshooting
- If system doesn't boot: double-check BIOS chip orientation
- If TPM errors persist: repeat the entire process
- For black screen issues: verify BIOS file compatibility

---

**Disclaimer**: This guide is for educational purposes. Perform these steps at your own risk. The author is not responsible for any damage to hardware.
