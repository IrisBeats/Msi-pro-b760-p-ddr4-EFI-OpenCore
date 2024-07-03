

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

### **Before You Start**

* Ensure your hardware is compatible.
* Update your BIOS to version `>= 7D98v18`.
* Read the [OpenCore Desktop Guide](https://dortania.github.io/OpenCore-Install-Guide/prerequisites.html).

## Installation

### BIOS Settings

* **Advanced** → D.T.M → [**Enabled**]
* **Security** → Secure Boot → [**Disabled**]
* **Advanced** → PCI Configuration → Above 4G Decoding → [**Enabled**]
* **Advanced** → PCI Configuration → Re-Size BAR Support → [**Disabled**]
* **Advanced** → Storage Configuration → SATA Mode → [**AHCI**]
* **WARNING** → XHCI Hand-off → [**Enabled**]
* **Boot** → Fast Boot → [**Disabled**]
* **Boot** → MSI Fast Boot → [**Disabled**]

### XMP

You can enable XMP if your memory supports it.

## Gathering Files

- You must download all non-bundled kexts and drivers from their repositories yourself.

### ACPI

- [SSDT-AWAC.aml]
- [SSDT-EC.aml]
- [SSDT-EC-USBX.aml]
- [SSDT-GPRW.aml]
- [SSDT-PLUG-ALT.aml]
- [SSDT-PLUT-ALT.dsl]
- [SSDT-USB-RESET.aml]

### EFI Drivers

* [HfsPlus.efi][7] - Needed for seeing HFS volumes (i.e., macOS installers and recovery partitions/images).
* OpenRuntime.efi - Required for working with native NVRAM.
* OpenCanopy.efi - For [OpenCore's GUI][25].

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

### Before Installation

Generate a serial number for your Mac with this tool:  
https://github.com/corpnewt/GenSMBIOS

### Config Property List

Please check `Config Example\config.plist` for a post-install config example.

### After Installation

**Boot from Disk**  
Transfer your EFI folder from your USB stick to your disk for USB-free booting with this tool:  
https://www.olarila.com/files/Utils/ESP%20Mounter%20Pro.app_v1.9.1.zip

**Map Your USB**  
Map your USB ports with this tool:  
https://github.com/USBToolBox/tool/releases

**Set Audio with ALC897 Boot Args**

- Add `boot-args`:
  - Under `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`
  - Add `agdpmod=pikera e1000=0 keepsyms=1 -ctrsmt alcid=11` to remove verbose mode and add audio support.

### Platform Information

- Populate the `PlatformInfo > Generic` section in `config.plist`. This can be easily done with `GenSMBIOS`. Follow the [OpenCore Desktop Guide][23].
- For Navi20 dGPUs to work properly, use the `iMacPro7,1` SMBIOS.

### Post-Installation

- **Misc -> Boot**
  - Set `PickerMode` to `External` and add files from [Setting up OpenCore's GUI][26].
- **Misc -> Security**
  - Set `ScanPolicy` to `0` for dual booting. Refer to the [ScanPolicy Docs][24].
- **NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args**:
  - Remove `-v` from your config.plist.
- **NVRAM -> Add -> D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14 -> UIScale**:
  - One-byte data defining boot.efi user interface scaling. Use `01` for normal screens and `02` for HiDPI screens. (Set to `02` for Dell P2418D).

### Troubleshooting

- If you encounter any problems, search for `xhciportlimits` in config.plist and disable it.

---
