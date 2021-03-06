# T480-OpenCore-Hackintosh-Thunderbolt

<img align="right" src="https://github.com/EETagent/T480-OpenCore-Hackintosh/raw/master/Other/README_Resources/ThinkPad.gif" alt="T480 macOS" width="430">

[![OpenCore](https://img.shields.io/badge/OpenCore-0.8.0-lightblue.svg)](https://github.com/acidanthera/OpenCorePkg)
[![macOS-Stable](https://img.shields.io/badge/macOS-12.4-purple.svg)](https://www.apple.com/macos/monterey/)
[![Windows-Stable](https://img.shields.io/badge/Windows-11-blue.svg)](https://www.microsoft.com/en-us/windows)

## Introduction

<details>
<summary><strong>Hardware</strong></summary>
<br>


[![UEFI](https://img.shields.io/badge/UEFI-N24ET61W-lightgrey)](https://pcsupport.lenovo.com/us/en/products/laptops-and-netbooks/thinkpad-t-series-laptops/thinkpad-t480-type-20l5-20l6/downloads/ds502355)

| Category  | Component                         | Note                                                         |
| --------- | --------------------------------- | ------------------------------------------------------------ |
| CPU       | Intel Core i7-8550U               | 20L50000MC                                                   |
| GPU       | Intel UHD 620                     |                                                              |
| SSD       | Western Digital 970 SN750 500GB   | Replaced cursed PM 981 which stil doesn't work reliably      |
| Memory    | 8+16GB DDR4 2400Mhz               |                                                              |
| Battery   | Dual battery                      |                                                              |
| Camera    | 720p Camera                       |                                                              |
| Wifi & BT | BCM1820A                          |                                                              |
| Input     | PS2 Keyboard & Synaptics TrackPad | [YogaSMC](https://github.com/zhen-zen/YogaSMC) for media keys like microphone switch, etc. PrtSc is mapped as F13. |

</details>  

<details>

<summary><strong>Main software</strong></summary>
<br>

| Component      | Version       |
| -------------- | ------------- |
| macOS Monterey | 12.4 (21F79) |
| Windows 11     | 21H2          |
| OpenCore       | v0.8.0        |

</details>

<details>

<summary><strong>Kernel extensions</strong></summary>
<br>

| Kext                  | Version        |
| :-------------------- | -------------- |
| itlwm.                | 2.2.0          |
| AppleALC              | 1.7.1          |
| BrightnessKeys        | 1.0.2          |
| CPUFriend             | 1.2.5          |
| HibernationFixup      | 1.4.5          |
| BlueToolFixup         | 2.6.1          |
| IntelMausi            | 1.0.8          |
| Lilu                  | 1.6.0          |
| NoTouchID             | 1.0.4          |
| RTCMemoryFixup        | 1.0.8          |
| VirtualSMC            | 1.2.9          |
| VoltageShift          | Disabled, 1.22 |
| VoodooPS2Controller   | 2.2.8          |
| VoodooRMI             | 1.3.4          |
| VoodooSMBus           | 3.0            |
| WhateverGreen         | 1.5.8          |
| YogaSMC               | 1.5.1          |

</details>

<details>

<summary><strong>UEFI drivers</strong></summary>
<br>

|     Driver      | Version           |
| :-------------: | ----------------- |
|  AudioDxe.efi   | OpenCorePkg 0.8.0 |
|   HfsPlus.efi   | OcBinaryData      |
| OpenCanopy.efi  | OpenCorePkg 0.8.0 |
| OpenRuntime.efi | OpenCorePkg 0.8.0 |

</details>

## Before installation

<details>  

<summary><strong>UEFI settings</strong></summary>
<br>

**Security**

- `Security Chip` **Disabled**
- `Memory Protection -> Execution Prevention` **Enabled**
- `Virtualization -> Intel Virtualization Technology` **Enabled**
- `Virtualization -> Intel VT-d Feature` **Enabled**
- `Anti-Theft -> Computrace -> Current Setting` **Disabled**
- `Secure Boot -> Secure Boot` **Disabled**
- `Intel SGX -> Intel SGX Control` **Disabled**
- `Device Guard` **Disabled**

**Startup**

- `UEFI/Legacy Boot` **UEFI Only**
- `CSM Support` **No**

**Thunderbolt**

- `Thunderbolt BIOS Assist Mode` **Disabled**
- `Wake by Thunderbolt(TM) 3` **Disabled**
- `Security Level` **User Authorization**
- `Support in Pre Boot Environment -> Thunderbolt(TM) device` **Enabled**

</details>  

<details>

<summary><strong>Own prev-lang-kbd</strong></summary>
<br>

Either add as a string or as a data ( HEX data [(ProperTree)](https://github.com/corpnewt/ProperTree) )

Format is lang-COUNTRY:keyboard

- 🇨🇳 | [252] en - ABC --> zh-Hans:252 --> 7A682D48 616E733A 323532
- 🇺🇸 | [0] en_US - U.S --> en-US:0 --> 656e2d55 533a30

etc.

[AppleKeyboardLayouts.txt](https://github.com/acidanthera/OpenCorePkg/blob/master/Utilities/AppleKeyboardLayouts/AppleKeyboardLayouts.txt)

</details>

<details>

<summary><strong>Secure Boot (Optional)</strong></summary>
<br>

1. Set Secure Boot to Setup Mode. Secure Boot should be reported as off by UEFI main tab
2. Create FAT32 formatted USB
3. Create EFI folder in the root of the newly formatted flash drive and move there content of SecureBoot/KeyTool
4. Boot flash drive via F12 boot menu
5. Choose **Edit keys**

<img src="https://github.com/EETagent/T480-OpenCore-Hackintosh/raw/master/Other/README_Resources/SecureBoot/MainMenu.png" alt="Main menu">

6. Start by **replacing** Signature Database. Select .auth file

<img src="https://github.com/EETagent/T480-OpenCore-Hackintosh/raw/master/Other/README_Resources/SecureBoot/ManipulateKey.png" alt="Select key to manipulate with">
<img src="https://github.com/EETagent/T480-OpenCore-Hackintosh/raw/master/Other/README_Resources/SecureBoot/SelectAuth.png" alt="Select .auth file">


7. Do the same for Key Exchange Keys Database (KEK) and Platform Key (PK) **in this order**
8. Exit and shutdown your machine
9. Boot into the UEFI settings and check if Secure Boot is reported as `on`
10. Boot you favorite OS with Secure Boot enabled

[More detailed information here](https://habr.com/en/post/273497)

```diff
! Still quite experimental
```

</details>

## Status

<details>  


<summary><strong>What's working ✅</strong></summary>

- [x] Battery percentage (please note the way both batteries are virtually combined might cause erroneous "service recommended" prompts)
- [x] Bluetooth - Stock Intel
- [x] Boot chime
- [x] Boot menu `OpenCanopy` 
- [x] CPU power management / performance `Now on par with Windows without XTU undervolt.`
- [x] FireVault 2 `No config.plist changes needed` 
- [x] GPU UHD 620 hardware acceleration / performance 
- [x] HDMI `Closed and opened lid. With audio.`
- [x] iMessage, FaceTime, App Store, iTunes Store. **Generate your own SMBIOS**
- [x] Intel I219V Ethernet port
- [x] Keyboard `Volume and brightness hotkeys. Another media keys with YogaSMC.`
- [x] Microphone `With keyboard switch using ThinkPad Assistant or YogaSMC app.`
- [x] Realtek® ALC3287 ("ALC257") Audio
- [x] SD card reader `Fortunately, USB connected.`
- [x] Sidecar wired `Works with 15,2 SMBIOS.`
- [x] Sleep/Wake 
- [x] TouchPad `1-5 fingers swipe works. Emulate force touch using longer and more voluminous touch.`
- [x] TrackPoint  `Works perfectly. Just like on Windows or Linux.`
- [x] USB Ports (including USB 3.1 10 Gbps on the front USB C port `USB Map is different for devices with Windows Hello camera.`
- [x] Web camera
- [x] Wifi - Stock Intel
- [x] DRM `Widevine, validated on Firefox 82. WhateverGreen's DRM is broken on Big Sur and up`
- [x] Windows 11 boot from OC boot menu
- [x] 120hz with a compatible LCD
- [x] Basic Thunderbolt functionality (see Thunderbolt notes below)
- [x] Thunderbolt hotplug
- [x] Thunderbolt wake from sleep
  <details>
    <summary><strong>Thunderbolt Details</strong></summary>
    Thunderbolt on the T480 is setup in an unusual way, devices are exposed to macOS as expresscard pcie devices. hotplug and wake from sleep work.
    Devices I have tested that work: AKiTiO Node + RX 6600, Elgato Thunderbolt 3 Dock (20DAA9902)
    Devices I have tested that do not work: Apple Thunderbolt 3 to Thunderbolt 2 adapter + Apple thunderbolt to ethernet adapter (A1433)
    
    Note that the physical limitations of the T480 mean that only the front most USB C port is Thunderbolt, and it is limited to 2 lanes of pcie.
  </details>

</details>  

<details>  

<summary><strong>What's not working ⚠️</strong></summary>

- [ ] Fingerprint reader  `There is finally after many years working driver for Linux (python-validity), don't expect macOS driver any time soon.`

- [ ] PM 981 `Still unstable. Get any other SSD`

- [ ] Sidecar wireless `If you want to use this feature, buy a compatible Broadcom card!`

</details>  



##  Credits

https://gitee.com/dhbxs/ThinkPad-T480-OpenCore-Hackintosh

https://github.com/EETagent/T480-OpenCore-Hackintosh

https://github.com/tylernguyen/x1c6-hackintosh

https://github.com/the-darkvoid/XPS9360-macOS

https://github.com/hexart/T480-OpenCore-Hackintosh

https://github.com/KirisameR/ThinkPad-X1-Yoga-3rd-Hackintosh
