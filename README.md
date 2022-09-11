# Dell Precision T5500 Hackintosh

[![OpenCore Version](https://img.shields.io/badge/OpenCore-0.8.4-green.svg)](https://github.com/qaziabdullah/EFI-Dell-Precision-T5500/)

### This repo contains all the fixes for Dell T5500 with Kepler Card and Xeon X5675 on macOS Monterey.

The EFI folder can also be used on older versions of macOS [tested on Big Sur and Catalina]. However, the main support of this branch is based on Monterey. 


Tested on:

Model | Precision T5500
------------- | ---------------
CPU | Intel Xeon 5675
GPU | Nvidia Quadro 410 GK107 [Kepler]
RAM | 32GB DDR3 1600Mhz
Storage | Liteonit LC-128M6S
Ethernet | Broadcom 5761 Gigabit Ethernet (GbE) PCI-Express 
Software | macOS 12.5.1 Monterey

## What works?

Almost everything.

## What doesn’t work?

1. Sleep is broken due to CPUSTSCSYNC not supported on Monterey. However, it works in Big Sur.
2. USB 1,1. Blame Apple for that.
3. Wired memory on my system stays at 10GB, dunno if that’s an issue but here we have it.
4. Disabled CPUPM, since X5675 doesn’t have any PM SSDT or I was unable to get it working but enabling CPUPM results in low freq and cpu doesn’t go at turbo freq.
5. Shutdown works, reboot doesn’t. Which might get solved.
6. Removing verbose makes it unable to boot I really don’t know why.
7. Haven’t really tested anything other than that which didn’t work so everything works.
8. Could be different for someone else, backup restore doesn’t work for me using time machine.
9. Update from Big Sur doesn’t work either.

## How To Use

Copy the EFI folder to your EFI partition. That's it.

## Kepler Card

To get Kepler working on Monterey, you have to use shikigva=40 and agdpmod=9696.
Those are added in the config.plist.
Otherwise, expect a black display on boot. 
Use OCLP for acceleration as Monterey removed support for Kepler cards.
Ignore this entire section if you have > Kepler card or AMD OOTB supported card.

[OCLP](https://dortania.github.io/OpenCore-Legacy-Patcher/)


## RTC

This will cause BIOS to reset after every shutdown. B*tch took long to get fixed. With Bios A17, the RTC fixes are added in config.plist.
However, with a different version of BIOS, the sectors might differ. Either Upgrade/Downgrade to A17 or find bad sectors if bios resets.


## CpusTSCSync

It’s just required for sleep atm. Everything else isn’t affected because of this.
Wait for a newer version, possibly 1.10.0 as current version won’t let you boot.

## How to Install macOS Monterey

There are two ways you can install Monterey:

1. If you have an already working macOS, download the Installer from the App Store and make a bootable Installer with `createinstallmedia` by using this command in Terminal: `sudo /Applications/Install\ macOS\ Monterey.app/Contents/Resources/createinstallmedia --volume /Volumes/MyVolume`

2. If you are using Windows, use [macrecovery.py](https://github.com/acidanthera/OpenCorePkg/tree/master/Utilities/macrecovery) from the offical [OpenCore release package](https://github.com/acidanthera/OpenCorePkg/releases/). Follow this [guide](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html) to understand how it works.

After you have created a bootable Installer, copy the EFI folder to the EFI partition and install as usual. After the installation, mount the EFI partition of the installed OS and copy the EFI folder to its partition.

### NVIDIA OPTIONAL
When you boot on a kepler card, you will notice no acceleration will be there. 
Complete setup, copy efi, and use OCLP.

## Screenshot

![About this Mac](https://i.postimg.cc/28MmT8bd/9a1df918-2e6f-4e81-b82f-89b32046d098-copy.jpg)


## Credits

Thanks to:

- [SkyrilHD](https://github.com/SkyrilHD/) For wasting around 7-10 days to get this POS working.
- [TECHNIKVERBOT](https://github.com/TECHNIKVERBOT/) For making the EFI, updating it and cleaning up after I f*ck it up.
- [ananjaser1211](https://github.com/ananjaser1211/) For sending sus stickers in chat.
- [MacOS Modded Central Group](https://t.me/hackintoshports/) For having a group to chat in the first place lmao.
- [Me](http://ididnothing.com/).


All this was written on an T5500 with Monterey installed and after kanging [SkyrilHD’s](https://github.com/SkyrilHD/) readme.
