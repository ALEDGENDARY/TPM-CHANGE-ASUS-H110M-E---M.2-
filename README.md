
## For ASUS H110M-E M.2 Motherboards with Integrated TPM

---

## 📋 **Executive Summary**

This guide provides a systematic approach to resetting hardware identity components (TPM 2.0/PTT and storage identifiers) for ASUS H110M-E M.2 motherboards. The process involves:
1. **Software TPM clearance** through Windows and BIOS
2. **Aggressive BIOS reprogramming** to reset firmware-based identifiers
3. **Optional NVMe SSD serial number modification** for complete hardware anonymity

**⚠️ IMPORTANT:** This process is designed for legitimate ownership transfer, hardware troubleshooting, or educational purposes only. Misuse may violate software license agreements or terms of service.

---

## 🎯 **Objective**

Reset the following hardware identifiers to factory defaults:
- **Intel Platform Trust Technology (PTT) / fTPM state**
- **Motherboard UUID and SMBIOS data**
- **NVMe SSD serial number and identifiers**
- **Secure Boot keys and NVRAM settings**

---

## 📦 **Required Materials**

### **Hardware Components:**
| Item | Purpose | Approx. Cost |
|------|---------|-------------|
| CH341A BIOS Programmer | External BIOS chip programming | ₱120 |
| USB 2.0/3.0 Drive (4GB+) | FreeDOS boot medium | ₱200 |
| Fanxiang S500Pro 256GB NVMe SSD | Target storage for spoofing | ₱1,500 |
| M.2 NVMe to USB Adapter | SSD programming interface | ₱130 |
| BIOS Chip Removal Tools | SOP-8 chip extractor | ₱150 |
| **Total Estimated Cost:** | | **₱2,100** |

### **Software Tools:**
| Software | Purpose | Source |
|----------|---------|--------|
| Rufus 3.2+ | Bootable USB creation | rufus.ie |
| NeoProgrammer 2.2.0.10 | BIOS chip programming | Github |
| UEFITool | BIOS file manipulation | Github |
| AFUDOS Utility | BIOS flashing utility | ASUS |
| map1202too | NVMe serial number modification | Github |
| TPM Spoof Tools | Additional TPM utilities | Github |

---

## 📖 **Process Overview**

### **Phase 1: Software Preparation & Initial Clearing**
### **Phase 2: BIOS Identity Reset via Hardware Reprogramming**
### **Phase 3: Storage Device Identifier Modification**
### **Phase 4: System Validation & Testing**

---

## 🔧 **Phase 1: Initial Software Clearing**

### **Step 1.1: Windows TPM Management**
1. Open **Run Dialog** (`Win + R`)
2. Type `tpm.msc` and press Enter
3. In the TPM Management Console:
   - Select **"Clear TPM..."** in the right-hand Actions pane
   - Choose **"Restart now"** when prompted
   - The system will reboot and clear the TPM/PTT module

### **Step 1.2: BIOS/UEFI Configuration Reset**
1. Enter BIOS Setup (`Del` key during boot)
2. Navigate to **Security → Trusted Computing**
3. Select **"Clear TPM"** or **"Reset TPM"**
4. Navigate to **Boot → Secure Boot**
5. Select **"Restore Factory Keys"** or **"Clear All Secure Boot Keys"**
6. Save changes and exit (`F10`)

### **Step 1.3: CMOS Clear (Physical Reset)**
1. Power off the system and disconnect power cord
2. Locate the CMOS battery on motherboard
3. Remove battery for 5 minutes
4. Short the CMOS jumper pins (CLRTC) for 10 seconds
5. Reinstall battery and reconnect power

---

## 🧩 **Phase 2: BIOS Identity Reset via Hardware Reprogramming**

### **Step 2.1: BIOS Files Preparation**
1. Download official BIOS files from ASUS:
   - Initial: `H110M-E-M2-ASUS-0404.CAP` (Oldest incompatible)
   - Target: `H110M-E-M2-ASUS-4210.CAP` (Latest stable)
   
2. Convert files using UEFITool:
   ```bash
   # Extract ROM from CAP files
   UEFITool → Open CAP → Extract body → Save as .ROM
   
   # Convert latest version for programmer
   H110M-E-M2-ASUS-4210.CAP → H110M-E-M2-ASUS-4210.BIN
   ```

### **Step 2.2: Create FreeDOS Bootable USB**
1. Insert USB drive into working computer
2. Open Rufus with Administrator privileges
3. Configure settings:
   - Device: [Your USB Drive]
   - Boot selection: FreeDOS
   - Partition scheme: MBR
   - File system: FAT32
4. Click **Start** and confirm formatting

### **Step 2.3: Prepare BIOS Flashing Files**
1. Copy these files to USB root directory:
   - `AFUDOS.exe` (BIOS flashing utility)
   - `BIOS.ROM` (converted from 0404 version)
   - `AUTOEXEC.BAT` (automatic execution script)

