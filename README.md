# Hackintosh on Msi Pro B760-P with AMD RX 6800 via [OpenCore]



### **Hardware configuration**

* I7 12700kf
* Msi Pro B760-P DDR4
  * Audio: Realtek ALC897
  * LAN: 	Realtek® RTL8125BG 2.5G LAN
* Corsair Vengeance RS RGB 4x16go 3200MHZ 
* XFX AMD Radeon RX 6800 16G
* Samsung SSD 970 EVO PLUS 1TB m.2 (WINDOWS 10)
* Crucial P3 1TB (MacOs Monterey)
* [Wifi and bluetooth Don't work)
* IIyama PL3480WQ 1st screen work in 180hz 3440x1440 (DP)
* IIyama PL2770H 2nd screen work in 120hz 1920x1080 (hdmi to DP)
* Acer P236HL work in 1920x1080 (60hz) (hdmi)
  

### **Before you start make sure you have**

* Working hardware
* [BIOS] version `>= 7D98v18`
* Read [OpenCore Desktop Guide]
* `https://dortania.github.io/OpenCore-Install-Guide/prerequisites.html`


## Installation

### BIOS Settings

* *Advanced* → D.T.M  [**Enabled**]
* *Security* → Secure boot → [**Disabled**]
* *Advanced* → PCI Configuration → Above 4G Decoding [**Enabled**]
* *Advanced* → PCI Configuration → Re-Size BAR Support [**Disabled**]
* *Advanced* → Storage Configuration → SATA Mode [**AHCI**]
* *WARNING* → xhci hand off need to be enabled →  [**Disabled**]
* *Boot* → Fast Boot [**Disabled**]
* *Boot* → Msi Fast Boot [**Disabled**]
  
### XMP

You can enable XMP if your memory supports it.

## Gathering files

- You must download all not bundled kexts and drivers from repositories by yourself.

### ACPI

- [SSDT-AWAC.aml]
- [SSDT-EC.aml]
- [SSDT-EC-USBX.aml]
- [SSDT-GPRW.aml]
- [SSDT-PLUG-ALT.aml]
- [SSDT-PLUT-ALT.dsl]
- [SSDT-USB-RESET.aml]

### EFI drivers

* [HfsPlus.efi][7] - Needed for seeing HFS volumes(ie. macOS Installers and Recovery partitions/images).
* OpenRuntime.efi - Must have to work with native NVRAM
* OpenCanopy.efi - For [OpenCore's GUI][25]

### Kexts
*[AirportItlwm.kext] - Enables Wi-Fi functionality for Intel wireless cards.
*[AppleALC.kext] - Makes getting audio to work as easy as possible.
*[CPUFriend.kext] - Adjusts CPU power management settings.
*[CPUFriendDataProvider.kext] - Provides data to CPUFriend.kext for fine-tuning power management.
*[Lilu.kext] - Dependency for VirtualSMC.kext and WhateverGreen.kext, essential for many system modifications.
*[LucyRTL8125Ethernet.kext] - Provides a driver for Realtek RTL8125 Ethernet cards.
*[RadeonSensor.kext] - Monitors temperature and other sensors on Radeon GPUs.
*[RestrictEvents.kext] - Prevents certain system events from causing issues with macOS.
*[SMCProcessor.kext] - Monitors CPU sensors via SMC.
*[SMCRadeonGPU.kext] - Monitors sensors on Radeon GPUs via SMC.
*[SMCSuperIO.kext] - Adds support for various hardware monitoring sensors via SMC.



-----


### Before-Install
generate a serial number for ur mac with this tool
https://github.com/corpnewt/GenSMBIOS

### Config Property list

Please check `Config Example\config.plist` for post-install config example.

#### After-Install

**boot from disk**
transfer your efi folder from your usb stick to your disk for usb-free booting 
with this tool
https://www.olarila.com/files/Utils/ESP%20Mounter%20Pro.app_v1.9.1.zip

**Map Ur USB**
- map ur usb with 
https://github.com/USBToolBox/tool/releases


**Set audio with ALC897 boot args**

- Add `boot-args` :
  - Under `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`
  - Add ` agdpmod=pikera e1000=0 keepsyms=1 -ctrsmt alcid=11` For remove verbose mode and add ur audio
    
**Platfom information**

- Populated `PlatformInfo > Generic` section in `config.plist`, can be easily done with `GenSMBIOS` please follow [OpenCore Desktop Guide][23].
- For Navi20 dGPU will work properly, We must use `iMacPro1,1` SMBIOS.

#### Post-Install 

- `Misc -> Boot`
  - Set `PickerMode` as `External` and add files from [Setting up OpenCore's GUI][26]
- `Misc -> Security`
  - Set `ScanPolicy` to `0` - for dual boot, [Scanpolicy Docs][24]
- `NVRAM -> Add -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot-args`:
  - Remove `-v` from your config.plist
- `NVRAM -> Add -> D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14 -> UIScale`:
  - One-byte data defining boot.efi user interface scaling. Should be `01` for normal screens and `02` for HiDPI screens. (When using Dell P2418D set it to `02`)
  ####  if u have any problems
 -  search in config.plist `xhciportlimits` and disable it
   
