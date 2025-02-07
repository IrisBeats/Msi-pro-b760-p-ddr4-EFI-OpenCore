

# Hackintosh on MSI Pro B760-P with AMD RX 6800 via [OpenCore]

### **Hardware Configuration**

* I7 12700KF
* MSI Pro B760-P DDR4
  * Audio: Realtek ALC897
  * LAN: Realtek® RTL8125BG 2.5G LAN
* Corsair Vengeance RS RGB 4x16GB 3200MHz
* XFX AMD Radeon RX 6800 16GB
* Samsung SSD 970 EVO PLUS 1TB M.2 (Windows 10)
* Crucial P3 1TB (macOS Monterey)
* [Wi-Fi and Bluetooth don't work]
* IIyama PL3480WQ: Primary screen, 3440x1440 at 180Hz (DP)
* IIyama PL2770H: Secondary screen, 1920x1080 at 120Hz (HDMI to DP)
* Acer P236HL: Tertiary screen, 1920x1080 at 60Hz (HDMI)

---

### **Before You Start**

* Ensure your hardware is compatible.
* Update your BIOS to version `>= 7D98v18`.
* Read the [OpenCore Desktop Guide](https://dortania.github.io/OpenCore-Install-Guide/prerequisites.html).

---

## Installation

---

* **Install macOS Monterey**
  - [Download from here](https://www.olarila.com/topic/6278-olarila-vanilla-images-macos-installer/)

* **Burn your RAW file to your USB with Balena Etcher**
  - [Download from here](https://etcher.balena.io/)

* **Make your EFI USB partition visible with MiniTool Partition Wizard**
  - [Download the free version](https://www.partitionwizard.com/)

* **Paste the EFI folder using Explorer++**
  - [Download from here](https://explorerplusplus.com/download)

* **For a tutorial, follow this video**
  - [Watch here](https://www.youtube.com/watch?v=BfcdklKjvY4)

---
### Platform Information

- Populate the `PlatformInfo > Generic` section in `config.plist`. This can be easily done with `GenSMBIOS`. Follow the [OpenCore Desktop Guide][23].
- For Navi20 dGPUs to work properly, use the `iMacPro7,1` SMBIOS.

### Post-Installation

- **Misc -> Boot**
  - Set `PickerMode` to `External` and add files from [Setting up OpenCore's GUI].
- **Misc -> Security**
  - Set `ScanPolicy` to `0` for dual booting. Refer to the [ScanPolicy Docs].
- **NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args**:
  - Add `-v agdpmod=pikera e1000=0 keepsyms=1 debug=0x100 -ctrsmt alcid=11` from your config.plist.
    
### Troubleshooting

- If you encounter any problems, search for `xhciportlimits` in config.plist and disable it.

---
### BIOS Settings

* **Advanced** → D.T.M → [**Enabled**]
* **Security** → Secure Boot → [**Disabled**]
* **Advanced** → PCI Configuration → Above 4G Decoding → [**Enabled**]
* **Advanced** → PCI Configuration → Re-Size BAR Support → [**Disabled**]
* **Advanced** → Storage Configuration → SATA Mode → [**AHCI**]
* **WARNING** → XHCI Hand-off → [**Enabled**]
* **Boot** → Fast Boot → [**Disabled**]
* **Boot** → MSI Fast Boot → [**Disabled**]

---

### XMP

You can enable XMP if your memory supports it.

  
---

### ACPI

- [SSDT-AWAC.aml]
- [SSDT-EC.aml]
- [SSDT-EC-USBX.aml]
- [SSDT-GPRW.aml]
- [SSDT-PLUG-ALT.aml]
- [SSDT-PLUT-ALT.dsl]
- [SSDT-USB-RESET.aml]
  
---

### EFI Drivers

* [HfsPlus.efi] - Needed for seeing HFS volumes (i.e., macOS installers and recovery partitions/images).
* OpenRuntime.efi - Required for working with native NVRAM.
* OpenCanopy.efi - For [OpenCore's GUI].
  
---

### Kexts

* **[AirportItlwm.kext]** - Enables Wi-Fi functionality for Intel wireless cards.
* **[AppleALC.kext]** - Makes getting audio to work as easy as possible.
* **[CPUFriend.kext]** - Adjusts CPU power management settings.
* **[CPUFriendDataProvider.kext]** - Provides data to CPUFriend.kext for fine-tuning power management.
* **[Lilu.kext]** - Dependency for VirtualSMC.kext and WhateverGreen.kext, essential for many system modifications.
* **[LucyRTL8125Ethernet.kext]** - Provides a driver for Realtek RTL8125 Ethernet cards.
* **[RadeonSensor.kext]** - Monitors temperature and other sensors on Radeon GPUs.
* **[RestrictEvents.kext]** - Prevents certain system events from causing issues with macOS.
* **[SMCProcessor.kext]** - Monitors CPU sensors via SMC.
* **[SMCRadeonGPU.kext]** - Monitors sensors on Radeon GPUs via SMC.
* **[SMCSuperIO.kext]** - Adds support for various hardware monitoring sensors via SMC.

---

### Before Installation

Generate a serial number for your Mac with this tool:  
- [Download from here](https://github.com/corpnewt/GenSMBIOS)

---

### Config Property List

Please check `Config Example\config.plist` for a post-install config example.

---

### After Installation

**Boot from Disk**  
Transfer your EFI folder from your USB stick to your disk for USB-free booting with this tool:  
- [Download from here](https://www.olarila.com/files/Utils/ESP%20Mounter%20Pro.app_v1.9.1.zip)

---

**Map Your USB**  
Map your USB ports with this tool:  
- [Download from here](https://github.com/USBToolBox/tool/releases)

---

**Set Audio with ALC897 Boot Args**

- Add `boot-args`:
  - Under `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`
  - Change to `agdpmod=pikera e1000=0 keepsyms=1 -ctrsmt alcid=11` to remove verbose mode and add audio support.