2. Create `AUTOEXEC.BAT` with this content:
```batch
@echo off
cls
echo BIOS Downgrade in Progress...
echo DO NOT POWER OFF!
afudos.exe BIOS.ROM /GAN /N
echo Flashing complete. System will power off.
```

### **Step 2.4: Execute BIOS Downgrade**
1. Insert prepared USB into target system
2. Boot system and enter Boot Menu (`F8`)
3. **Critical**: Select USB device **WITHOUT "UEFI" prefix**
4. System will automatically:
   - Boot into FreeDOS
   - Execute AFUDOS with downgrade BIOS
   - Complete flashing process
   - Result: **Black Screen (Expected)**

### **Step 2.5: Physical BIOS Chip Reprogramming**
1. **Power off and disconnect all cables**
2. **Extract BIOS chip** using SOP-8 extractor tool:
   - Identify chip location (near CMOS battery)
   - Gently lift with extractor tool
   - Avoid bending pins

3. **Connect to CH341A Programmer:**
   - Align chip with pin 1 marker
   - Lock chip securely in socket
   - Connect USB to laptop

4. **Program with NeoProgrammer:**
```
[Software Interface Process]
1. Open NeoProgrammer
2. Select Chip: Winbond 25Q64FV (8MB)
3. Click "Detect"
4. Click "Read" (optional backup)
5. Click "Erase" (full chip erase)
6. Load File: H110M-E-M2-ASUS-4210.BIN
7. Click "Program"
8. Click "Verify"
9. Status: "Verification Successful"
```

5. **Reinstall BIOS Chip:**
   - Align pin 1 with motherboard marking
   - Gently press into socket
   - Ensure all pins are seated

---

## 💾 **Phase 3: Storage Device Identifier Modification**

### **Step 3.1: NVMe SSD Programming Setup**
1. Connect NVMe SSD to USB adapter
2. Insert adapter into working computer
3. Open Command Prompt as Administrator

### **Step 3.2: Identify SSD Parameters**
```bash
# Using map1202too tool
map1202too.exe -i

# Expected output:
# Device: Fanxiang S500Pro 256GB
# Serial: xxxxxxxxxxxx
# Model: FSxxxxxxxx
# Firmware: xxxxxxxx
```

### **Step 3.3: Modify Serial Number**
```bash
# Generate random serial number
# Format: 12 alphanumeric characters

# Example command structure:
map1202too.exe -s [NEW_SERIAL] -m [NEW_MODEL] -f [NEW_FIRMWARE]

# Practical example:
map1202too.exe -s F0A1B2C3D4E5 -m FANXIANG256 -f FSA001
```

### **Step 3.4: Verification**
```bash
# Confirm changes
map1202too.exe -i

# Expected result shows new identifiers
```

---

## ✅ **Phase 4: System Validation**

### **Step 4.1: Initial Boot Test**
1. Reassemble all components
2. Connect minimal peripherals
3. Power on system
4. Observe:
   - POST completion
   - BIOS version display
   - Boot device detection

### **Step 4.2: BIOS Configuration**
1. Enter BIOS Setup (`Del`)
2. Check:
   - **Main → System Information**: Verify new BIOS version
   - **Security → TPM Configuration**: Should show "Available" or "Enabled"
   - **Boot → Secure Boot**: Should be "Disabled" or "Setup Mode"

3. Configure optimal settings:
   - Load Optimized Defaults
   - Enable XMP for RAM (if applicable)
   - Configure boot order
   - Save and Exit (`F10`)

### **Step 4.3: Windows Installation & Verification**
1. Install fresh Windows 11/10
2. During installation:
   - Skip product key entry
   - Choose "I don't have internet" for local account
   - Create offline administrator account

3. Post-installation checks:
```powershell
# Check TPM status
Get-Tpm

# Check motherboard information
wmic baseboard get product,Manufacturer,version,serialnumber

# Check disk information
wmic diskdrive get serialnumber,model

# Verify system uniqueness
powershell "Get-WmiObject Win32_ComputerSystemProduct | Select-Object UUID"
```

### **Step 4.4: Functional Testing**
1. **Windows Activation Test:**
   - Check Activation status
   - Attempt digital license transfer

2. **Software License Validation:**
   - Install licensed software
   - Verify new hardware detection

3. **Performance Testing:**
   - Run stress tests
   - Verify stability
   - Check temperatures

---

## 📊 **Expected Results & Verification Matrix**

| Component | Before Procedure | After Procedure | Verification Method |
|-----------|-----------------|-----------------|-------------------|
| TPM/PTT State | Owned/Cleared | Factory Default | `tpm.msc` |
| Motherboard UUID | Original Serial | New/Generic | `wmic baseboard` |
| BIOS Version | Latest (4210) | Latest (4210) | BIOS Splash Screen |
| Secure Boot | Custom Keys | Factory Default | BIOS Setup |
| SSD Serial | Original | Modified | `map1202too -i` |
| Windows Activation | Linked to old HW | Not Activated | Settings → Activation |

