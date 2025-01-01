# OpenCore-T470s

This repository is about OpenCore installation for macOS 12 (Monterey) on Lenovo Thinkpad T470s

## _Attention Please_ ⚠️

_Hackintoshing your computer is a risky task. This repository is just a reference guide for Lenovo Thinkpad T470s. I'm **NOT RESPONSIBLE** for any harm you cause to your computer(s)._

> **NOTE:** If you want to just copy-paste EFI still, I would recommend you to go through the [Opencore Installation Guide](https://dortania.github.io/OpenCore-Install-Guide).

## My Hardware Specs

| COMPONENT        | Model                             |
| ---------------- | --------------------------------- |
| CPU              | i7-6600U                          |
| GPU              | Intel Integrated GPU 520 HD       |
| Chipset          | Intel HM170 Chipset               |
| SSD              | Intel SSDPEKKF256G8L M.2 NVMe SSD |
| Wifi & Bluetooth | Intel Wireless Card 8260          |
| Ethernet         | Intel I219-LM                     |
| Audio Codec      | Realtek ALC298                    |
| Memory           | 8GB DDR4 2.40GHz                  |

## Prerequisites

- Basic Understanding of command line
- Python installed on your system
- An Internet Connection
- 4GB Pendrive
- Lenovo Thinkpad T470s :)
- No urgent work or some sort of deadlines

## Tools We Need

- [OpenCore Pkg](https://github.com/acidanthera/OpenCorePkg/releases) - Debug version is preffered
- [USBToolBox](https://github.com/USBToolBox/tool) (Optional) - Ports are already mapped
- [ProperTree](https://github.com/corpnewt/ProperTree)
- [GenSMBios](https://github.com/corpnewt/GenSMBIOS)

## Getting Started

To get started, we'll need to create an installation media for this, we'll need to download macOS .dmg and .chunklist files. so, follow these steps:

2. Extract the [OpenCore Pkg](https://github.com/acidanthera/OpenCorePkg/releases)
3. `cd` to OpenCore-[1.0.1]-DEBUG/Utilities/macrecovery
4. Run this command

```Bash
# Monterey (macOS 12)
py macrecovery.py -b Mac-FFE5EF870D7BA81A -m 00000000000000000 download
```

## Making the Installer using Windows

After the installation completes, we'll make the installation. For this guide, we'll be using windows for installation, if you prefer linux then go with this [link](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/linux-install.html#making-the-installer)

**NOTE:** if your USB is larger than 16GB use [rufus](https://github.com/pbatard/rufus/releases/download/v4.5/rufus-4.5.exe) method. Otherwise, use Windows Disk Management

### Method 1: Using Rufus

- Open Rufus
- Set `Device` to `YOUR USB DISK`
- Set `Boot Selection` to `Non Bootable`
- Set `Partition Scheme` to `FAT32`
- Set `Target System` to `UEFI`
- Click on `Start` button

### Method 2: Disk Managment

- Search `Create and format hard disk partition` using windows start menu
- Click on `Create and format hard disk partition`
- Select your USB Drive, right Click on it
- Click on `Format`

After the Disk is formated, create a new folder/direcotory `com.apple.recovery.boot` in the root of the USB drive. Now grab `BaseSystem.dmg` and `BaseSystem.chunklist` from `OpenCore/Utilties/macrecovery` and paste these 2 files into `com.apple.recovery.boot` folder of the USB.

Moreover, clone this repository and copy the EFI folder of this repo into the root of the USB partition. After all of these steps are done now your USB will look like this:

```
YOUR_USB_DRIVE
│--com.apple.recovery.boot
|   BaseSystem.dmg
|   BaseSystem.chunklist
|
│--EFI
|   |--BOOT
|   |--OC
```

and there you go, you've made the installer.

## Generating SMBIOS

We've successfully made the installer now it's time to generate SMBIOS for our thinkpad to have a working iMessage and facetime so, for this we'll follow the following steps:

1. Extract [GenSMBIOS](https://github.com/corpnewt/GenSMBios)
2. `cd` to GenSMBios
3. Click on `GenSMBios.bat`
4. Use option `3` to Generate SMBios
5. Type `MacBookPro13,3`
6. You'll be presented with information like this

```
Type:         MacBookPro13,3
Serial:       C02TJJYFGTFN
Board Serial: C02714501QXHCF9JC
SmUUID:       3C931CE2-77B8-440C-A4D2-F4AB6E751A34
Apple ROM:    CC29F51F8589
```

7. Extract [ProperTree](https://github.com/corpnewt/ProperTree)
8. `cd` to Propertree
9. Right click on `ProperTree.bat` and click on `Run as Administrator`
10. This will open the propertree, now click on `File -> Open`
11. Open `YOUR_USB_DRIVE/EFI/OC/config.plist`
12. Press `Crtl + Shift + r`
13. Go to `PlatformInfo` section in propertree

Put the values from the above list to Propertree

- `Type` Goes to `PlatformInfo -> Generic -> SystemProductName`
- `Apple ROM` Goes to `PlatformInfo -> Generic -> ROM` (Here you Can put your device Mac address as well)
- `Board Serial` Goes to `PlatformInfo -> Generic -> MLB`
- `SmUUID` Goes to `PlatformInfo -> Generic -> SystemUUID`
- `Serial` Goes to `PlatformInfo -> Generic -> SystemSerialNumber`

and now you'll have a working iMessage and facetime

## BIOS Settings

We must configure the BIOS to successfully boot & install macOS.

In Security menu, set the following settings:

    Security -> Security Chip: must be Disabled
    Memory Protection -> Execution Prevention: must be Enabled
    Virtualization -> Intel Virtualization Technology: must be Enabled
    Virtualization -> Intel VT-d Feature: must be Enabled
    Anti-Theft -> Computrace -> Current Setting: must be Disabled
    Secure Boot -> Secure Boot: must be Disabled
    Intel SGX -> Intel SGX Control: must be Disabled
    Device Guard: must be Disabled

In Startup menu, set the following options:

    UEFI/Legacy Boot: UEFI Only
    CSM Support: No

In Thunderbolt menu, set the following options:

    Thunderbolt BIOS Assist Mode: UEFI Only
    Wake by Thunderbolt(TM) 3: No
    Security Level: No
    Support in Pre Boot Environment > Thunderbolt(TM) device: No

And that's it, now go ahead and boot the USB drive and Enjoy!

## Tested Components ✅

- [x] Intel WiFi & Bluetooth (thanks to **itwlm**)
- [x] Brightness / Volume Control
- [x] Battery Information
- [x] USB Ports
- [x] Power management / Sleep
- [x] Graphics Acceleration
- [x] Trackpoint / Touchpad
- [x] Built-in Camera
- [x] Audio (Audio Jack & Speaker)
- [x] FaceTime / iMessage (iServices)
- [x] HDMI
- [x] SIP / FireVault 2

## Untested 🔄
- [ ] USB-C
- [ ] Sidecar (Cable) / AirPlay to Mac

## Not Working ❌
- [ ] Airdrop: Airdrop is not working because intel wireless cards doesn't support Airdrop

## TO-DOs 🗓️

- [ ] Upgrade To Venture/Sonoma

## Feedback & Support: 

if you find any bug or have some suggestions then don't hesitate to provide your feedback using the discussion tab. Moreover, if you think this repository is useful for you then don't forget to give it a star. Thanks

## Credits 

- [Open Core Guide](https://dortania.github.io/OpenCore-Install-Guide)
- [CorpNewt](https://github.com/corpnewt)


## License 
MIT - License
