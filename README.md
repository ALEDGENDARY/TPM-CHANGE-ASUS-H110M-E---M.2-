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



---

# BIOS Recovery & TPM Clearing Guide for ASUS H110M-E M.2

## 📋 Overview
This guide documents the process of intentionally bricking and recovering a BIOS chip to clear TPM information and hardware IDs. We'll use a CH431A programmer with NeoProgrammer software to restore the BIOS to the latest version.

## ⚠️ **Critical Warning**
**This process will temporarily brick your motherboard** by downgrading to the earliest BIOS version, resulting in a black screen. Proceed only if you have the proper hardware and technical expertise.

---

## 🛠️ **Tools & Materials Required**

### **Hardware:**
- ASUS H110M-E M.2 Motherboard
- CH341A/CH431A BIOS Programmer
- USB flash drive (4GB+)
- BIOS chip removal tools
- Laptop/PC for programming

### **Software:**
- Rufus (for FreeDOS bootable USB)
- NeoProgrammer
- AFUDOS.exe
- UEFITool
- BIOS files:
  - Lowest version: `H110M-E-M2-ASUS-0404.rom`
  - Latest version: `H110M-E-M2-ASUS-4210.bin`

---

## 📸 **Visual Guide**

### **Step 1: Flashing Bios using FreeDOS**
![ACTUAL USB FREE DOS SPOOFING TO BRICKED MY BIOS](ACTUAL%20USB%20FREE%20DOS%20SPOOFING%20TO%20BRICKED%20MY%20BIOS.jpeg)

**What you're seeing:**  
When booting, **select the USB name WITHOUT "UEFI"** to ensure legacy boot mode.

### **Step 2: Removing the BIOS Chip**
![REMOVE BIOS AND PUT IT TO CH431A PROGRAMMER](REMOVE%20BIOS%20AND%20PUT%20IT%20TO%20CH431A%20PROGRAMMER.jpeg)

**What you're seeing:**  
The BIOS chip carefully removed from the motherboard and placed in the CH431A programmer socket. The background shows the motherboard with the empty BIOS chip location.

### **Step 3: Connecting the Programmer**
![CONNECT CH431A TO LAPTOP TO REPROGRAM BIOS](CONNECT%20CH431A%20TO%20LAPTOP%20TO%20REPROGRAM%20BIOS.jpeg)

**What you're seeing:**  
The CH431A programmer connected to a laptop, ready to run NeoProgrammer software for BIOS chip reprogramming.

### **Step 4: Reprogramming the BIOS**
![NEOPROGRAMMER SOFTWARE](NEOPROGRAMMER%20SOFTWARE.png)

**What you're seeing:**  
NeoProgrammer software interface showing the process of reprogramming the bricked BIOS chip. We use this to flash the latest BIOS version after intentionally breaking the old one to clear TPM and hardware IDs.

---

## 🔄 **Process Flow**

1. **Create FreeDOS USB** → Flash earliest BIOS (causes intentional brick)
2. **Remove BIOS chip** → Extract from motherboard
3. **Connect to CH431A** → Hook up to laptop/PC
4. **Use NeoProgrammer** → Flash latest BIOS version
5. **Reinstall chip** → Test boot with cleared TPM

---

## ⚡ **Quick Steps Summary**

1. **Prepare USB** with FreeDOS and AFUDOS using Rufus
2. **Boot from USB** (select non-UEFI option)
3. **Flash oldest BIOS** → System bricks (expected)
4. **Remove BIOS chip** from motherboard
5. **Connect to CH431A programmer**
6. **Program with NeoProgrammer** using latest BIOS
7. **Reinstall chip** → Boot with cleared TPM

---

## 💡 **Why This Works**
By forcing a downgrade to the earliest BIOS version, we trigger a complete TPM and hardware ID reset. The physical reprogramming then restores full functionality with the latest firmware, effectively wiping all previous security data.

---

## ⚠️ **Final Notes**
- **Backup everything** before starting
- **Ensure stable power** during programming
- **Handle chips carefully** to avoid static damage
- **This voids warranty** - proceed at your own risk
- **Test thoroughly** after completion

---

*This guide is for educational purposes. Always follow proper ESD precautions and understand the risks involved in hardware modification.*

