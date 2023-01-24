# Dell Precision T5500 Hackintosh

[![OpenCore Version](https://img.shields.io/badge/OpenCore-0.8.8-green.svg)](https://github.com/qaziabdullah/EFI-Dell-Precision-T5500/)
[![GitHub issues](https://img.shields.io/github/issues/qaziabdullah/EFI-Dell-Precision-T5500.svg)](https://github.com/qaziabdullah/EFI-Dell-Precision-T5500/issues/)

### This repo contains all the fixes for Dell T5500 with Rx570 and Dual Xeon X5675 on macOS Monterey.
#### This should also work on T3500 system but I haven't tested them. Try and let us know.

The EFI folder can also be used on older versions of macOS [tested on Ventura, Bigsur & Catalina]. However, the main support of this branch is based on Monterey since Ventura has a few issues that makes me keep Monterey as the main OS for now. On Ventura, my major issue is no rotation on extended display. 


Tested on:

Model | Precision T5500
------------- | ---------------
CPU | Intel Xeon 5675 x2
GPU | AMD Radeon RX 570 4GB (Sapphire) 
RAM | 32GB DDR3 1600Mhz
Storage | Samsung EVO 870 1TB
Ethernet | Broadcom 5761 Gigabit Ethernet (GbE) PCI-Express 
Software | macOS 12.6 Monterey, Ventura & Bigsur
BIOS | A16
Serial Port | Disabled
Hyperthreading | Disabled (If you are on 1x Xeon, enable HT. If you are on 2x Xeon, disable it otherwise it won't even boot)
SATA Operation | AHCI
Bluetooth | Orico BCM20702 - CSR8510 didn't work

## What works?

Almost everything except hyperthreading.

## What doesn’t work?

1. CPUPM, I haven't added it. You can add it using ssdtprgen. Works.
3. Shutdown works, reboot doesn’t. Which might get solved. 

## How To Use

Copy the EFI folder to your EFI partition. That's it.

## Kepler Card (UPGRADED TO RX570)

To get Kepler working on Monterey, you have to use shikigva=40 and agdpmod=9696. (THIS FIX IS MONTEREY ONLY)
Those are added in the config.plist.
Otherwise, expect a black display on boot. 
Use OCLP for acceleration as Monterey removed support for Kepler cards.
Ignore this entire section if you have AMD OOTB supported card.

Here is [OCLP](https://dortania.github.io/OpenCore-Legacy-Patcher/)
###### INCASE YOU DON'T HAVE KEPLER CARD AND HAVE AN AMD ONE, REMOVE THOSE BOOT-ARGS

## Ventura

RX570 is supported on Ventura after patching with OCLP. Use 03080000 sip to boot Ventura and use OCLP.
Didn't test much.
USB is broken completely, OCLP will fix that too.

## RTC

This will cause BIOS to reset after every shutdown. B*tch took long to get fixed. With Bios A17, the RTC fixes are added in config.plist.
However, with a different version of BIOS, the sectors might differ. Either Upgrade/Downgrade to A16 or find bad sectors if bios resets.


## CpusTSCSync

It’s just required for sleep atm. Everything else isn’t affected because of this.
Wait for a newer version, possibly 1.10.0 as current version won’t let you boot.
Sleep is kinda working fine for me. See if you have issues and let me know.

## Bluetooth

The default SMBIOS is MacPro1,1.
Everything works so no need to change this ig.
Test Bluetooth on Ventura.


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

If you don't have native NVRAM, don't fret. We can set up emulated NVRAM by using a script to save the NVRAM contents to a plist during the shutdown process, which will then be loaded by OpenCore at the next startup.

To enable emulated NVRAM, you'll need the following set:

Within your config.plist:

- Booter -> Quirks:
DisableVariableWrite: set to NO
Misc -> Security:
ExposeSensitiveData: set to at least 0x1
- NVRAM:
LegacyOverwrite set to YES
LegacySchema: NVRAM variables set (OpenCore compares these to the variables present in nvram.plist)
WriteFlash: set to YES

And within your EFI:

OpenVariableRuntimeDxe.efi driver
OpenRuntime.efi driver (this is needed for proper sleep, shutdown and other services to work correctly)
Make sure to snapshot after to make sure the drivers are listed in your config.plist. Afterwards, make sure that both OpenVariableRuntimeDxe.efi and OpenRuntime.efi have LoadEarly set to YES, and that OpenVariableRuntimeDxe.efi is placed before OpenRuntime.efi in your config .

Now grab the LogoutHook folder (opens new window)(inside Utilities) and place it somewhere safe (e.g. within your user directory, as shown below):

/Users/$(whoami)/LogoutHook/

Open up terminal and run the following (one at a time):

cd /Users/$(whoami)/LogoutHook/
./Launchd.command install 


Copied from OpenCore. Tested, working.


## How to Install macOS Monterey

There are two ways you can install Monterey:

1. If you have an already working macOS, download the Installer from the App Store and make a bootable Installer with `createinstallmedia` by using this command in Terminal: `sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

2. If you are using Windows, use [macrecovery.py](https://github.com/acidanthera/OpenCorePkg/tree/master/Utilities/macrecovery) from the offical [OpenCore release package](https://github.com/acidanthera/OpenCorePkg/releases/). Follow this [guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html) to understand how it works.

After you have created a bootable Installer, copy the EFI folder to the EFI partition and install as usual. After the installation, mount the EFI partition of the installed OS and copy the EFI folder to its partition.



## How to Install macOS Ventura

There are two ways you can install Monterey:

1. If you have an already working macOS, download the Installer from the App Store and make a bootable Installer with `createinstallmedia` by using this command in Terminal: `sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

2. If you are using Windows, use [macrecovery.py](https://github.com/acidanthera/OpenCorePkg/tree/master/Utilities/macrecovery) from the offical [OpenCore release package](https://github.com/acidanthera/OpenCorePkg/releases/). Follow this [guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html) to understand how it works.

After you have created a bootable Installer, copy the EFI folder to the EFI partition and install as usual. After the installation, mount the EFI partition of the installed OS and copy the EFI folder to its partition. 


### NVIDIA OPTIONAL (MONTEREY)
When you boot on a kepler card, you will notice no acceleration will be there. 
Complete setup, copy efi, and use OCLP.

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