---

## ⚠️ **Critical Considerations & Warnings**

### **Legal & Ethical Implications:**
1. **Software Licensing**: Resetting hardware identifiers may violate software license agreements
2. **Warranty Voidance**: Physical BIOS chip removal voids manufacturer warranty
3. **Regulatory Compliance**: Some jurisdictions restrict hardware modification
4. **Ethical Use**: Intended for legitimate ownership transfer only

### **Technical Risks:**
1. **Permanent Damage**: Incorrect chip handling can destroy motherboard
2. **Boot Failure**: Improper programming can brick system
3. **Data Loss**: All data on target SSD will be erased
4. **Compatibility Issues**: Modified hardware may have driver conflicts

### **Safety Precautions:**
1. **ESD Protection**: Use anti-static wrist strap and mat
2. **Power Safety**: Disconnect all power sources before hardware work
3. **Backup Priority**: Backup existing BIOS before modification
4. **Tool Quality**: Use proper tools to avoid physical damage

---

## 🆘 **Troubleshooting Guide**

### **Common Issues & Solutions:**

| Problem | Possible Cause | Solution |
|---------|---------------|----------|
| Black screen after USB flash | Expected behavior | Proceed to chip programming |
| CH341A not detected | Driver issues | Install correct USB drivers |
| Programming fails | Chip not detected | Check chip orientation/socket |
| System won't POST | BIOS chip issue | Re-seat chip, verify programming |
| Windows activation fails | Hardware hash persists | Repeat Phase 2 completely |

### **Recovery Procedures:**
1. **BIOS Recovery Mode**: Some ASUS boards support USB BIOS Flashback
2. **Secondary BIOS Chip**: If available, switch to backup chip
3. **Professional Repair**: Contact motherboard repair service if needed

---

## 📈 **Success Metrics**

The procedure is successful when:
1. ✅ System boots to Windows without errors
2. ✅ TPM shows as "Ready" or "Ownable" state
3. ✅ Windows Activation shows "Not Activated"
4. ✅ Software licenses recognize as "new hardware"
5. ✅ System passes 24-hour stability test
6. ✅ All modified components report new identifiers

---

## 🔍 **Advanced Notes for Technical Users**

### **Understanding the Technical Basis:**
1. **Intel PTT Architecture**: Firmware-based TPM 2.0 implementation stored in SPI flash
2. **SMBIOS Structure**: DMI data stored in flash, modifiable via programming
3. **NVMe Identify Controller**: Serial number field in controller accessible via vendor commands
4. **Hardware Hash Generation**: Windows uses multiple hardware measurements for activation

### **Alternative Approaches:**
1. **Intel Flash Programming Tool (FPT)**: Can modify ME region if accessible
2. **BIOS Modding**: Direct modification of SMBIOS tables in BIOS file
3. **Hardware Emulation**: Virtual TPM solutions for testing

### **Limitations:**
1. **MAC Addresses**: Network controller identifiers remain unchanged
2. **Intel ME Region**: May retain unique identifiers if not fully cleared
3. **Other Components**: GPU, CPU, RAM have their own identifiers

---

## 📚 **References & Further Reading**

1. **ASUS H110M-E M.2 Technical Documentation**
2. **Intel Platform Trust Technology SDK**
3. **TPM 2.0 Specification** (Trusted Computing Group)
4. **NVMe Base Specification 1.4** (Serial Number modification)
5. **UEFI Specification 2.9** (SMBIOS structure)

---

## 📞 **Support & Disclaimer**

### **Support Channels:**
- ASUS Technical Support: Official manufacturer support
- Hardware Forums: Community troubleshooting assistance
- Professional Services: Certified repair technicians

### **Legal Disclaimer:**
```
This guide is provided for EDUCATIONAL PURPOSES ONLY. The author assumes 
NO RESPONSIBILITY for any damage, data loss, legal issues, or warranty 
voidance resulting from following this procedure. Users assume all risks 
associated with hardware modification.

This procedure may violate software license agreements, terms of service, 
or local regulations. Consult with legal counsel before proceeding if 
uncertain about compliance requirements.

By proceeding, you acknowledge understanding and acceptance of all risks 
and responsibilities associated with this hardware modification process.
```

---

## 🎯 **Quick-Start Checklist**

- [ ] Backup all important data
- [ ] Gather all required tools and components
- [ ] Download and verify BIOS files
- [ ] Clear TPM via Windows and BIOS
- [ ] Create FreeDOS USB with AFUDOS
- [ ] Perform BIOS downgrade (expect black screen)
- [ ] Extract and program BIOS chip with CH341A
- [ ] Modify NVMe serial number with map1202too
- [ ] Reassemble and test system
- [ ] Verify all identifiers changed
- [ ] Perform stability testing

---

**Document Version:** 1.0  
**Last Updated:** October 2024  
**Applicability:** ASUS H110M-E M.2 Motherboards  
**Complexity Level:** Advanced/Technical
