# Dell Precision T5500 Hackintosh

[![OpenCore Version](https://img.shields.io/badge/OpenCore-0.9.3-green.svg)](https://github.com/qaziabdullah/EFI-Dell-Precision-T5500/)
[![GitHub issues](https://img.shields.io/github/issues/qaziabdullah/EFI-Dell-Precision-T5500.svg)](https://github.com/qaziabdullah/EFI-Dell-Precision-T5500/issues/)

### This repo contains an OpenCore EFI with fixes for the Dell Precision T5500.
#### This should also work on the T3500 but I haven't tested them. Try and let us know.

The EFI folder can also be used on older versions of macOS [tested on Ventura, Bigsur & Catalina]. However, the main support of this branch is based on Monterey since Ventura has a few issues that makes me keep Monterey as the main OS for now. On Ventura, my major issue is no rotation on extended display. 


Tested on:

Model | Precision T5500
------------- | ---------------
CPU | Intel Xeon X5675 x2
GPU | AMD Radeon RX 570 4GB (Sapphire) 
RAM | 32GB DDR3 1600Mhz
Storage | Samsung 870 EVO 1TB
Ethernet | Broadcom 5761 Gigabit Ethernet (GbE) PCI-Express 
Software | macOS 12.6 Monterey, Ventura & Bigsur
BIOS | A16
Serial Port | Disabled
Hyperthreading | Disabled (Enable on single CPU, disable on dual CPU config, more [here](#hyperthreading))
SATA Operation | AHCI
Bluetooth | Orico BCM20702 USB dongle

## What works?

Almost everything except hyperthreading.

## What doesn’t work?

- CPUPM, I haven't added it. You can add it using ssdtprgen. Works.
- Shutdown works, reboot doesn’t. Which might get solved.
- Hyperthreading on dual CPU config

## How To Use

Copy the EFI folder to your EFI partition. That's it.

## GPU specific settings

AMD Polaris and Vega GPU users can skip this portion. For other GPUs, you'll need to add their appropriate boot-args:

| GPU type | boot-args |
|--------|---------|
| NVIDIA Kepler (GTX 600/700 series) (Catalina/Big Sur) | `shikigva=40 agdpmod=vit9696` |
| AMD Navi (RX 5000/6000 series) | `agdpmod=pikera`

## Note for Ventura users (also Monterey with Kepler GPUs)

1. Set `SecureBootModel` to from `Default` to `Disabled`
2. Add `ipc_control_port_options=0 revpatch=sbvmm` and (Ventura only) `amfi_get_out_of_my_way=1` in addition to the aformentioned boot-args
3. Change value of `csr-active-config` to `03080000`
4. Grab [OpenCore Legacy Patcher](https://github.com/dortania/OpenCore-Legacy-Patcher/) and install the Post-Install root patches to add back GPU acceleration!

## RTC

This will cause BIOS to reset after every shutdown. B*tch took long to get fixed. With Bios A16, the RTC fixes are added in config.plist.
However, with a different version of BIOS, the sectors might differ. Either Upgrade/Downgrade to A16 or find bad sectors if bios resets.

## CpusTSCSync

It’s just required for sleep atm. Everything else isn’t affected because of this.
Wait for a newer version, possibly 1.10.0 as current version won’t let you boot.
Sleep is kinda working fine for me. See if you have issues and let me know.

## Hyperthreading

A weird thing, Idk why. If Hyperthreading is enabled with 2x Xeon on T5500, it won't boot. Won't even panic. Tried multiple fixes, some of them are listed below. This issue seems to be High Seirra and up. I haven't tested HS but test and let me know. Tested on Bigsur, Monterey & Ventura but never worked.

- CpuTscsync
- VoodoTscsync
- DSDT edits to change thread counts
- Ssdtprgen
- SSDT LPC 
- TscSyncTimeout
- Different bios settings
- Different SMBIOS

A lot of other things too but don't remember.

## Emulated NVRAM

[Guide](https://dortania.github.io/OpenCore-Post-Install/misc/nvram.html#emulating-nvram-with-a-nvram-plist)

Tested, working.

## How to Install macOS Monterey

There are two ways you can install Monterey:

1. If you have an already working macOS, download the Installer from the App Store and make a bootable Installer with `createinstallmedia` by using this command in Terminal: `sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

2. If you are using Windows, use [macrecovery.py](https://github.com/acidanthera/OpenCorePkg/tree/master/Utilities/macrecovery) from the offical [OpenCore release package](https://github.com/acidanthera/OpenCorePkg/releases/). Follow this [guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html) to understand how it works.

After you have created a bootable Installer, copy the EFI folder to the EFI partition and install as usual. After the installation, mount the EFI partition of the installed OS and copy the EFI folder to its partition.

## How to Install macOS Ventura

There are two ways you can install Ventura:

1. If you have an already working macOS, download the Installer from the App Store and make a bootable Installer with `createinstallmedia` by using this command in Terminal: `sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

2. If you are using Windows, use [macrecovery.py](https://github.com/acidanthera/OpenCorePkg/tree/master/Utilities/macrecovery) from the offical [OpenCore release package](https://github.com/acidanthera/OpenCorePkg/releases/). Follow this [guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html) to understand how it works.

After you have created a bootable Installer, copy the EFI folder to the EFI partition and install as usual. After the installation, mount the EFI partition of the installed OS and copy the EFI folder to its partition. 

### POST INSTALL

You can remove -v from boot-args. It works without verbose.

## Screenshot

![About this Mac](https://i.postimg.cc/28MmT8bd/9a1df918-2e6f-4e81-b82f-89b32046d098-copy.jpg)

## Credits

Thanks to:

- [SkyrilHD](https://github.com/SkyrilHD/) For wasting around 7-10 days to get this POS working. 
- [TECHNIKVERBOT](https://github.com/TECHNIKVERBOT/) For making the EFI, updating it and for the Ventura support.
- [ananjaser1211](https://github.com/ananjaser1211/) For sending sus stickers in chat.
- [MacOS Modded Central Group](https://t.me/hackintoshports/) For having a group to chat in the first place lmao.
- [Me](http://ididnothing.com/).


All this was written on a T5500 with Monterey installed and after kanging [SkyrilHD’s](https://github.com/SkyrilHD/) readme.
